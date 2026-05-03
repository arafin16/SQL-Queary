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

# increase the salary of all employees by 5%
UPDATE Employee_Account
SET Salary = Salary * 1.05;
# Increase the salary by 10% for employees whose salary is greater than 70,000.
UPDATE Employee_Account
SET Salary = Salary * 1.10
WHERE Salary > 70000;
# Increase salary by 8% for employees in the IT department and 5% for all other departments using CASE.
UPDATE Employee_Account
SET Salary = Salary *
CASE 
    WHEN Department = 'IT' THEN 1.08
    ELSE 1.05
END;
# Update the salary to 40,000 for employees working in the Marketing department.
UPDATE Employee_Account
SET Salary = 40000
WHERE Department = 'Marketing';
# Display all employees who work in Dhaka.
SELECT * 
FROM Employee_Account
WHERE City = 'Dhaka';
# Display employee details whose salary is between 45,000 and 80,000.
SELECT * 
FROM Employee_Account
WHERE Salary BETWEEN 45000 AND 80000;
# Display all employees except those working in the IT department.
SELECT *
FROM Employee_Account
WHERE Department <> 'IT';
# Find employees whose salary is greater than the average salary.
SELECT *
FROM Employee_Account
WHERE Salary > (SELECT AVG(Salary) FROM Employee_Account);
# Find employees whose names start with 'R', end with 'D', and have at least 4 characters.
SELECT *
FROM Employee_Account
WHERE Emp_Name LIKE 'R%D'
AND LENGTH(Emp_Name) >= 4;
# Display employee names that have exactly 5 characters.
SELECT *
FROM Employee_Account
WHERE LENGTH(Emp_Name) = 5;


# SUBQUERY QUESTIONS
# Find employees who earn the highest salary in the company.
SELECT *
FROM Employee_Account
WHERE Salary = (SELECT MAX(Salary) FROM Employee_Account);
# Find employees whose salary is greater than the overall average salary.
SELECT *
FROM Employee_Account
WHERE Salary > (SELECT AVG(Salary) FROM Employee_Account);
# Find employees whose salary is greater than the average salary of their own department.
SELECT *
FROM Employee_Account e1
WHERE Salary > (
    SELECT AVG(Salary)
    FROM Employee_Account e2
    WHERE e1.Department = e2.Department
);
# Find departments whose average salary is greater than the overall average salary.
SELECT Department
FROM Employee_Account
GROUP BY Department
HAVING AVG(Salary) > (SELECT AVG(Salary) FROM Employee_Account);
# Find employees whose department has at least one project.
SELECT *
FROM Employee_Account
WHERE Emp_Name IN (
    SELECT Emp_Name FROM Project_Assignment
);
# Find employees whose salary is greater than all salaries of employees in the Finance department.
SELECT *
FROM Employee_Account
WHERE Salary > ALL (
    SELECT Salary
    FROM Employee_Account
    WHERE Department = 'Finance'
);
# Find employees who do NOT have any project assigned.
SELECT *
FROM Employee_Account e
WHERE NOT EXISTS (
    SELECT *
    FROM Project_Assignment p
    WHERE e.Emp_Name = p.Emp_Name
);
# JOIN QUERIES (Employee + Project)
# Display employee names with their project details using INNER JOIN.
SELECT e.Emp_Name, e.Department, p.Project_Code, p.Project_Type
FROM Employee_Account e
INNER JOIN Project_Assignment p
ON e.Emp_Name = p.Emp_Name;
# Display all employees and their projects (include employees without projects) using LEFT JOIN.
SELECT e.Emp_Name, p.Project_Code, p.Project_Type
FROM Employee_Account e
LEFT JOIN Project_Assignment p
ON e.Emp_Name = p.Emp_Name;
# Display all projects including those without matching employees using RIGHT JOIN.
SELECT e.Emp_Name, p.Project_Code, p.Project_Type
FROM Employee_Account e
RIGHT JOIN Project_Assignment p
ON e.Emp_Name = p.Emp_Name;

