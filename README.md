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


