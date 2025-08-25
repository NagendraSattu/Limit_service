import os, sys, json, glob
import requests
from dotenv import load_dotenv
import urllib3

# Disable SSL verification warnings
urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)

load_dotenv()

# ---- Config from .env ----
TOKEN = os.getenv("TRACKER_PRO_TOKEN")
HOLDER_ID = int(os.getenv("HOLDER_ID"))
FILE_MAPPING_ID = int(os.getenv("FILEMAPPING_ID"))
FILE_PATH = "LPS_new_data.xlsx"   # match your actual file
BASE_URL = "https://ech-qa.us.comerica.net"

# ---- Safety checks ----
if not os.path.exists(FILE_PATH):
    print(f"❌ File not found: {FILE_PATH}")
    print("Folder contents:"); [print(" -", p) for p in sorted(glob.glob('*'))]
    sys.exit(1)

HEADERS_JSON = {"accept": "application/json", "Authorization": TOKEN}

def upload_file(file_path: str) -> str:
    url = f"{BASE_URL}/api/Uploads/Upload/Import"
    with open(file_path, "rb") as f:
        files = {"file": f}
        r = requests.post(url, headers={"Authorization": TOKEN}, files=files, verify=False)  # <- SSL disabled
    r.raise_for_status()
    js = r.json()
    return js.get("data", {}).get("fileId") or js.get("result", {}).get("fileId")

def import_file(file_id: str, file_mapping_id: int, holder_id: int) -> dict:
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
        "defaultHolderId": holder_id,
        "fileMappingId": file_mapping_id,
        "ownerUniqueFieldIds": [],
        "propertyUniqueFieldIds": [],
        "uploadFileId": file_id
    }
    r = requests.post(url, headers={**HEADERS_JSON, "Content-Type": "application/json"}, json=body, verify=False)  # <- SSL disabled
    r.raise_for_status()
    return r.json()

if __name__ == "__main__":
    try:
        print("📤 Uploading file...")
        fid = upload_file(FILE_PATH)
        print(f"✅ Uploaded. fileId = {fid}")

        print("📥 Importing file...")
        result = import_file(fid, FILE_MAPPING_ID, HOLDER_ID)
        print(json.dumps(result, indent=2))

        ok = (result.get("success") is True) or (not result.get("errors"))
        print("✅ Import completed successfully" if ok else "❌ Import finished with errors")

    except Exception as e:
        print("❌ Error:", e)
        raise


////////////////////////////////////////////////////////////
