$env:JAVA_HOME="C:\Program Files\Eclipse Adoptium\jdk-21.0.8.9-hotspot"
$env:Path="$env:JAVA_HOME\bin;$env:Path"

INSERT INTO country_code (
    two_digit_country_code,
    country_name,
    three_digit_country_code_digit
)
VALUES 
('US', 'United States', 'USA'),
('CA', 'Canada', 'CAN');


INSERT INTO dda_account_details (
    dda_account_number,
    dda_account_name,
    dda_lob_code,
    dda_lob_cost_center,
    dda_lob_email,
    dda_state,
    source_system,
    legal_check_type,
    legal_recommendation,
    dda_status
) VALUES
('2000000001', 'Accounts Payable ARP', 'ARP001', 10101, 'arp@comerica.com', 'TX', 'ARP', 1, 'Approved', 'Active'),
('2000000002', 'Core Banking Hogan', 'HOG002', 10102, 'hogan@comerica.com', 'CA', 'Hgn', 2, 'Pending Review', 'Active'),
('2000000003', 'Dash Payments', 'DAS003', 10103, 'dash@comerica.com', 'NY', 'Dash', 1, 'Approved', 'Inactive'),
('2000000004', 'Online BillPay', 'BIL004', 10104, 'billpay@comerica.com', 'FL', 'BPY', 3, 'Rejected', 'Suspended'),
('2000000005', 'Regulatory - Reg E', 'REG005', 10105, 'rege@comerica.com', 'MI', 'RegE', 2, 'Escalated', 'Active');




INSERT INTO check_location (
    owner_id_type,
    owner_id_number,
    business_name,
    lka_payee_name,
    payee_last_name,
    payee_first_name,
    street_address_1,
    city,
    state,
    zipcode,
    bank_code,
    dda_account_number,
    check_number,
    property_code,
    ilm_retention_label
) VALUES 
('S', '1234567890', 'ABC Corp', 'John Doe', 'Doe', 'John', '123 Main St', 'Dallas', 'TX', '75001', '101', '2000000001', 'CHK001', 'PROP001', 'ILM001'),
('I', '9988776655', 'XYZ Inc', 'Jane Smith', 'Smith', 'Jane', '456 Oak St', 'Austin', 'TX', '75002', '102', '2000000002', 'CHK002', 'PROP002', 'ILM002'),
('S', '2233445566', 'DEF LLC', 'Alice Johnson', 'Johnson', 'Alice', '789 Pine St', 'Houston', 'TX', '77001', '103', '2000000003', 'CHK003', 'PROP003', 'ILM003'),
('I', '3344556677', 'GHI Co', 'Bob Brown', 'Brown', 'Bob', '321 Elm St', 'San Antonio', 'TX', '78201', '104', '2000000004', 'CHK004', 'PROP004', 'ILM004'),
('S', '4455667788', 'JKL Ltd', 'Carol White', 'White', 'Carol', '654 Maple St', 'Plano', 'TX', '75093', '105', '2000000005', 'CHK005', 'PROP005', 'ILM005');


INSERT INTO banking_center_details (
    banking_center_number,
    banking_center_name,
    banking_center_status,
    banking_center_state_code,
    banking_center_zip_code,
    banking_center_bank_code
) VALUES 
('BC1001', 'Dallas Main Branch', 'Active', 'TX', '75201', '101'),
('BC1002', 'Austin Central', 'Inactive', 'TX', '73301', '102'),
('BC1003', 'Houston South', 'Active', 'TX', '77002', '103'),
('BC1004', 'San Antonio North', 'Pending', 'TX', '78205', '104'),
('BC1005', 'Plano HQ', 'Active', 'TX', '75093', '105');


INSERT INTO check_details (
    check_location_id,
    bank_code,
    dda_account_number,
    check_number,
    banking_center_number,
    check_purpose,
    check_amount,
    transaction_date,
    check_status,
    initial_payee_name,
    remittee_name,
    check_source,
    check_state
) VALUES
(1, '101', '2000000001', 'CHK001', 'BC1001', 'Vendor Payment', 1000.00, '2025-07-25', 'Issued', 'John Doe', 'John Doe', 'ARP', 'TX'),
(2, '102', '2000000002', 'CHK002', 'BC1002', 'Invoice Refund', 500.00, '2025-07-24', 'Cleared', 'Jane Smith', 'Jane Smith', 'Hogan', 'TX'),
(3, '103', '2000000003', 'CHK003', 'BC1003', 'Payroll', 1500.00, '2025-07-23', 'Pending', 'Alice Johnson', 'Alice Johnson', 'Dash', 'TX'),
(4, '104', '2000000004', 'CHK004', 'BC1004', 'Online Payment', 750.00, '2025-07-22', 'Suspended', 'Bob Brown', 'Bob Brown', 'BillPay', 'TX'),
(5, '105', '2000000005', 'CHK005', 'BC1005', 'Regulatory Refund', 900.00, '2025-07-21', 'Rejected', 'Carol White', 'Carol White', 'Reg-E', 'TX');









