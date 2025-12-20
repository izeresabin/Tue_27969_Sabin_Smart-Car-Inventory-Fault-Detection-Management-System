# 🚗 Smart Car Inventory and Fault Management System

# project overview
## 📘 Introduction

Car dealerships manage large inventories of vehicles, each of which undergoes multiple inspections, repairs, and evaluations before being approved for sale. Managing these processes manually or without a centralized system can lead to data inconsistencies, delayed fault detection, and inefficient decision-making.

This project presents a **Smart Car Inventory & Fault-Detection Management System** developed using **Oracle Database and PL/SQL** to automate the management of vehicles, inspections, faults, and mechanic activities. The system intelligently identifies and flags vehicles with repeated faults, ensuring improved operational efficiency, enhanced data integrity, and faster, more informed decision-making within a real-world dealership environment.
## 🎯 Project Objectives

- Design and implement a robust Oracle database schema for managing car inventory, inspections, and fault records  
- Automate core dealership operations using PL/SQL procedures, functions, and packages  
- Enforce business rules and data integrity through database triggers and constraints  
- Automatically detect and flag vehicles with repeated or critical faults  
- Maintain comprehensive audit logs for accountability and traceability of system operations  
- Provide analytical insights to support informed and data-driven decision-making  
## 🚀 What Our Solution Delivers

- Centralized management of car inventory, inspections, and fault records  
- Automated detection and flagging of vehicles with repeated or critical faults  
- Enforcement of business rules directly at the database level using PL/SQL triggers  
- Secure and controlled data operations through procedures and packaged logic  
- Comprehensive audit trails to track user actions and system events  
- Reliable, consistent, and scalable data storage aligned with real-world dealership operations
## 🌍 Real-World Impact

- Customers benefit from improved vehicle reliability through early fault detection  
- Dealership staff gain real-time visibility into vehicle status and fault history  
- Mechanics can efficiently track inspections, repairs, and recurring issues  
- Managers access accurate operational and analytical reports for decision-making  
- Auditors can trace all system activities through comprehensive audit logs  
## 🧭 Project Journey – Development Phases

| Phase | Deliverables |
|------:|--------------|
| Phase I | Problem identification and project proposal presentation |
| Phase II | Business process modeling using UML/BPMN diagrams with explanation |
| Phase III | Logical database design including ER diagram and data dictionary |
| Phase IV | Oracle Pluggable Database (PDB) creation and configuration scripts |
| Phase V | Table implementation, constraints, and realistic data insertion |
| Phase VI | PL/SQL development: procedures, functions, packages, and cursors |
| Phase VII | Advanced programming: triggers, business rules, and auditing |
| Phase VIII | Final documentation, Business Intelligence, and presentation |
## 📌 Phase I: Problem Identification & Project Overview

### Overview
This phase focuses on identifying a real-world problem that can be effectively solved using an Oracle Database and PL/SQL–based system. The goal is to clearly define the problem context, target users, and the value of the proposed solution.

### Problem Statement
Car dealerships manage large inventories of vehicles, each undergoing multiple inspections, repairs, and evaluations before being sold. Managing these processes without an automated system leads to data inconsistency, delayed fault detection, and inefficient decision-making.

The proposed system introduces a centralized database solution that automates car inventory management, inspection tracking, and fault detection using PL/SQL.

### Target Users
- Dealership staff responsible for vehicle registration and inspections  
- Mechanics handling repairs and fault resolution  
- Managers overseeing operations and performance  
- Auditors responsible for compliance and accountability  

### Key Outcomes
- Clearly defined system scope and objectives  
- Identification of operational challenges to be addressed  
- Foundation for database design and process modeling in subsequent phases  

## 📌 Phase II: Business Process Modeling

This phase focuses on modeling the core business processes involved in managing car inventory, inspections, fault detection, and repairs. The objective is to clearly illustrate how different actors interact with the system and how data flows from vehicle registration to fault resolution.

### Business Process Diagram (BPMN)
Business Process Diagram:
<img width="1873" height="923" alt="BPMN" src="https://github.com/user-attachments/assets/e27d4685-97ca-4676-9b72-a85476a36492" />


### Diagram Explanation

The BPMN diagram represents the complete operational workflow of the **Smart Car Inventory & Fault-Detection Management System**, clearly showing how vehicles move from registration to final approval for sale, while handling inspections, faults, and repairs.

**Main Components:**  
The process begins with the *Inventory Manager* registering a car and assigning it for inspection. The *Mechanic* performs the inspection and records the result as either pass or fail. If the vehicle passes, it proceeds directly toward being marked as ready for sale. If the vehicle fails, a fault is logged and stored in the database, and the system continues tracking the vehicle’s status.

Fault handling includes recording fault reports, storing status history, and checking for recurring faults. Vehicles with repeated faults are automatically marked with a *critical* status. Repair and re-inspection cycles are modeled using decision gateways, ensuring that a vehicle cannot proceed until it successfully passes inspection.

**MIS Functions:**  
The system supports key Management Information System (MIS) functions, including data capture (inspection results and fault reports), data processing (fault evaluation and recurrence checks), data storage (status history and audit records), and information dissemination (alerts and readiness status). Decision gateways represent system-controlled logic that enforces consistency and operational rules.

