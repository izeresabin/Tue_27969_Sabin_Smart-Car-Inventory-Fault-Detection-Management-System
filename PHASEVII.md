## PHASE VII – Advanced Programming, Triggers & Auditing

Phase VII focuses on enforcing advanced business rules using database triggers, implementing auditing mechanisms to track user activities, and preventing unauthorized or invalid data manipulation operations automatically.


## A. Public Holiday Management

This table stores public holidays that are used to restrict database operations during sensitive dates.

```sql
CREATE TABLE public_holidays (
    holiday_date DATE PRIMARY KEY,
    description  VARCHAR2(100)
);
````

```sql
INSERT INTO public_holidays VALUES (DATE '2025-01-01', 'New Year');
INSERT INTO public_holidays VALUES (DATE '2025-01-26', 'Public Holiday');
COMMIT;
```
<img width="1915" height="486" alt="public holidays" src="https://github.com/user-attachments/assets/a1348e9e-4fb5-458d-8387-0ce3c9eb9f2b" />

## B. Audit Log Table

The audit log table records all database operations, including the user performing the action, the affected table, action type, execution status, and descriptive messages.

```sql
CREATE TABLE audit_log (
    audit_id    NUMBER GENERATED ALWAYS AS IDENTITY,
    username    VARCHAR2(50),
    action      VARCHAR2(10),
    table_name  VARCHAR2(50),
    action_date DATE,
    status      VARCHAR2(20),
    message     VARCHAR2(200)
);
```
<img width="1889" height="990" alt="Audit Log Table" src="https://github.com/user-attachments/assets/74916be1-2f97-4410-921f-3f70c4aa942f" />


## C. Audit Logging Procedure

This reusable procedure inserts audit records whenever a trigger detects a database operation.

```sql
CREATE OR REPLACE PROCEDURE log_audit (
    p_action     VARCHAR2,
    p_table      VARCHAR2,
    p_status     VARCHAR2,
    p_message    VARCHAR2
)
IS
BEGIN
    INSERT INTO audit_log (
        username, action, table_name,
        action_date, status, message
    )
    VALUES (
        USER, p_action, p_table,
        SYSDATE, p_status, p_message
    );
END;
/
```
<img width="1916" height="1017" alt="Audit Logging procedure" src="https://github.com/user-attachments/assets/5a32a08f-cb36-4979-b427-0b57bea8e279" />


## D. Core Business Rule Function – Operation Restriction

This function enforces a core business rule that blocks database operations during weekdays and public holidays.

```sql
CREATE OR REPLACE FUNCTION is_operation_allowed
RETURN BOOLEAN
IS
    v_day        VARCHAR2(10);
    v_holiday_ct NUMBER;
BEGIN
    -- Check weekday
    v_day := TO_CHAR(SYSDATE, 'DY', 'NLS_DATE_LANGUAGE=ENGLISH');

    IF v_day IN ('MON','TUE','WED','THU','FRI') THEN
        RETURN FALSE;
    END IF;

    -- Check upcoming-month holidays
    SELECT COUNT(*) INTO v_holiday_ct
    FROM public_holidays
    WHERE holiday_date BETWEEN TRUNC(SYSDATE)
          AND ADD_MONTHS(TRUNC(SYSDATE), 1);

    IF v_holiday_ct > 0 THEN
        RETURN FALSE;
    END IF;

    RETURN TRUE;
END;
/
```
<img width="1919" height="1015" alt="Restriction Check Function (CORE RULE)" src="https://github.com/user-attachments/assets/52091c63-db18-45e8-80a2-54596c32fd44" />


## E. Simple Trigger – CAR Table Restriction

This trigger blocks INSERT, UPDATE, and DELETE operations on the `CAR` table when business rules are violated and logs the action outcome.

```sql
CREATE OR REPLACE TRIGGER trg_car_restriction
BEFORE INSERT OR UPDATE OR DELETE ON car
FOR EACH ROW
BEGIN
    IF NOT is_operation_allowed THEN
        log_audit(
            ORA_SYSEVENT,
            'CAR',
            'BLOCKED',
            'Operation not allowed on weekdays or public holidays'
        );
        RAISE_APPLICATION_ERROR(
            -20050,
            'DML blocked: Weekdays and public holidays are restricted'
        );
    END IF;

    log_audit(
        ORA_SYSEVENT,
        'CAR',
        'SUCCESS',
        'Operation allowed'
    );
