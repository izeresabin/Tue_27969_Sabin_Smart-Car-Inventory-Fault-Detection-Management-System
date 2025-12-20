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

# Data Dictionary 

This data dictionary describes the database tables used in the **Smart Car Inventory and Fault Management System**, including their attributes and associated constraints.

---

## 1. CAR Table

| Column Name | Data Type | Description | Constraints |
|------------|----------|-------------|-------------|
| CAR_ID | NUMBER | Unique identifier for each vehicle | **PRIMARY KEY**, NOT NULL |
| MODEL | VARCHAR2(50) | Vehicle model name | NOT NULL |
| BRAND | VARCHAR2(50) | Manufacturer brand | NOT NULL |
| YEAR | NUMBER | Manufacturing year | NOT NULL, CHECK (YEAR >= 1886) |
| STATUS | VARCHAR2(20) | Current operational status | NOT NULL, CHECK (STATUS IN ('ACTIVE', 'INACTIVE', 'REPAIR')) |
| REG_DATE | DATE | Date the vehicle was registered | DEFAULT SYSDATE |

---

## 2. MECHANIC Table

| Column Name | Data Type | Description | Constraints |
|------------|----------|-------------|-------------|
| MECHANIC_ID | NUMBER | Unique mechanic identifier | **PRIMARY KEY**, NOT NULL |
| NAME | VARCHAR2(100) | Full name of the mechanic | NOT NULL |
| SPECIALIZATION | VARCHAR2(50) | Area of technical expertise | NOT NULL |

---

## 3. INSPECTION Table

| Column Name | Data Type | Description | Constraints |
|------------|----------|-------------|-------------|
| INSPECTION_ID | NUMBER | Unique inspection identifier | **PRIMARY KEY**, NOT NULL |
| CAR_ID | NUMBER | Inspected vehicle | **FOREIGN KEY** → CAR(CAR_ID), NOT NULL |
| MECHANIC_ID | NUMBER | Assigned mechanic | **FOREIGN KEY** → MECHANIC(MECHANIC_ID), NOT NULL |
| INSPECTION_DATE | DATE | Date of inspection | NOT NULL |
| RESULT | VARCHAR2(20) | Inspection outcome | NOT NULL, CHECK (RESULT IN ('PASS', 'FAIL', 'PENDING')) |
| NOTES | VARCHAR2(250) | Additional comments | NULL |

---

## 4. FAULT_REPORT Table

| Column Name | Data Type | Description | Constraints |
|------------|----------|-------------|-------------|
| FAULT_ID | NUMBER | Unique fault identifier | **PRIMARY KEY**, NOT NULL |
| CAR_ID | NUMBER | Vehicle with reported fault | **FOREIGN KEY** → CAR(CAR_ID), NOT NULL |
| DESCRIPTION | VARCHAR2(250) | Fault description | NOT NULL |
| SEVERITY | VARCHAR2(20) | Fault severity level | NOT NULL, CHECK (SEVERITY IN ('LOW', 'MEDIUM', 'HIGH')) |
| REPORT_DATE | DATE | Date fault was reported | DEFAULT SYSDATE |

---

## 5. STATUS_HISTORY Table

| Column Name | Data Type | Description | Constraints |
|------------|----------|-------------|-------------|
| HISTORY_ID | NUMBER | Unique status change record | **PRIMARY KEY**, NOT NULL |
| CAR_ID | NUMBER | Vehicle being tracked | **FOREIGN KEY** → CAR(CAR_ID), NOT NULL |
| OLD_STATUS | VARCHAR2(20) | Previous vehicle status | NOT NULL |
| NEW_STATUS | VARCHAR2(20) | Updated vehicle status | NOT NULL |
| CHANGE_DATE | DATE | Date of status change | DEFAULT SYSDATE, NOT NULL |

---

## Summary

The data dictionary ensures:
- Strong **data integrity** through primary and foreign keys
- Controlled values using **CHECK constraints**
- Accurate **audit tracking** via status history
- Reliable relationships across all system entities

This structure supports efficient inspections, fault tracking, reporting, and business intelligence analysis.

