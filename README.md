
defaultValues (Optional)
Sets a default value for one or more fields during import.
Used when a specific field is not in the file but a constant value is needed.

Format: Field ID as key and string value.

Example:

json
Copy
Edit
"defaultValues": {
  "123": "TX",
  "124": "USD"
}



reformatAddressesDelimiter (Required if using address block)
Specifies the delimiter used to separate components within an address block.

Common values: "," (comma), "|" (pipe)

Not allowed: space, tab, or NULL

Example address block:
Address 1, Address 2, City, State, Zip, County, Country

reformatNameBlock (Required)
Defines how owner names are formatted in the import file.

"None" – Not using a block (names are in separate fields like First Name, Last Name)

"FirstNameFirst" – Block format: First Name, Last Name, Middle Name, Prefix, Suffix, Title

"LastNameFirst" – Block format: Last Name, First Name, Middle Initial, Prefix, Suffix, Title

reformatNamesDelimiter (Required if using name block)
Specifies the delimiter used within a name block.

Common values: ",", "|"

Not allowed: space, tab, or NULL

searchBusinessNames (Required)
Determines whether to analyze the owner name to decide if it’s a business or an individual.

true – if you want Tracker PRO to determine business vs. individual

false – if you already provide an identifier (SSN, FEIN, TIN) and know the type

searchInactiveForDuplicates (Required)
Indicates whether to check historical/inactive records for duplicate detection.

true – include inactive records in duplicate check

false – only check active records

stageOperation (Required)
Specifies how records should be handled during import staging:

ExcludeAndCommit – Records that fail mandatory validation are excluded; the rest are committed.

ReviewAndCommit – All records (valid and invalid) go to staging for review.

CommitPassedValidations – If no record fails mandatory validation, all are committed. If any record fails, all go to staging.
