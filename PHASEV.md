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
