# SQL-Queary
Employee Table:
CREATE TABLE Employee_Account (
    Emp_Id VARCHAR(5) PRIMARY KEY,
    Emp_Name VARCHAR(50),
    Department VARCHAR(50),
    City VARCHAR(50),
    Salary INT
);

Project Table:
CREATE TABLE Project_Assignment (
    Emp_Name VARCHAR(50),
    Project_Code VARCHAR(10),
    Project_Type VARCHAR(50),
    City VARCHAR(50)
);
INSERT DATA:Employee Data
INSERT INTO Employee_Account VALUES
('E101', 'Arif', 'IT', 'Dhaka', 85000),
('E102', 'Nusrat', 'HR', 'Rajshahi', 42000),
('E103', 'Tanvir', 'Finance', 'Dhaka', 67000),
('E104', 'Mim', 'IT', 'Chattogram', 95000),
('E105', 'Fahim', 'Marketing', 'Khulna', 38000),
('E106', 'Ayesha', 'Finance', 'Dhaka', 72000),
('E107', 'Rifat', 'IT', 'Rajshahi', 50000),
('E108', 'Samia', 'HR', 'Chattogram', 46000);

INSERT DATA:Project Data
INSERT INTO Project_Assignment VALUES
('Arif', 'P-201', 'AI', 'Dhaka'),
('Nusrat', 'P-202', 'HRMS', 'Rajshahi'),
('Mim', 'P-203', 'Web', 'Chattogram'),
('Fahim', 'P-204', 'Marketing', 'Khulna'),
('Ayesha', 'P-205', 'AI', 'Dhaka'),
('Rakib', 'P-206', 'ERP', 'Sylhet'),
('Samia', 'P-207', 'HRMS', 'Chattogram');


