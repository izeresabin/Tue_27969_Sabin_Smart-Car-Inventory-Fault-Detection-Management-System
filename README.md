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

