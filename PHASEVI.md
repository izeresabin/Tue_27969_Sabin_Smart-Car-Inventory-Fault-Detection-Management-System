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

```sql
--PACKAGE SPECIFICATION (PUBLIC INTERFACE):This is what other users/programs can call.
CREATE OR REPLACE PACKAGE smartcar_mgmt_pkg AS

    -- Procedure: Add a new car
    PROCEDURE add_car (
        p_car_id        IN NUMBER,
        p_model         IN VARCHAR2,
        p_brand         IN VARCHAR2,
        p_year          IN NUMBER,
        p_status        IN VARCHAR2,
        p_date_registered IN DATE DEFAULT SYSDATE
    );

    -- Procedure: Update car status (with OUT old status)
    PROCEDURE update_car_status (
        p_car_id     IN NUMBER,
        p_new_status IN VARCHAR2,
        p_old_status OUT VARCHAR2
    );

    -- Procedure: Delete mechanic safely
    PROCEDURE delete_mechanic (
        p_mechanic_id  IN NUMBER,
        p_rows_deleted OUT NUMBER
    );

    -- Function: Validate car status
    FUNCTION is_valid_status (
        p_status IN VARCHAR2
    ) RETURN VARCHAR2;

    -- Function: Get mechanic name
    FUNCTION get_mechanic_name (
        p_mechanic_id IN NUMBER
    ) RETURN VARCHAR2;

    -- Function: Check if a car has a critical fault
    FUNCTION has_critical_fault (
        p_car_id IN NUMBER
    ) RETURN VARCHAR2;

END smartcar_mgmt_pkg;
/
```
<img width="1919" height="1023" alt="PACKAGE SPECIFICATION (PUBLIC INTERFACE)" src="https://github.com/user-attachments/assets/a67b36a4-0094-4d27-a148-422e19689a4b" />


### E.2 Package Body
**Purpose:**  
Implements the logic defined in the package specification.
```sql
--PACKAGE BODY (IMPLEMENTATION):This contains the actual code.
CREATE OR REPLACE PACKAGE BODY smartcar_mgmt_pkg AS


-- ===========================================
-- PROCEDURE 1: Add a new car
-- ===========================================
PROCEDURE add_car (
    p_car_id        IN NUMBER,
    p_model         IN VARCHAR2,
    p_brand         IN VARCHAR2,
    p_year          IN NUMBER,
    p_status        IN VARCHAR2,
    p_date_registered IN DATE
)
IS
BEGIN
    INSERT INTO car (car_id, model, brand, year, status, date_registered)
    VALUES (p_car_id, p_model, p_brand, p_year, p_status, p_date_registered);

EXCEPTION
    WHEN DUP_VAL_ON_INDEX THEN
        DBMS_OUTPUT.PUT_LINE('Error: Car ID already exists.');
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('Unexpected error: ' || SQLERRM);
END add_car;



-- ===========================================
-- PROCEDURE 2: Update car status
-- ===========================================
PROCEDURE update_car_status (
    p_car_id     IN NUMBER,
    p_new_status IN VARCHAR2,
    p_old_status OUT VARCHAR2
)
IS
BEGIN
    -- Fetch previous status
    SELECT status INTO p_old_status
    FROM car
    WHERE car_id = p_car_id;

    -- Update car
    UPDATE car
    SET status = p_new_status
    WHERE car_id = p_car_id;

    -- Log history
    INSERT INTO status_history (
        history_id, car_id, old_status, new_status, change_date
    )
    VALUES (
        status_history_seq.NEXTVAL,
        p_car_id,
        p_old_status,
        p_new_status,
        SYSDATE
    );

EXCEPTION
    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE('Error: Car not found.');
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('Unexpected error: ' || SQLERRM);
END update_car_status;



-- ===========================================
-- PROCEDURE 3: Delete mechanic
-- ===========================================
PROCEDURE delete_mechanic (
    p_mechanic_id  IN NUMBER,
    p_rows_deleted OUT NUMBER
)
IS
    v_count NUMBER;
BEGIN
    -- Block deletion if mechanic is referenced
    SELECT COUNT(*) INTO v_count
    FROM inspection
    WHERE mechanic_id = p_mechanic_id;

    IF v_count > 0 THEN
        DBMS_OUTPUT.PUT_LINE('Error: Mechanic has inspections.');
        p_rows_deleted := 0;
        RETURN;
    END IF;

    -- Delete record
    DELETE FROM mechanic
    WHERE mechanic_id = p_mechanic_id;

    p_rows_deleted := SQL%ROWCOUNT;

EXCEPTION
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('Unexpected error: ' || SQLERRM);
END delete_mechanic;



-- ===========================================
-- FUNCTION 1: Validate status
-- ===========================================
FUNCTION is_valid_status (
    p_status IN VARCHAR2
) RETURN VARCHAR2
IS
BEGIN
    IF p_status IN ('ACTIVE', 'IN_REPAIR', 'DISABLED') THEN
        RETURN 'VALID';
    ELSE
        RETURN 'INVALID';
    END IF;
END is_valid_status;



-- ===========================================
-- FUNCTION 2: Get mechanic name
-- ===========================================
FUNCTION get_mechanic_name (
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
END get_mechanic_name;



-- ===========================================
-- FUNCTION 3: Check for critical fault
-- ===========================================
FUNCTION has_critical_fault (
    p_car_id IN NUMBER
) RETURN VARCHAR2
IS
    v_count NUMBER;
BEGIN
    SELECT COUNT(*) INTO v_count
    FROM fault_report
    WHERE car_id = p_car_id AND severity = 'CRITICAL';

    IF v_count > 0 THEN
        RETURN 'YES';
    ELSE
        RETURN 'NO';
    END IF;

END has_critical_fault;


END smartcar_mgmt_pkg;
/
```
<img width="1915" height="1022" alt="PACKAGE BODY (IMPLEMENTATION)" src="https://github.com/user-attachments/assets/f43cdecf-12bc-478c-9db8-3b4da7748f80" />


