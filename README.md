$env:JAVA_HOME="C:\Program Files\Eclipse Adoptium\jdk-21.0.8.9-hotspot"
$env:Path="$env:JAVA_HOME\bin;$env:Path"

INSERT INTO country_code (two_digit_country_code, country_name, three_digit_country_code_digit, created_by, modified_by)
VALUES 
('US', 'United States', 'USA', 'system', 'system'),
('IN', 'India', 'IND', 'system', 'system'),
('CA', 'Canada', 'CAN', 'system', 'system');
