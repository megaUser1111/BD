USE master;
GO
DROP DATABASE IF EXISTS CRD;
GO
CREATE DATABASE CRD;
GO
USE CRD;
GO;

-- Node Tables
CREATE TABLE Clients (
	Client_ID INT PRIMARY KEY,
	Last_Name NVARCHAR(255) NOT NULL,
	First_Name NVARCHAR(255) NOT NULL,
	Middle_Name NVARCHAR(255),
	Date_of_Birth DATE NOT NULL,
	Gender NVARCHAR(10) NOT NULL CHECK (Gender IN ('Male', 'Female')),
	Passport_Details NVARCHAR(255) NOT NULL,
	Registration_Date DATETIME
) AS NODE;

CREATE TABLE Employees (
	Employee_ID INT PRIMARY KEY,
	Last_Name NVARCHAR(255) NOT NULL,
	First_Name NVARCHAR(255) NOT NULL,
	Middle_Name NVARCHAR(255),
	Position NVARCHAR(100) NOT NULL,
	Hire_Date DATETIME
) AS NODE;

CREATE TABLE Tariffs (
	Tariff_ID INT PRIMARY KEY,
	Name NVARCHAR(100) NOT NULL,
	Cost DECIMAL(10, 2) NOT NULL CHECK (Cost > 0)
) AS NODE;
GO

-- Edge Tables
CREATE TABLE Client_Employee (
	Client_ID INT REFERENCES Clients(Client_ID),
	Employee_ID INT REFERENCES Employees(Employee_ID)
) AS EDGE;

CREATE TABLE Employee_Tariff (
	Employee_ID INT REFERENCES Employees(Employee_ID),
	Tariff_ID INT REFERENCES Tariffs(Tariff_ID)
) AS EDGE;

CREATE TABLE Client_Tariff (
	Client_ID INT REFERENCES Clients(Client_ID),
	Tariff_ID INT REFERENCES Tariffs(Tariff_ID)
) AS EDGE;
GO

-- вставка в Node Tables
INSERT INTO Clients (Client_ID, Last_Name, First_Name, Middle_Name, Date_of_Birth, Gender, Passport_Details)
VALUES 
(1, N'Ivanov', N'Ivan', N'Ivanovich', '1980-01-01', N'Male', N'111111111'),
(2, N'Petrova', N'Anna', N'Petrovna', '1990-02-02', N'Female', N'222222222'),
(3, N'Smirnov', N'Alexey', N'Alexeevich', '1985-03-03', N'Male', N'111222333'),
(4, N'Kuznetsova', N'Maria', N'Sergeevna', '1995-04-04', N'Female', N'444555666'),
(5, N'Petrov', N'Petr', N'Petrovich', '1982-05-05', N'Male', N'555666777'),
(6, N'Ivanova', N'Irina', N'Ivanovna', '1992-06-06', N'Female', N'777888999'),
(7, N'Sidorov', N'Sergey', N'Sergeevich', '1987-07-07', N'Male', N'999000111'),
(8, N'Smirnova', N'Svetlana', N'Sergeevna', '1997-08-08', N'Female', N'111222333'),
(9, N'Kuznetsov', N'Konstantin', N'Konstantinovich', '1989-09-09', N'Male', N'333444555'),
(10, N'Ivanov', N'Igor', N'Igorevich', '1999-10-10', N'Male', N'555666777');

INSERT INTO Employees (Employee_ID, Last_Name, First_Name, Middle_Name, Position)
VALUES 
(1, N'Sidorov', N'Sergey', N'Sergeevich', N'Consultant'),
(2, N'Kuznetsova', N'Maria', N'Ivanovna', N'Manager'),
(3, N'Popov', N'Petr', N'Petrovich', N'Consultant'),
(4, N'Sokolova', N'Svetlana', N'Ivanovna', N'Manager'),
(5, N'Petrov', N'Pavel', N'Pavlovich', N'Consultant'),
(6, N'Ivanova', N'Inna', N'Ivanovna', N'Manager'),
(7, N'Sidorov', N'Stanislav', N'Stanislavovich', N'Consultant'),
(8, N'Smirnova', N'Sofia', N'Sergeevna', N'Manager'),
(9, N'Kuznetsov', N'Kirill', N'Kirillovich', N'Consultant'),
(10, N'Ivanov', N'Ilya', N'Igorevich', N'Manager');

INSERT INTO Tariffs (Tariff_ID, Name, Cost)
VALUES 
(1, N'Tariff 1', 100.00),
(2, N'Tariff 2', 200.00),
(3, N'Tariff 3', 300.00),
(4, N'Tariff 4', 400.00),
(5, N'Tariff 5', 500.00),
(6, N'Tariff 6', 600.00),
(7, N'Tariff 7', 700.00),
(8, N'Tariff 8', 800.00),
(9, N'Tariff 9', 900.00),
(10, N'Tariff 10', 1000.00);
GO

-- вставка в Edge Tables
INSERT INTO Client_Employee ($from_id, $to_id)
VALUES 
(1, 1),
(2, 2),
(3, 3),
(4, 4),
(5, 5),
(6, 6),
(7, 7),
(8, 8),
(9, 9),
(10, 10);

INSERT INTO Employee_Tariff ($from_id, $to_id)
VALUES 
(1, 1),
(2, 2),
(3, 3),
(4, 4),
(5, 5),
(6, 6),
(7, 7),
(8, 8),
(9, 9),
(10, 10);

INSERT INTO Client_Tariff ($from_id, $to_id)
VALUES 
(1, 1),
(2, 2),
(3, 3),
(4, 4),
(5, 5),
(6, 6),
(7, 7),
(8, 8),
(9, 9),
(10, 10);
GO

-- Match

SELECT Client_ID, Employee_ID
FROM Client_Employee
WHERE MATCH (Clients-(Client_Employee)->Employees)
AND Employees.Employee_ID = 3;

SELECT Employee_ID, Tariff_ID
FROM Employee_Tariff
WHERE MATCH (Employees-(Employee_Tariff)->Tariffs)
AND Employees.Employee_ID = 3;

SELECT Client_ID, Tariff_ID
FROM Client_Tariff
WHERE MATCH (Clients-(Client_Tariff)->Tariffs)
AND Tariffs.Tariff_ID = 3;

SELECT Client_ID, Employee_ID
FROM Client_Employee
WHERE MATCH (Clients-(Client_Employee)->Employees)
AND Clients.Client_ID = 3;

SELECT Client_ID, Tariff_ID
FROM Client_Tariff
WHERE MATCH (Clients-(Client_Tariff)->Tariffs)
AND Clients.Client_ID = 3;

-- Shortest_Path

SELECT Client_ID, Employee_ID, Tariff_ID
FROM Client_Employee, Employee_Tariff
WHERE MATCH (SHORTEST_PATH(Clients-(Client_Employee)->Employees-(Employee_Tariff)->Tariffs))
AND Clients.Client_ID = 3 AND Tariffs.Tariff_ID = 3;

SELECT Client_ID, Employee_ID, Tariff_ID
FROM Client_Employee, Employee_Tariff
WHERE MATCH (SHORTEST_PATH(Clients-(Client_Employee)->Employees-(Employee_Tariff)->Tariffs){1,3})
AND Clients.Client_ID = 3 AND Tariffs.Tariff_ID = 3;