**Organizational Impact:**  
By automating inspection workflows and fault detection, the system improves coordination between inventory managers and mechanics, reduces human error, and ensures that unsafe or unreliable vehicles are not released for sale. The clear separation of responsibilities enhances accountability and operational efficiency within the dealership.

**Analytics Opportunities:**  
The modeled process enables analytical insights such as identifying vehicles with frequent faults, analyzing inspection pass/fail rates, monitoring repair effectiveness, and tracking time-to-sale metrics. These analytics support data-driven decision-making and proactive inventory management.

## 📌 Phase III: Logical Database Design

This phase focuses on designing a fully normalized logical data model to support car inventory management, inspections, fault detection, repairs, and status tracking. The design ensures data integrity, scalability, and readiness for PL/SQL automation and Business Intelligence.

### Entity-Relationship Diagram (ERD)
<img width="1025" height="776" alt="ERD" src="https://github.com/user-attachments/assets/32989e65-5133-414d-a97e-683f5c365f1b" />

### ER Diagram Description
The Entity-Relationship Diagram (ERD) illustrates the logical structure of the Smart Car Inventory & Fault-Detection Management System by showing the core entities, their attributes, and how they relate to one another.

A **CAR** can undergo multiple **INSPECTION** records over time, while each inspection is associated with exactly one car. A **MECHANIC** can perform many inspections, but each inspection is carried out by a single mechanic. A **CAR** may have multiple **FAULT_REPORT** entries, capturing all faults identified during inspections. Additionally, every change in a car’s status is recorded in **STATUS_HISTORY**, where one car can have many status history records, ensuring complete traceability of status transitions.


### Key Entities
The logical model includes core entities such as:
- **CAR** – Stores vehicle inventory details  
- **MECHANIC** – Represents personnel performing inspections and repairs  
- **INSPECTION** – Logs inspection activities and outcomes  
- **FAULT_REPORT** – Captures faults detected during inspections  
- **STATUS_HISTORY** – Tracks all car status changes over time  

These entities are connected through well-defined one-to-many relationships to accurately represent real-world dealership operations.

### Normalization
The database schema is normalized to **Third Normal Form (3NF)**:
- Atomic attributes with no repeating groups (1NF)  
- No partial dependencies (2NF)  
- No transitive dependencies (3NF)  

This prevents data redundancy and update anomalies while improving consistency and performance.

### Data Dictionary & Assumptions
A detailed data dictionary documents all tables, attributes, data types, and constraints. Clear assumptions are defined to enforce valid inspection results, fault severity levels, controlled status values, and mandatory status history tracking.

This logical design forms the foundation for physical table implementation, PL/SQL development, auditing, and analytical reporting in later phases.
## 📌 Phase IV: Database Creation & Configuration

This phase focuses on setting up the complete Oracle database environment required for the Smart Car Inventory System. In this phase, a dedicated pluggable database (PDB) is created, custom tablespaces for data, indexes, and temporary operations are configured with autoextend settings, and memory parameters such as SGA and PGA are documented. The archive log status is checked and recorded, and finally, a project-specific user is created and granted the necessary privileges to own and manage all future database objects.


## Pluggable Database Creation & Creating the Admin User and Granting Privileges
### Pluggable Database Creation
A dedicated pluggable database was created to isolate the project environment and support secure development and testing.

```sql
CREATE PLUGGABLE DATABASE TUE_27969_SABIN_SMARTCARINVENTORY_DB
ADMIN USER sabin1 IDENTIFIED BY sabin
FILE_NAME_CONVERT = (
  'C:\APP\HP\PRODUCT\21C\ORADATA\XE\PDBSEED',
  'C:\APP\HP\PRODUCT\21C\ORADATA\XE\TUE_27969_SABIN_SMARTCARINVENTORY_DB'
);

ALTER SESSION SET CONTAINER = TUE_27969_SABIN_SMARTCARINVENTORY_DB;
ALTER PLUGGABLE DATABASE TUE_27969_SABIN_SMARTCARINVENTORY_DB OPEN;
ALTER PLUGGABLE DATABASE TUE_27969_SABIN_SMARTCARINVENTORY_DB SAVE STATE;
```
### Creating the Admin User and Granting Privileges

```sql
CREATE USER izere IDENTIFIED BY sabin;
GRANT CONNECT, RESOURCE, DBA, SYSDBA TO izere;
```
<img width="1224" height="1000" alt="creating pdb,user and granting previleges" src="https://github.com/user-attachments/assets/2a036d0c-8b44-456a-bcb3-4cd3ea285a4f" />

### Creating Tablespaces (DATA, INDEX, TEMP)