END;
/
```
<img width="1910" height="1015" alt="Simple Trigger (Single Table)" src="https://github.com/user-attachments/assets/e80769e9-8b52-4340-81c7-da631315db73" />


## F. Compound Trigger – INSPECTION Table

This compound trigger applies the same restriction logic to the `INSPECTION` table while auditing both row-level and statement-level operations.

```sql
CREATE OR REPLACE TRIGGER trg_dml_restriction_compound
FOR INSERT OR UPDATE OR DELETE
ON inspection
COMPOUND TRIGGER

BEFORE EACH ROW IS
BEGIN
    IF NOT is_operation_allowed THEN
        log_audit(
            ORA_SYSEVENT,
            'INSPECTION',
            'BLOCKED',
            'Restricted day operation'
        );
        RAISE_APPLICATION_ERROR(
            -20051,
            'DML blocked by business rule'
        );
    END IF;
END BEFORE EACH ROW;

AFTER STATEMENT IS
BEGIN
    log_audit(
        ORA_SYSEVENT,
        'INSPECTION',
        'SUCCESS',
        'Statement completed successfully'
    );
END AFTER STATEMENT;

END;
/
```

<img width="1914" height="1018" alt="Compound Trigger (Multiple Events)" src="https://github.com/user-attachments/assets/77be521d-a0a2-4783-aedd-43de3dd0a6b7" />


## H. Phase VII – Trigger & Auditing Testing

This section validates that all triggers and auditing mechanisms correctly enforce business rules, block restricted operations, allow valid operations, and record every action in the audit log.



## H.1 Trigger Test – INSERT Blocked on Weekday (DENIED)

This test verifies that the trigger prevents INSERT operations on the `CAR` table during weekdays.

```sql
INSERT INTO car
VALUES (9100, 'TestCar', 'TestBrand', 2020, 'ACTIVE', SYSDATE);
````

<img width="1911" height="984" alt="1  when insetring data in week dayss" src="https://github.com/user-attachments/assets/29f017c3-c1f8-48c6-871c-1a54c1d0307e" />

## H.2 Trigger Test – INSERT Allowed on Weekend (ALLOWED)

This test confirms that INSERT operations are permitted on weekends when no business rules are violated.

```sql
INSERT INTO car
VALUES (9101, 'WeekendCar', 'Toyota', 2021, 'ACTIVE', SYSDATE);
```
<img width="1916" height="1019" alt="2  Trigger allows INSERT on WEEKEND (ALLOWED) inserted on sunday" src="https://github.com/user-attachments/assets/81e7f6fc-4191-4af1-8712-0bb8052fcd1d" />


## H.3 Trigger Test – INSERT Blocked on Public Holiday (DENIED)

This test ensures that INSERT operations are blocked when a public holiday exists within the upcoming month.

```sql
INSERT INTO car
VALUES (9102, 'HolidayCar', 'Honda', 2022, 'ACTIVE', SYSDATE);
```
<img width="1458" height="406" alt="3Trigger blocks INSERT on PUBLIC HOLIDAY (DENIED)" src="https://github.com/user-attachments/assets/17cb04e3-3c30-44b1-8320-3cd518bc79b4" />


## H.4 Audit Log Verification – All Attempts Recorded

This test verifies that both successful and blocked operations are fully recorded in the audit log.

```sql
SELECT username, action, table_name, status, message, action_date
FROM audit_log
ORDER BY action_date DESC;
```
<img width="1913" height="1018" alt="4  Audit log captures ALL attempts (SUCCESS + BLOCKED)  status in weekend" src="https://github.com/user-attachments/assets/665ab9a8-a2e1-491b-ab39-3c78f3b342b6" />


## H.5 Audit Log Verification – User Information Captured

This test confirms that the database user performing each operation is properly recorded in the audit log.

```sql
SELECT DISTINCT username
FROM audit_log;
```
<img width="1500" height="372" alt="6  User info properly recorded" src="https://github.com/user-attachments/assets/189313e1-fbda-4915-b32d-0c2b2cb37437" />


## Phase VII Testing Conclusion

* Triggers correctly block restricted weekday and public holiday operations
* Valid weekend operations are successfully allowed
* Every attempt is logged with accurate status and messages
* User identity and timestamps are fully captured
