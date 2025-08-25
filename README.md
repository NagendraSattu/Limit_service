# lambda_function.py
import os, json, tempfile
import boto3, requests, urllib3

# ---- Config via Lambda environment variables ----
BASE_URL           = os.getenv("BASE_URL", "https://ech-qa.us.comerica.net")
TRACKER_PRO_TOKEN  = os.getenv("TRACKER_PRO_TOKEN")          # "Bearer <token>"
HOLDER_ID          = int(os.getenv("HOLDER_ID", "0"))
FILEMAPPING_ID     = int(os.getenv("FILEMAPPING_ID", "0"))

# Optional: if you set this to "true", we skip SSL checks to avoid cert errors
DISABLE_SSL_VERIFY = os.getenv("DISABLE_SSL_VERIFY", "false").lower() == "true"

VERIFY = False if DISABLE_SSL_VERIFY else True
if DISABLE_SSL_VERIFY:
    urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)

S3 = boto3.client("s3")
HEADERS_JSON = {"accept": "application/json", "Authorization": TRACKER_PRO_TOKEN}

def upload_file(local_path: str) -> str:
    url = f"{BASE_URL}/api/Uploads/Upload/Import"
    with open(local_path, "rb") as f:
        files = {"file": f}
        r = requests.post(url, headers={"Authorization": TRACKER_PRO_TOKEN},
                          files=files, verify=VERIFY, timeout=(10, 120))
    r.raise_for_status()
    js = r.json()
    file_id = js.get("data", {}).get("fileId") or js.get("result", {}).get("fileId")
    if not file_id:
        raise RuntimeError(f"Upload response missing fileId: {js}")
    return file_id

def import_file(file_id: str) -> dict:
    url = f"{BASE_URL}/api/v2.0/Imports/Import"
    body = {
        "duplicateRecord": "AddNewOnly",
        "headerRows": 0,
        "reformatAddresses": True,
        "reformatAddressesDelimiter": ",",
        "reformatNamesBlock": "None",
        "reformatNamesDelimiter": ",",
        "searchBusinessNames": True,
        "searchActInactivesForDuplicates": True,
        "stageOperation": "ReviewAndCommit",
        "defaultHolderId": HOLDER_ID,
        "fileMappingId": FILEMAPPING_ID,
        "ownerUniqueFieldIds": [],
        "propertyUniqueFieldIds": [],
        "uploadFileId": file_id
    }
    r = requests.post(url, headers={**HEADERS_JSON, "Content-Type": "application/json"},
                      json=body, verify=VERIFY, timeout=(10, 300))
    r.raise_for_status()
    return r.json()

def download_from_s3(bucket: str, key: str) -> str:
    # Save under /tmp (Lambda’s writable folder)
    _, ext = os.path.splitext(key)
    fd, local_path = tempfile.mkstemp(prefix="upload_", suffix=ext)
    os.close(fd)
    S3.download_file(bucket, key, local_path)
    return local_path

def lambda_handler(event, context):
    """
    Two ways to invoke:

    1) Direct test (from Console):
       {
         "bucket": "bce-import-dev-123",
         "key": "inbound/LPS_new_data.xlsx"
       }

    2) S3 trigger event (automatic on new file):
       event["Records"][0]["s3"]["bucket"]["name"]
       event["Records"][0]["s3"]["object"]["key"]
    """
    try:
        if "Records" in event:  # S3 trigger
            rec = event["Records"][0]
            bucket = rec["s3"]["bucket"]["name"]
            key = rec["s3"]["object"]["key"]
        else:  # direct invoke
            bucket = event["bucket"]
            key    = event["key"]

        local_path = download_from_s3(bucket, key)
        file_id = upload_file(local_path)
        result  = import_file(file_id)

        ok = (result.get("success") is True) or (not result.get("errors"))
        return {
            "statusCode": 200 if ok else 500,
            "body": json.dumps({
                "message": "Upload & Import completed" if ok else "Import finished with errors",
                "fileId": file_id,
                "result": result
            })
        }
    except Exception as e:
        return {
            "statusCode": 500,
            "body": json.dumps({"error": str(e)})
        }