```sql
-- DATA tablespace
CREATE TABLESPACE smartcar_data  
DATAFILE 'C:\APP\HP\PRODUCT\21C\ORADATA\XE\TUE_27969_SABIN_SMARTCARINVENTORY_DB\smartcar_data01.dbf'  
SIZE 100M  
AUTOEXTEND ON NEXT 10M MAXSIZE UNLIMITED;

-- INDEX tablespace
CREATE TABLESPACE smartcar_index  
DATAFILE 'C:\APP\HP\PRODUCT\21C\ORADATA\XE\TUE_27969_SABIN_SMARTCARINVENTORY_DB\smartcar_index01.dbf'  
SIZE 50M  
AUTOEXTEND ON NEXT 10M MAXSIZE UNLIMITED;

-- TEMP tablespace
CREATE TEMPORARY TABLESPACE smartcar_temp  
TEMPFILE 'C:\APP\HP\PRODUCT\21C\ORADATA\XE\TUE_27969_SABIN_SMARTCARINVENTORY_DB\smartcar_temp01.dbf'  
SIZE 50M  
AUTOEXTEND ON NEXT 5M MAXSIZE UNLIMITED;
```
<img width="1019" height="412" alt="tablespace creation" src="https://github.com/user-attachments/assets/3f0b5d52-a613-4907-b036-2b88a56a760b" />

### TABLESPACES IN OEM
<img width="1905" height="908" alt="OEM TABLESPACE" src="https://github.com/user-attachments/assets/796d81f5-0019-428f-890c-d669591514f8" />

### Enabling Archive Logging

```sql
SHUTDOWN IMMEDIATE;
STARTUP MOUNT;
ALTER DATABASE ARCHIVELOG;
ALTER DATABASE OPEN;
```
<img width="756" height="824" alt="archive logging enabled" src="https://github.com/user-attachments/assets/d20d3cd5-40df-4a1d-a4db-46b0f53fcdb0" />

### Memory Parameters
```sql
SHOW PARAMETER sga_target;
SHOW PARAMETER pga_aggregate_target;
SHOW PARAMETER memory_target;
```
<img width="1158" height="348" alt="Memory parameters (SGA, PGA)" src="https://github.com/user-attachments/assets/33a867f4-07f3-4180-ba7b-21c43578c778" />

### Phase IV Completion Summary
At the end of this phase, a fully functional Oracle Database Pluggable Database has been created and configured with dedicated tablespaces, archive logging enabled, documented memory parameters, and an administrative user ready to support all subsequent phases of the Smart Car Inventory System.

# Phase V – Table Implementation & Data Insertion  

## Phase Overview
This phase focuses on implementing the **physical database structure** for the Smart Car Inventory System. All entities identified in the logical data model are translated into Oracle tables with appropriate data types, primary keys, foreign keys, constraints, and default values. These tables support vehicle registration, inspections, fault reporting, mechanic assignments, and historical tracking of car status changes.

---

## 📊 Database Tables Used

| Table Name       | Description |
|-----------------|-------------|
| **CAR** | Stores registered vehicles including model, brand, year, operational status, and registration date. |
| **MECHANIC** | Stores mechanics responsible for inspecting and repairing vehicles, including their specialization. |
| **INSPECTION** | Records inspection details for cars, including inspection date, assigned mechanic, results, and notes. |
| **FAULT_REPORT** | Stores reported vehicle faults with severity level and reporting date. |
| **STATUS_HISTORY** | Tracks historical changes in vehicle status over time for auditing and analysis purposes. |

## A. Table Creation

### CAR Table
```sql
CREATE TABLE car (
    car_id          NUMBER          PRIMARY KEY,
    model           VARCHAR2(50)    NOT NULL,
    brand           VARCHAR2(50)    NOT NULL,
    year            NUMBER(4)       CHECK (year >= 1980),
    status          VARCHAR2(30)    CHECK (status IN ('ACTIVE','IN_REPAIR','DISABLED')),
    date_registered DATE            DEFAULT SYSDATE
);
````
<img width="1862" height="992" alt="table car creation" src="https://github.com/user-attachments/assets/6cf6edb6-f296-48c0-8aac-9de939ad9d40" />

### MECHANIC Table

```sql
CREATE TABLE mechanic (
    mechanic_id    NUMBER          PRIMARY KEY,
    name           VARCHAR2(50)    NOT NULL,
    specialization VARCHAR2(50)    CHECK (specialization IN ('Engine','Electrical','Bodywork','Diagnostics'))
);
```
<img width="1896" height="988" alt="table mechanic creation" src="https://github.com/user-attachments/assets/fd5d22fd-35f8-4e01-a808-c757dfbf76b8" />

### INSPECTION Table

```sql
CREATE TABLE inspection (
    inspection_id   NUMBER          PRIMARY KEY,
    car_id          NUMBER          NOT NULL,
    mechanic_id     NUMBER          NOT NULL,
    inspection_date DATE            NOT NULL,
    notes           VARCHAR2(200),
    result          VARCHAR2(20)    CHECK (result IN ('PASS','FAIL')),
    CONSTRAINT fk_inspection_car
        FOREIGN KEY (car_id) REFERENCES car(car_id),
    CONSTRAINT fk_inspection_mechanic
        FOREIGN KEY (mechanic_id) REFERENCES mechanic(mechanic_id)
);
```
<img width="1898" height="992" alt="table inspection creation" src="https://github.com/user-attachments/assets/cb5cbb68-970d-4025-bd20-7ec7b0b6e8fe" />

### FAULT_REPORT Table

```sql
CREATE TABLE fault_report (
    fault_id       NUMBER          PRIMARY KEY,
    car_id         NUMBER          NOT NULL,
    fault_desc     VARCHAR2(200)   NOT NULL,
    severity       VARCHAR2(20)    CHECK (severity IN ('MINOR','MAJOR','CRITICAL')),
    date_reported  DATE            DEFAULT SYSDATE,
    CONSTRAINT fk_fault_car
        FOREIGN KEY (car_id) REFERENCES car(car_id)
);
```
<img width="1885" height="1010" alt="table fault report creation" src="https://github.com/user-attachments/assets/3d016f82-c6c3-4818-9cd9-fd5b44d9824f" />

### STATUS_HISTORY Table

```sql
CREATE TABLE status_history (
    history_id   NUMBER          PRIMARY KEY,
    car_id       NUMBER          NOT NULL,
    old_status   VARCHAR2(30)    CHECK (old_status IN ('ACTIVE','IN_REPAIR','DISABLED')),
    new_status   VARCHAR2(30)    CHECK (new_status IN ('ACTIVE','IN_REPAIR','DISABLED')),
    change_date  DATE            DEFAULT SYSDATE,
    CONSTRAINT fk_status_car
        FOREIGN KEY (car_id) REFERENCES car(car_id)
);
```
<img width="1908" height="980" alt="tables status history created" src="https://github.com/user-attachments/assets/81357720-70ae-4dae-955c-02ef99077ed8" />

## B. Data Insertion

The following SQL statements insert realistic and edge-case test data into all tables of the Smart Car Inventory System.  

```sql
----- DATA INSERTION ------------

