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