## F. Exception Handling & Error Management

### F.1 Error Logging Table
**Purpose:**  
Stores runtime errors for auditing and troubleshooting.
```sql
--1. ERROR LOGGING TABLE (Required for Logging):Before writing exceptions, we need a table to store errors.
CREATE TABLE error_log (
    error_id      NUMBER GENERATED ALWAYS AS IDENTITY,
    error_code    NUMBER,
    error_message VARCHAR2(400),
    error_date    DATE DEFAULT SYSDATE,
    module_name   VARCHAR2(50)
);
```
<img width="1919" height="985" alt="1  ERROR LOGGING TABLE (Required for Logging)" src="https://github.com/user-attachments/assets/0933717e-a7ed-49fe-a1bc-9e2b910b4b27" />


### F.2 Central Error Logging Procedure
**Purpose:**  
Provides a reusable mechanism for logging errors across all procedures.
```sql
ERROR LOGGING PROCEDURE: Every exception handler will call this.
CREATE OR REPLACE PROCEDURE log_error (
    p_code    NUMBER,
    p_msg     VARCHAR2,
    p_module  VARCHAR2
)
IS
BEGIN
    INSERT INTO error_log (error_code, error_message, module_name)
    VALUES (p_code, p_msg, p_module);

EXCEPTION
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('Logging failed: ' || SQLERRM);
END;
/
```
<img width="1919" height="1023" alt="2  ERROR LOGGING PROCEDURE" src="https://github.com/user-attachments/assets/bc5c56e0-bd71-41e9-b742-b1238b8a8577" />


### F.3 Predefined Exceptions
**Purpose:**  
Handles standard Oracle exceptions such as:
- NO_DATA_FOUND  
- DUP_VAL_ON_INDEX  
- VALUE_ERROR  
```sql
Example: Handling NO_DATA_FOUND, DUP_VAL_ON_INDEX, VALUE_ERROR.
CREATE OR REPLACE PROCEDURE update_car_year (
    p_car_id IN NUMBER,
    p_year   IN NUMBER
)
IS
BEGIN
    UPDATE car
    SET year = p_year
    WHERE car_id = p_car_id;

EXCEPTION
    WHEN NO_DATA_FOUND THEN
        log_error(SQLCODE, SQLERRM, 'update_car_year');
        DBMS_OUTPUT.PUT_LINE('Error: Car not found.');

    WHEN DUP_VAL_ON_INDEX THEN
        log_error(SQLCODE, SQLERRM, 'update_car_year');
        DBMS_OUTPUT.PUT_LINE('Duplicate key error.');

    WHEN VALUE_ERROR THEN
        log_error(SQLCODE, SQLERRM, 'update_car_year');
        DBMS_OUTPUT.PUT_LINE('Invalid numeric or date format.');

    WHEN OTHERS THEN
        log_error(SQLCODE, SQLERRM, 'update_car_year');
        DBMS_OUTPUT.PUT_LINE('Unknown error occurred.');
END;
/
```
<img width="1919" height="1000" alt="3  PREDEFINED EXCEPTIONS EXAMPLE" src="https://github.com/user-attachments/assets/62927b03-7b1b-4a0c-9a81-16aa59f8a7e7" />

### F.4 Custom Exceptions
**Purpose:**  
Implements business-specific validation rules such as preventing future manufacturing years.