------ MECHANIC ------------
INSERT INTO mechanic VALUES (1, 'John Mukasa', 'Engine');
INSERT INTO mechanic VALUES (2, 'Alice Karemera', 'Electrical');
INSERT INTO mechanic VALUES (3, 'Samuel Mugenzi', 'Bodywork');
INSERT INTO mechanic VALUES (4, 'Fiona Uwase', 'Diagnostics');
-- Edge case: NULL specialization allowed
INSERT INTO mechanic VALUES (5, 'Patrick Nshuti', NULL);


-------------- CAR -------------
-- Valid normal entries
INSERT INTO car VALUES (101, 'Corolla', 'Toyota', 2018, 'ACTIVE', SYSDATE);
INSERT INTO car VALUES (102, 'Civic', 'Honda', 2020, 'IN_REPAIR', SYSDATE);

-- Edge case: older valid year
INSERT INTO car VALUES (103, 'Pajero', 'Mitsubishi', 1985, 'DISABLED', SYSDATE);

-- Edge case: rare brand + active status
INSERT INTO car VALUES (104, 'Model S', 'Tesla', 2021, 'ACTIVE', SYSDATE);

-- Edge case: NULL date_registered (allowed)
INSERT INTO car VALUES (105, 'A4', 'Audi', 2019, 'IN_REPAIR', NULL);


-------------- INSPECTION ---------------
INSERT INTO inspection 
VALUES (1001, 101, 1, DATE '2024-01-10', 'Routine engine check', 'PASS');

INSERT INTO inspection 
VALUES (1002, 102, 2, DATE '2024-02-15', 'Electrical diagnostics', 'FAIL');

-- Edge case: NULL notes
INSERT INTO inspection 
VALUES (1003, 103, 3, DATE '2024-03-05', NULL, 'FAIL');

-- Edge case: PASS with special characters
INSERT INTO inspection 
VALUES (1004, 104, 4, DATE '2024-03-20', 'Full scan – no issues found', 'PASS');

-- Edge case: future scheduled inspection
INSERT INTO inspection 
VALUES (1005, 105, 5, DATE '2025-01-10', 'Pre-scheduled annual inspection', 'PASS');


-------------- FAULT_REPORT -------------
INSERT INTO fault_report 
VALUES (5001, 102, 'Alternator failure', 'MAJOR', SYSDATE);

INSERT INTO fault_report 
VALUES (5002, 103, 'Front bumper dent', 'MINOR', SYSDATE);

-- Edge case: CRITICAL severity
INSERT INTO fault_report 
VALUES (5003, 104, 'Brake system malfunction', 'CRITICAL', SYSDATE);

-- Edge case: long description
INSERT INTO fault_report 
VALUES (5004, 105, 'Battery voltage irregularities detected overnight', 'MAJOR', SYSDATE);

-- Edge case: DEFAULT SYSDATE
INSERT INTO fault_report 
VALUES (5005, 101, 'Oil leak under engine bay', 'MINOR', NULL);


-------------- STATUS_HISTORY ------------
INSERT INTO status_history 
VALUES (7001, 102, 'ACTIVE', 'IN_REPAIR', SYSDATE);

INSERT INTO status_history 
VALUES (7002, 103, 'IN_REPAIR', 'DISABLED', SYSDATE);

-- Edge case: old_status equals new_status
INSERT INTO status_history 
VALUES (7003, 104, 'ACTIVE', 'ACTIVE', SYSDATE);

-- Edge case: back-and-forth update
INSERT INTO status_history 
VALUES (7004, 105, 'IN_REPAIR', 'ACTIVE', SYSDATE);

-- Edge case: DEFAULT SYSDATE
INSERT INTO status_history 
VALUES (7005, 101, 'ACTIVE', 'IN_REPAIR', NULL);

