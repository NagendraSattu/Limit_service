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
