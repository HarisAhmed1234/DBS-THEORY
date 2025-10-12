--QUESTION 3
CREATE TABLE Patient (
    Patient_ID NUMBER PRIMARY KEY,
    Name VARCHAR2(100) NOT NULL,
    Gender CHAR(1) CHECK (Gender IN ('M', 'F')),
    DOB DATE,
    Email VARCHAR2(255) UNIQUE,
    Phone VARCHAR2(20),
    Address VARCHAR2(255),
    Username VARCHAR2(50),
    Password VARCHAR2(50)
);

CREATE TABLE Doctor (
    Doctor_ID NUMBER PRIMARY KEY,
    Name VARCHAR2(100),
    Specialization VARCHAR2(100),
    Username VARCHAR2(50),
    Password VARCHAR2(50)
);

CREATE TABLE Appointment (
    Appointment_ID NUMBER PRIMARY KEY,
    Appointment_Date DATE,
    Appointment_Time VARCHAR2(8),
    Status VARCHAR2(20),
    Clinic_Number NUMBER,
    Patient_ID NUMBER,
    Doctor_ID NUMBER
);

CREATE TABLE Prescription (
    Prescription_ID NUMBER PRIMARY KEY,
    Prescription_Date DATE, 
    Doctor_Advice VARCHAR2(4000),
    Followup_Required CHAR(1),
    Patient_ID NUMBER,
    Doctor_ID NUMBER
);

CREATE TABLE Invoice (
    Invoice_ID NUMBER PRIMARY KEY,
    Invoice_Date DATE,
    Amount NUMBER(10,2),
    Payment_Status VARCHAR2(20),
    Payment_Method VARCHAR2(50),
    Patient_ID NUMBER
);

CREATE TABLE Tests (
    Test_ID NUMBER PRIMARY KEY,
    Blood_Test VARCHAR2(255),
    X_Ray VARCHAR2(255),
    MRI VARCHAR2(255),
    CT_Scan VARCHAR2(255),
    Patient_ID NUMBER,
    Doctor_ID NUMBER
);


INSERT INTO Patient (Patient_ID, Name, Gender, DOB, Email, Phone, Address, Username, Password)
VALUES (1, 'Ali Khan', 'M', TO_DATE('1990-05-15', 'YYYY-MM-DD'), 'ali.khan@example.com', '1234567890', '123 Main St, Karachi', 'alikhan', 'pass123');

INSERT INTO Patient (Patient_ID, Name, Gender, DOB, Email, Phone, Address, Username, Password)
VALUES (2, 'Sana Ahmed', 'F', TO_DATE('1985-08-22', 'YYYY-MM-DD'), 'sana.ahmed@example.com', '0987654321', '456 Elm St, Lahore', 'sanaahmed', 'pass456');

INSERT INTO Doctor (Doctor_ID, Name, Specialization, Username, Password)
VALUES (1, 'Dr. Omar Farooq', 'Cardiology', 'omarfarooq', 'docpass1');

INSERT INTO Doctor (Doctor_ID, Name, Specialization, Username, Password)
VALUES (2, 'Dr. Ayesha Malik', 'Neurology', 'ayeshamalik', 'docpass2');

INSERT INTO Appointment (Appointment_ID, Appointment_Date, Appointment_Time, Status, Clinic_Number, Patient_ID, Doctor_ID)
VALUES (1, TO_DATE('2025-09-01', 'YYYY-MM-DD'), '10:00', 'Booked', 101, 1, 1);

INSERT INTO Appointment (Appointment_ID, Appointment_Date, Appointment_Time, Status, Clinic_Number, Patient_ID, Doctor_ID)
VALUES (2, TO_DATE('2025-09-02', 'YYYY-MM-DD'), '14:00', 'Cancelled', 102, 2, 2);

INSERT INTO Appointment (Appointment_ID, Appointment_Date, Appointment_Time, Status, Clinic_Number, Patient_ID, Doctor_ID)
VALUES (3, TO_DATE('2025-09-03', 'YYYY-MM-DD'), '11:30', 'Booked', 101, 2, 1);

INSERT INTO Prescription (Prescription_ID, Prescription_Date, Doctor_Advice, Followup_Required, Patient_ID, Doctor_ID)
VALUES (1, TO_DATE('2025-09-02', 'YYYY-MM-DD'), 'Take aspirin daily', 'Y', 1, 1);

INSERT INTO Prescription (Prescription_ID, Prescription_Date, Doctor_Advice, Followup_Required, Patient_ID, Doctor_ID)
VALUES (2, TO_DATE('2025-09-02', 'YYYY-MM-DD'), 'MRI recommended', 'N', 2, 2);