COMMIT;
```

## C. Data Validation and Integrity Checks

### C.1 Primary Key Uniqueness Validation
This validation ensures that primary key values are unique and no duplicate records exist.

```sql
SELECT mechanic_id, COUNT(*) 
FROM mechanic 
GROUP BY mechanic_id 
HAVING COUNT(*) > 1;

SELECT car_id, COUNT(*) 
FROM car 
GROUP BY car_id 
HAVING COUNT(*) > 1;
````
<img width="1906" height="1021" alt="mechanic id uniqueness validation" src="https://github.com/user-attachments/assets/8bbaaa66-d54f-4e43-8c67-e20fc66faa62" />



---

### C.2 Domain Constraint Validation

This validation checks whether column values respect defined CHECK constraints.

```sql
SELECT * 
FROM car
WHERE status NOT IN ('ACTIVE','IN_REPAIR','DISABLED');

SELECT * 
FROM fault_report
WHERE severity NOT IN ('MINOR','MAJOR','CRITICAL');

SELECT * 
FROM car
WHERE year < 1980;
```
<img width="1919" height="1020" alt="car id uniqueness validation" src="https://github.com/user-attachments/assets/3854d5ce-a6f3-450e-b27a-4985d89900ce" />


---

### C.3 Foreign Key Integrity Validation

This validation verifies that all foreign key references correctly match existing parent records.

```sql
SELECT i.inspection_id
FROM inspection i
LEFT JOIN car c ON i.car_id = c.car_id
WHERE c.car_id IS NULL;

SELECT i.inspection_id
FROM inspection i
LEFT JOIN mechanic m ON i.mechanic_id = m.mechanic_id
WHERE m.mechanic_id IS NULL;
```
<img width="1919" height="1017" alt="foreign key data integrity validation" src="https://github.com/user-attachments/assets/f3cf25f7-ee11-428c-b2cd-4a207c945364" />

### C.4 Data Completeness Check
This validation ensures that all tables contain records and confirms successful data insertion across the database.

```sql
SELECT 'MECHANIC' AS table_name, COUNT(*) FROM mechanic
UNION ALL
SELECT 'CAR', COUNT(*) FROM car
UNION ALL
SELECT 'INSPECTION', COUNT(*) FROM inspection
UNION ALL
SELECT 'FAULT_REPORT', COUNT(*) FROM fault_report
UNION ALL
SELECT 'STATUS_HISTORY', COUNT(*) FROM status_history;
```
<img width="1894" height="1019" alt="data completeness check" src="https://github.com/user-attachments/assets/e306e890-0fca-43ea-80a5-bf30231fd174" />
Perfect — these are **strong testing queries** and exactly what lecturers expect to see.
Below is your **Testing Queries section**, **cleanly structured**, with **clear titles per category**, and **ready for screenshots**.


## D. Testing and Query Verification

### D.1 Basic Data Retrieval Queries
These queries verify that data can be retrieved successfully from each table.

```sql
SELECT * FROM car;
SELECT * FROM mechanic;
SELECT * FROM inspection;
SELECT * FROM fault_report;
SELECT * FROM status_history;
````
example: car
<img width="1918" height="1017" alt="test car" src="https://github.com/user-attachments/assets/441ffdb7-ab99-423f-89c0-b219dcae6188" />


### D.2 Multi-Table Join Queries

These queries validate relationships between tables using JOIN operations.

```sql
SELECT c.car_id, c.model, c.brand, i.inspection_date, i.result
FROM car c
JOIN inspection i ON c.car_id = i.car_id;

SELECT m.name, COUNT(i.inspection_id) AS total_inspections
FROM mechanic m
LEFT JOIN inspection i ON m.mechanic_id = i.mechanic_id
GROUP BY m.name;
```
<img width="1917" height="1018" alt="number of inspections per mechanic" src="https://github.com/user-attachments/assets/9faccd94-6356-40ce-8883-9a2f10100b9b" />


### D.3 Subquery Testing

This query demonstrates the use of subqueries to filter records based on conditions from related tables.

```sql
-- Cars that have failed inspection
SELECT *
FROM car
WHERE car_id IN (
    SELECT car_id
    FROM inspection
    WHERE result = 'FAIL'
);
```
<img width="1914" height="1019" alt="subqueries test" src="https://github.com/user-attachments/assets/b17b74e1-15f8-4105-af93-3996681ebfe0" />

# Phase VI – Procedures, Functions, Cursors, Packages & Exception Handling  

## Phase Overview
This phase demonstrates the implementation of advanced **PL/SQL programming constructs** to enforce business rules, automate operations, validate data, and handle exceptions within the Smart Car Inventory and Fault Management System.

---

## A. Stored Procedures

Stored procedures encapsulate reusable database logic, improve security, and ensure consistency when performing database operations.

### A.1 Procedure: Add a New Car
**Purpose:**  
Inserts a new car into the system using input parameters while handling duplicate keys and invalid data.

```sql
--PROCEDURE 1:Add a New Car (INSERT + IN parameters + exception handling)