```sql
--We define a custom exception:
--Requirement:"Car year cannot be in the future"
CREATE OR REPLACE PROCEDURE add_car_safe (
    p_car_id  IN NUMBER,
    p_model   IN VARCHAR2,
    p_brand   IN VARCHAR2,
    p_year    IN NUMBER
)
IS
    future_year EXCEPTION;
BEGIN
    IF p_year > EXTRACT(YEAR FROM SYSDATE) THEN
        RAISE future_year;
    END IF;

    INSERT INTO car (car_id, model, brand, year, status, date_registered)
    VALUES (p_car_id, p_model, p_brand, p_year, 'ACTIVE', SYSDATE);

EXCEPTION
    WHEN future_year THEN
        log_error(-20010, 'Car manufacturing year cannot be in the future', 'add_car_safe');
        DBMS_OUTPUT.PUT_LINE('Invalid year: cannot insert a future year.');

    WHEN OTHERS THEN
        log_error(SQLCODE, SQLERRM, 'add_car_safe');
        DBMS_OUTPUT.PUT_LINE('Insert failed.');
END;
/
```
<img width="1919" height="992" alt="4  CUSTOM EXCEPTION EXAMPLE" src="https://github.com/user-attachments/assets/2f0486eb-0ec7-42ce-87a6-bf37f73373b0" />


### F.5 Recovery Mechanism
**Purpose:**  
Restores previous data state after a failed operation to maintain data integrity.

```sql
--Example: If updating mechanic fails ? restore old data.
CREATE OR REPLACE PROCEDURE safe_update_mechanic (
    p_id      IN NUMBER,
    p_newname IN VARCHAR2
)
IS
    v_old_name  mechanic.name%TYPE;
BEGIN
    -- Backup old value
    SELECT name INTO v_old_name
    FROM mechanic
    WHERE mechanic_id = p_id;

    -- Attempt update
    UPDATE mechanic
    SET name = p_newname
    WHERE mechanic_id = p_id;

EXCEPTION
    WHEN OTHERS THEN
        -- Recovery: restore old name
        UPDATE mechanic
        SET name = v_old_name
        WHERE mechanic_id = p_id;

        log_error(SQLCODE, SQLERRM, 'safe_update_mechanic');

        DBMS_OUTPUT.PUT_LINE('Update failed. Data restored.');
END;
/
```
<img width="1919" height="1019" alt="5  RECOVERY MECHANISM EXAMPLE" src="https://github.com/user-attachments/assets/19d5b9a3-a149-417d-bbbd-0eaa9faf1a3a" />


## G. Phase VI Testing Report

This section documents the testing performed to verify the correctness, robustness, and performance of all procedures, functions, and cursors implemented in Phase VI.


### G.1 Procedure and Function Testing
This test confirms that stored procedures and functions execute correctly using valid input values.

```sql
-- Test Procedure: add_car
EXEC smartcar_mgmt_pkg.add_car(3001, 'Civic', 'Honda', 2018, 'ACTIVE');
````
<img width="1919" height="1016" alt="1 Test for (Each procedure tested)" src="https://github.com/user-attachments/assets/2195cbba-fd56-4389-940e-0bc129d56cb1" />


### G.2 Edge Case Validation

This test validates that the system correctly handles invalid or unexpected input values.

```sql
-- Test Function: is_valid_status (invalid status)
SELECT smartcar_mgmt_pkg.is_valid_status('BROKEN') AS test_result
FROM dual;
```
<img width="1546" height="472" alt="test2" src="https://github.com/user-attachments/assets/14e5658e-74ba-48f6-a24f-127c93c83a93" />


### G.3 Performance Verification

This test verifies performance efficiency when processing multiple records using bulk operations.

```sql
DECLARE
    TYPE car_list IS TABLE OF car.car_id%TYPE;
    v_ids car_list;
BEGIN
    SELECT car_id BULK COLLECT INTO v_ids FROM car;
    DBMS_OUTPUT.PUT_LINE('Rows fetched: ' || v_ids.COUNT);
END;
/
```
<img width="1502" height="592" alt="test3" src="https://github.com/user-attachments/assets/24d2a2fd-0ced-4967-8716-79fe07360629" />


### Phase VI Testing Summary

All procedures, functions, and cursors were tested successfully.
Edge cases were handled gracefully, and bulk operations demonstrated efficient performance when processing multiple records.




### Phase VI Completion Summary
Phase VI successfully demonstrates advanced PL/SQL capabilities including procedures, functions, cursors, packages, analytical queries, and robust exception handling, ensuring reliability, performance, and data integrity across the system.