INSERT INTO Prescription (Prescription_ID, Prescription_Date, Doctor_Advice, Followup_Required, Patient_ID, Doctor_ID)
VALUES (3, TO_DATE('2025-09-03', 'YYYY-MM-DD'), 'Rest and hydration', 'Y', 1, 1);

INSERT INTO Invoice (Invoice_ID, Invoice_Date, Amount, Payment_Status, Payment_Method, Patient_ID)
VALUES (1, TO_DATE('2025-09-01', 'YYYY-MM-DD'), 1500.00, 'Unpaid', 'Credit Card', 1);

INSERT INTO Invoice (Invoice_ID, Invoice_Date, Amount, Payment_Status, Payment_Method, Patient_ID)
VALUES (2, TO_DATE('2025-09-02', 'YYYY-MM-DD'), 2000.00, 'Unpaid', 'Cash', 2);

INSERT INTO Invoice (Invoice_ID, Invoice_Date, Amount, Payment_Status, Payment_Method, Patient_ID)
VALUES (3, TO_DATE('2025-09-01', 'YYYY-MM-DD'), 1000.00, 'Paid', 'Credit Card', 1);

INSERT INTO Tests (Test_ID, Blood_Test, X_Ray, MRI, CT_Scan, Patient_ID, Doctor_ID)
VALUES (1, 'Complete Blood Count', NULL, NULL, NULL, 1, 1);

INSERT INTO Tests (Test_ID, Blood_Test, X_Ray, MRI, CT_Scan, Patient_ID, Doctor_ID)
VALUES (2, NULL, 'Chest X-Ray', NULL, NULL, 2, 2);

INSERT INTO Tests (Test_ID, Blood_Test, X_Ray, MRI, CT_Scan, Patient_ID, Doctor_ID)
VALUES (3, 'Lipid Profile', NULL, NULL, NULL, 1, 1);


 
--DML Queries:
--PART A: Update the phone number and email of a patient in the Patient table.
UPDATE Patient 
SET Phone = '1112223333', Email = 'ali.new@example.com' 
WHERE Patient_ID = 1;
SELECT Patient_ID, Name, Phone, Email FROM Patient WHERE Patient_ID = 1;

--PART B: Update the payment status of an invoice in the Invoice table from "Unpaid" to "Paid".
UPDATE Invoice 
SET Payment_Status = 'Paid' 
WHERE Invoice_ID = 1 AND Payment_Status = 'Unpaid';
SELECT Invoice_ID, Payment_Status FROM Invoice WHERE Invoice_ID = 1;

--PART C: Delete all cancelled appointments from the Appointment table.
DELETE FROM Appointment 
WHERE Status = 'Cancelled';
SELECT * FROM Appointment;

--PART D: Delete an invoice from the Invoice table for a patient who has been refunded.
DELETE FROM Invoice 
WHERE Patient_ID = 1;
SELECT * FROM Invoice;

--PART E: Select all appointments that are still "Booked".
SELECT * FROM Appointment 
WHERE Status = 'Booked';

--PART F: Select all invoices that are "Unpaid".
SELECT * FROM Invoice 
WHERE Payment_Status = 'Unpaid';

--PART G: Select all lab tests of type "Blood Test".
SELECT * FROM Tests 
WHERE Blood_Test IS NOT NULL;

--PART H: Select all prescriptions issued on '2025-09-02'.
SELECT * FROM Prescription 
WHERE Prescription_Date = TO_DATE('2025-09-02', 'YYYY-MM-DD');

--Advance SQL:
--PART A : Show all patients with their doctors booked.
SELECT p.Name AS Patient_Name, d.Name AS Doctor_Name
FROM Patient p
JOIN Appointment a ON p.Patient_ID = a.Patient_ID
JOIN Doctor d ON a.Doctor_ID = d.Doctor_ID
WHERE a.Status = 'Booked';

--PART B: Show all lab tests of patients and the doctor who requested them.
SELECT p.Name AS Patient_Name, d.Name AS Doctor_Name, t.*
FROM Tests t
JOIN Patient p ON t.Patient_ID = p.Patient_ID
JOIN Doctor d ON t.Doctor_ID = d.Doctor_ID;

--PART C: Show prescriptions with medicines only for patients named "Ali Khan".
SELECT pr.*
FROM Prescription pr
JOIN Patient p ON pr.Patient_ID = p.Patient_ID
WHERE p.Name = 'Ali Khan';

--PART D: Show prescriptions with doctors where follow-up is required.
SELECT pr.*, d.Name AS Doctor_Name
FROM Prescription pr
JOIN Doctor d ON pr.Doctor_ID = d.Doctor_ID
WHERE pr.Followup_Required = 'Y';