CREATE OR REPLACE PROCEDURE add_car (
    p_car_id        IN NUMBER,
    p_model         IN VARCHAR2,
    p_brand         IN VARCHAR2,
    p_year          IN NUMBER,
    p_status        IN VARCHAR2,
    p_date_registered IN DATE DEFAULT SYSDATE
)
IS
BEGIN
    INSERT INTO car (car_id, model, brand, year, status, date_registered)
    VALUES (p_car_id, p_model, p_brand, p_year, p_status, p_date_registered);

    DBMS_OUTPUT.PUT_LINE('Car inserted successfully: ' || p_car_id);

EXCEPTION
    WHEN DUP_VAL_ON_INDEX THEN
        DBMS_OUTPUT.PUT_LINE('Error: Car ID already exists.');
    WHEN VALUE_ERROR THEN
        DBMS_OUTPUT.PUT_LINE('Error: Invalid data type provided.');
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('Unexpected error: ' || SQLERRM);
END;
/
```
<img width="1915" height="1020" alt="procedure1 add a new car" src="https://github.com/user-attachments/assets/0a2fdfc2-627a-497b-b525-2b738717ffdb" />


### A.2 Procedure: Update Car Status
**Purpose:**  
Updates the status of a car and records the change in the status history table.

```sql
--PROCEDURE 2: Update Car Status (UPDATE + IN / OUT parameter + validation + exception handling)

CREATE OR REPLACE PROCEDURE update_car_status (
    p_car_id    IN NUMBER,
    p_new_status IN VARCHAR2,
    p_old_status OUT VARCHAR2
)
IS
BEGIN
    -- Fetch old status
    SELECT status INTO p_old_status 
    FROM car 
    WHERE car_id = p_car_id;

    -- Update to new status
    UPDATE car
    SET status = p_new_status
    WHERE car_id = p_car_id;

    -- Log into status history
    INSERT INTO status_history (history_id, car_id, old_status, new_status, change_date)
    VALUES (status_history_seq.NEXTVAL, p_car_id, p_old_status, p_new_status, SYSDATE);

    DBMS_OUTPUT.PUT_LINE('Status updated from ' || p_old_status || ' to ' || p_new_status);

EXCEPTION
    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE('Error: Car ID does not exist.');
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('Unexpected error: ' || SQLERRM);
END;
/
```
<img width="1917" height="1022" alt="PROCEDURE 2 Update Car Status" src="https://github.com/user-attachments/assets/99b5a09a-9a97-4300-94be-2718da674c1d" />


### A.3 Procedure: Delete Mechanic Safely
**Purpose:**  
Deletes a mechanic only if they are not referenced by inspection records.

```sql
--PROCEDURE 3: Delete Mechanic (DELETE + IN OUT parameter + exception handling + business check)

CREATE OR REPLACE PROCEDURE delete_mechanic (
    p_mechanic_id IN NUMBER,
    p_rows_deleted OUT NUMBER
)
IS
    v_count NUMBER;
BEGIN
    -- Check if mechanic is referenced in inspections
    SELECT COUNT(*) INTO v_count
    FROM inspection
    WHERE mechanic_id = p_mechanic_id;

    IF v_count > 0 THEN
        DBMS_OUTPUT.PUT_LINE('Error: Mechanic is linked to inspections. Cannot delete.');
        p_rows_deleted := 0;
        RETURN;
    END IF;

    -- Delete mechanic
    DELETE FROM mechanic
    WHERE mechanic_id = p_mechanic_id;

    p_rows_deleted := SQL%ROWCOUNT;

    DBMS_OUTPUT.PUT_LINE('Mechanic deleted. Rows affected: ' || p_rows_deleted);

EXCEPTION
    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE('Error: Mechanic not found.');
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('Unexpected error: ' || SQLERRM);
END;
/
```
<img width="1915" height="1017" alt="PROCEDURE 3 Delete Mechanic" src="https://github.com/user-attachments/assets/b3f55fc8-7773-4e17-8c1a-7e6e98c5a356" />


## B. Functions

Functions return single values and are used for validation, calculation, and data lookup.

### B.1 Function: Calculate Car Age in Days
**Purpose:**  
Calculates the number of days a car has been registered in the system.

```sql
--(Calculate how many days a car has been in the system)
CREATE OR REPLACE FUNCTION get_car_age_days (
    p_car_id IN NUMBER
) RETURN NUMBER
IS
    v_days NUMBER;
BEGIN
    SELECT TRUNC(SYSDATE - date_registered)
    INTO v_days
    FROM car
    WHERE car_id = p_car_id;

    RETURN v_days;

EXCEPTION
    WHEN NO_DATA_FOUND THEN
        RETURN NULL; -- car not found
    WHEN OTHERS THEN
        RETURN NULL;
END;
/
```
<img width="1917" height="1015" alt="FUNCTION 1  Calculation Function" src="https://github.com/user-attachments/assets/ce1a8e78-17bc-48d0-b197-f63b174fb95f" />


### B.2 Function: Validate Car Status
**Purpose:**  
Validates whether a given car status is allowed.

```sql
CREATE OR REPLACE FUNCTION is_valid_status (
    p_status IN VARCHAR2
) RETURN VARCHAR2
IS
BEGIN
    IF p_status IN ('ACTIVE', 'IN_REPAIR', 'DISABLED') THEN
        RETURN 'VALID';
    ELSE
        RETURN 'INVALID';
    END IF;