# Display all employees and all projects (matched or not) using FULL OUTER JOIN.
SELECT e.Emp_Name, p.Project_Code, p.Project_Type
FROM Employee_Account e
LEFT JOIN Project_Assignment p
ON e.Emp_Name = p.Emp_Name

UNION

SELECT e.Emp_Name, p.Project_Code, p.Project_Type
FROM Employee_Account e
RIGHT JOIN Project_Assignment p
ON e.Emp_Name = p.Emp_Name;

# Generate Cartesian product of Employee and Project using CROSS JOIN.
SELECT *
FROM Employee_Account
CROSS JOIN Project_Assignment;
# Display pairs of employees who belong to the same department using SELF JOIN.
SELECT e1.Emp_Name AS Employee1, e2.Emp_Name AS Employee2, e1.Department
FROM Employee_Account e1
JOIN Employee_Account e2
ON e1.Department = e2.Department
AND e1.Emp_Id < e2.Emp_Id;
# Display employee names and project types for employees working in Dhaka.
SELECT e.Emp_Name, p.Project_Type
FROM Employee_Account e
JOIN Project_Assignment p
ON e.Emp_Name = p.Emp_Name
WHERE e.City = 'Dhaka';
## ADVANCED / UNIQUE SQL QUERIES
# Top 2 Highest Paid Employees
SELECT *
FROM Employee_Account
ORDER BY Salary DESC
LIMIT 2;
# Second Highest Salary (INTERVIEW FAMOUS)
SELECT MAX(Salary) AS Second_Highest
FROM Employee_Account
WHERE Salary < (SELECT MAX(Salary) FROM Employee_Account);

# Department-wise Highest Salary
SELECT Department, MAX(Salary) AS Highest_Salary
FROM Employee_Account
GROUP BY Department;
# Employees who earn more than their manager 
SELECT *
FROM Employee_Account e
WHERE Salary > (
    SELECT AVG(Salary)
    FROM Employee_Account
    WHERE Department = e.Department
);
# Employees who have NO project (IMPORTANT)
SELECT *
FROM Employee_Account e
WHERE NOT EXISTS (
    SELECT 1
    FROM Project_Assignment p
    WHERE e.Emp_Name = p.Emp_Name
);
# Employees working on more than 1 project
SELECT Emp_Name, COUNT(*) AS Total_Project
FROM Project_Assignment
GROUP BY Emp_Name
HAVING COUNT(*) > 1;
# Department with highest total salary
SELECT Department
FROM Employee_Account
GROUP BY Department
ORDER BY SUM(Salary) DESC
LIMIT 1;
# Employees whose salary is above department average AND have project
SELECT *
FROM Employee_Account e
WHERE Salary > (
    SELECT AVG(Salary)
    FROM Employee_Account
    WHERE Department = e.Department
)
AND EXISTS (
    SELECT 1
    FROM Project_Assignment p
    WHERE e.Emp_Name = p.Emp_Name
);
# Find duplicate city employees
SELECT City, COUNT(*) AS Total
FROM Employee_Account
GROUP BY City
HAVING COUNT(*) > 1;
# Show salary category (CASE statement 🔥)
SELECT Emp_Name, Salary,
CASE 
    WHEN Salary >= 80000 THEN 'High'
    WHEN Salary >= 50000 THEN 'Medium'
    ELSE 'Low'
END AS Salary_Level
FROM Employee_Account;
# Rank employees by salary (ADVANCED ⭐)
SELECT Emp_Name, Salary,
RANK() OVER (ORDER BY Salary DESC) AS Rank_Position
FROM Employee_Account;
# Employees earning more than ALL HR employees
SELECT *
FROM Employee_Account
WHERE Salary > ALL (
    SELECT Salary
    FROM Employee_Account
    WHERE Department = 'HR'
);
# Find employees working in same city AND same project type
SELECT e.Emp_Name, p.Project_Type, e.City
FROM Employee_Account e
JOIN Project_Assignment p
ON e.Emp_Name = p.Emp_Name
WHERE e.City = p.City;
# Employees whose name length is maximum
SELECT *
FROM Employee_Account
WHERE LENGTH(Emp_Name) = (
    SELECT MAX(LENGTH(Emp_Name))
    FROM Employee_Account
);