END;
/
```
<img width="1915" height="1017" alt="FUNCTION 2 — Validation Function" src="https://github.com/user-attachments/assets/c7ae9f77-1af2-4eea-8a5d-13a0d834e1c9" />


### B.3 Function: Get Mechanic Name
**Purpose:**  
Returns the name of a mechanic using their ID.

```sql
CREATE OR REPLACE FUNCTION get_mechanic_name (
    p_mechanic_id IN NUMBER
) RETURN VARCHAR2
IS
    v_name mechanic.name%TYPE;
BEGIN
    SELECT name INTO v_name
    FROM mechanic
    WHERE mechanic_id = p_mechanic_id;

    RETURN v_name;

EXCEPTION
    WHEN NO_DATA_FOUND THEN
        RETURN 'MECHANIC NOT FOUND';
    WHEN OTHERS THEN
        RETURN 'ERROR';
END;
/
```
<img width="1918" height="1019" alt="FUNCTION 3 — Lookup Function" src="https://github.com/user-attachments/assets/e8b57611-74c1-4535-95d6-b5860b28ebc7" />


### B.4 Function: Check for Critical Faults
**Purpose:**  
Checks whether a car has any critical faults recorded.

```sql
CREATE OR REPLACE FUNCTION has_critical_fault (
    p_car_id IN NUMBER
) RETURN VARCHAR2
IS
    v_count NUMBER;
BEGIN
    SELECT COUNT(*) INTO v_count
    FROM fault_report
    WHERE car_id = p_car_id
      AND severity = 'CRITICAL';

    IF v_count > 0 THEN
        RETURN 'YES';
    ELSE
        RETURN 'NO';
    END IF;

EXCEPTION
    WHEN OTHERS THEN
        RETURN 'ERROR';
END;
/
```
<img width="1918" height="1022" alt="FUNCTION 4 — Lookup + Validation" src="https://github.com/user-attachments/assets/c408551e-e91f-41cf-9d38-7034ec517af0" />


## C. Cursors

Cursors are used for row-by-row processing of multiple records.

### C.1 Explicit Cursor – Cars Under Repair
**Purpose:**  
Lists all cars currently marked as under repair.

```sql
DECLARE
    CURSOR cur_cars_repairs IS
        SELECT car_id, model, brand
        FROM car
        WHERE status = 'IN_REPAIR';

    v_car_id   car.car_id%TYPE;
    v_model    car.model%TYPE;
    v_brand    car.brand%TYPE;
BEGIN
    OPEN cur_cars_repairs;

    LOOP
        FETCH cur_cars_repairs INTO v_car_id, v_model, v_brand;
        EXIT WHEN cur_cars_repairs%NOTFOUND;

        DBMS_OUTPUT.PUT_LINE(
            'Car: ' || v_car_id || ' | ' || v_model || ' | ' || v_brand
        );
    END LOOP;

    CLOSE cur_cars_repairs;
END;
/
```
<img width="1915" height="1020" alt="CURSOR 1 — Explicit Cursor (Multi-row Processing)" src="https://github.com/user-attachments/assets/7f77f672-e61e-4852-84fe-1668d1d9a092" />


### C.2 Parameterized Cursor – Inspections by Mechanic
**Purpose:**  
Displays all inspections performed by a specific mechanic.

```sql
DECLARE
    CURSOR cur_inspections_by_mech (p_mech_id NUMBER) IS
        SELECT inspection_id, car_id, result
        FROM inspection
        WHERE mechanic_id = p_mech_id;

    v_insp_id inspection.inspection_id%TYPE;
    v_car_id  inspection.car_id%TYPE;
    v_result  inspection.result%TYPE;
BEGIN
    OPEN cur_inspections_by_mech(2);  -- Example: mechanic_id = 2

    LOOP
        FETCH cur_inspections_by_mech INTO v_insp_id, v_car_id, v_result;
        EXIT WHEN cur_inspections_by_mech%NOTFOUND;

        DBMS_OUTPUT.PUT_LINE(
            'Inspection: ' || v_insp_id ||
            '  | Car: ' || v_car_id ||
            '  | Result: ' || v_result
        );
    END LOOP;

    CLOSE cur_inspections_by_mech;
END;
/
```
<img width="1919" height="1020" alt="CURSOR 2 — Parameterized Cursor" src="https://github.com/user-attachments/assets/230be494-38b0-44b0-b4e1-c2758d80f120" />


### C.3 Bulk Operations – Status History Logging
**Purpose:**  
Performs bulk insertion into the status history table for all active cars.

```sql
DECLARE
    TYPE car_list IS TABLE OF car.car_id%TYPE;
    v_car_ids car_list;

BEGIN
    -- BULK COLLECT: get all active cars
    SELECT car_id
    BULK COLLECT INTO v_car_ids
    FROM car
    WHERE status = 'ACTIVE';

    -- FORALL: Insert history for all cars in bulk
    FORALL i IN 1 .. v_car_ids.COUNT
        INSERT INTO status_history (
            history_id, car_id, old_status, new_status, change_date
        )
        VALUES (
            status_history_seq.NEXTVAL,
            v_car_ids(i),
            'ACTIVE',
            'ACTIVE',
            SYSDATE
        );

    DBMS_OUTPUT.PUT_LINE('Bulk history insert completed for ' || v_car_ids.COUNT || ' cars.');
END;
/
```
<img width="1919" height="1022" alt="CURSOR 3 — BULK OPERATION (BULK COLLECT + FORALL)" src="https://github.com/user-attachments/assets/1cf69053-70fb-4ab6-a960-ca698c5eca83" />


## D. Window Functions

Window functions are used for analytical and reporting purposes.

**Functions Used:**
- ROW_NUMBER()
```sql
1. ROW_NUMBER():Rank cars by year (newest to oldest)
SELECT 
    car_id,
    model,
    brand,
    year,
    ROW_NUMBER() OVER (ORDER BY year DESC) AS row_num
FROM car;
```
  
<img width="1917" height="1017" alt="1  ROW_NUMBER()" src="https://github.com/user-attachments/assets/5492e45f-6670-4cb7-963f-7fb72a1af923" />
 
- RANK()
```sql
RANK():Rank cars by manufacturing year (ties get same rank)
SELECT 
    car_id,
    model,
    brand,
    year,
    RANK() OVER (ORDER BY year DESC) AS year_rank
FROM car;
```
  <img width="1919" height="1020" alt="2  RANK()" src="https://github.com/user-attachments/assets/9bf918c3-8211-466c-a703-03684803c35b" />
 
- DENSE_RANK()
```sql
DENSE_RANK():Rank severity levels across fault reports
SELECT 
    car_id,
    severity,
    DENSE_RANK() OVER (ORDER BY severity DESC) AS severity_rank
FROM fault_report;
```
  <img width="1919" height="1018" alt="3  DENSE_RANK()" src="https://github.com/user-attachments/assets/479a625d-618e-4b2d-b0b7-edabad20e141" />
 
- LAG()
```sql
LAG():Compare a car’s current status with the previous status change
SELECT
    car_id,
    old_status,
    new_status,
    change_date,
    LAG(new_status, 1) OVER (PARTITION BY car_id ORDER BY change_date)
        AS previous_status
FROM status_history;
```
  <img width="1919" height="1019" alt="4  LAG()" src="https://github.com/user-attachments/assets/f17e6b14-d8fc-43cc-86ff-372862a944e3" />

- LEAD()
```sql
LEAD():Show the next scheduled status changeSELECT
    SELECT
    car_id,
    new_status,
    change_date,
    LEAD(new_status, 1) OVER (PARTITION BY car_id ORDER BY change_date)
        AS next_status
FROM status_history;
```
  <img width="1914" height="1011" alt="5  LEAD()" src="https://github.com/user-attachments/assets/4e5345ad-8103-4e0c-b1a8-3dae45e43254" />
 
- PARTITION BY + ORDER BY
```sql
PARTITION BY + ORDER BY : Show each mechanic’s inspection order per car
SELECT
    mechanic_id,
    inspection_id,
    car_id,
    inspection_date,
    ROW_NUMBER() OVER (
        PARTITION BY mechanic_id ORDER BY inspection_date
    ) AS mechanic_inspection_order
FROM inspection;
```
  <img width="1919" height="1022" alt="6  PARTITION BY + ORDER BY" src="https://github.com/user-attachments/assets/653c1733-e4cb-46eb-a49d-00f84f9f727e" />

-  Aggregate With OVER()
  ```sql
Aggregate With OVER(): Count total inspections per mechanic while showing each row
SELECT 
    mechanic_id,
    inspection_id,
    car_id,
    result,
    COUNT(*) OVER (PARTITION BY mechanic_id) AS total_inspections
FROM inspection;
```
  <img width="1914" height="1012" alt="7  Aggregate With OVER()" src="https://github.com/user-attachments/assets/8a816573-86eb-4e2f-b90c-66b2186dc580" />


**Business Value:**  
Supports ranking, trend analysis, historical comparisons, and advanced reporting.


## E. Packages

Packages organize related procedures and functions into a single modular unit.

### E.1 Package Specification
**Purpose:**  
Defines the public interface accessible to users and applications.

**Key Concepts:**
- Encapsulation  
- Modularity  
- Code reuse  

---

### E.2 Package Body
**Purpose:**  
Implements the logic defined in the package specification.

**Key Concepts:**
- Centralized business logic  
- Maintainability  
- Reusability  

---

## F. Exception Handling & Error Management

### F.1 Error Logging Table
**Purpose:**  
Stores runtime errors for auditing and troubleshooting.

---

### F.2 Central Error Logging Procedure
**Purpose:**  
Provides a reusable mechanism for logging errors across all procedures.

---

### F.3 Predefined Exceptions
**Purpose:**  
Handles standard Oracle exceptions such as:
- NO_DATA_FOUND  
- DUP_VAL_ON_INDEX  
- VALUE_ERROR  

---

### F.4 Custom Exceptions
**Purpose:**  
Implements business-specific validation rules such as preventing future manufacturing years.

---

### F.5 Recovery Mechanism
**Purpose:**  
Restores previous data state after a failed operation to maintain data integrity.

---

## Phase VI Completion Summary
Phase VI successfully demonstrates advanced PL/SQL capabilities including procedures, functions, cursors, packages, analytical queries, and robust exception handling, ensuring reliability, performance, and data integrity across the system.


