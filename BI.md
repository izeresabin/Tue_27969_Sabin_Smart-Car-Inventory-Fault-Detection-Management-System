# Phase VIII – Business Intelligence (BI)

## Overview
The Business Intelligence (BI) component enhances decision-making within the Smart Car Inventory and Fault Management System by transforming operational data into actionable insights. BI focuses on defining key performance indicators (KPIs), identifying decision-support needs, documenting stakeholders, and presenting conceptual dashboard mockups.

---

## 1. BI Requirements

### 1.1 Stakeholders
- Dealership Management
- Workshop Manager
- Mechanics
- Compliance Officers
- System Administrator

---

### 1.2 Decision Support Needs
- Monitor vehicle inspection quality and outcomes
- Identify cars requiring urgent repair
- Track mechanic workload and performance
- Detect violations of business rules and security policies
- Support strategic planning using historical trends

---

### 1.3 Reporting Frequency

| Report Type | Frequency |
|------------|-----------|
| Executive KPIs | Weekly |
| Audit & Compliance Reports | Daily |
| Performance Reports | Monthly |
| Fault Severity Analysis | Weekly |

---

## 2. Key Performance Indicators (KPIs)

### KPI 1: Total Registered Cars
- **Description:** Total number of vehicles registered in the system
- **Source Table:** CAR
- **Calculation:** COUNT(CAR_ID)
- **Decision Supported:** Inventory planning

### KPI 2: Cars Under Repair
- **Description:** Vehicles currently undergoing repair
- **Source Table:** CAR
- **Filter:** STATUS = 'REPAIR'
- **Decision Supported:** Resource allocation
# KPI FOR CARS
<img width="1426" height="730" alt="image" src="https://github.com/user-attachments/assets/f374235f-e08e-481a-b387-522711785a4f" />

### KPI 3: Inspection Pass Rate (%)
- **Description:** Percentage of inspections that passed
- **Source Table:** INSPECTION
- **Calculation:** (PASS / TOTAL) × 100
- **Decision Supported:** Quality assurance
<img width="1439" height="743" alt="inspection dash" src="https://github.com/user-attachments/assets/5a1ba7b0-4fc0-439a-8391-1907c3a1cf9e" />

### KPI 4: High Severity Faults
- **Description:** Number of faults classified as high severity
- **Source Table:** FAULT_REPORT
- **Filter:** SEVERITY = 'HIGH'
- **Decision Supported:** Risk management
<img width="1412" height="743" alt="severity fault " src="https://github.com/user-attachments/assets/7035e9a4-fa47-4b8d-8ca6-0188a0f065dd" />

### KPI 5: Audit Violations
- **Description:** Number of denied database operations
- **Source Table:** AUDIT_LOG
- **Filter:** ACTION_STATUS = 'DENIED'
- **Decision Supported:** Policy compliance
<img width="1466" height="734" alt="audit violations" src="https://github.com/user-attachments/assets/1984c7b6-6af7-4b1d-b2f6-0efbc410f9d3" />

### KPI 6: Inspections per Mechanic
- **Description:** Total inspections conducted by each mechanic
- **Source Table:** INSPECTION
- **Calculation:** GROUP BY MECHANIC_ID
- **Decision Supported:** Performance evaluation

---

## 3. Dashboard Mockups (Conceptual)

> Dashboards are documented as conceptual mockups to illustrate how analytics would be visualized using tools such as Power BI. Live database integration is not required.

---

### 3.1 Executive Summary Dashboard
**Purpose:** Provide a high-level overview for management.

**Components:**
- KPI Cards:
  - Total Registered Cars
  - Cars Under Repair
  - Inspection Pass Rate
  - High Severity Faults
- Visualizations:
  - Line chart showing inspections over time
  - Bar chart displaying faults by severity
  - Donut chart representing vehicle status distribution

**Target Users:** Management, Senior Supervisors

---

### 3.2 Audit Dashboard
**Purpose:** Monitor system compliance and enforcement of business rules.

**Components:**
- KPI Cards:
  - Total Operations Logged
  - Allowed Operations
  - Denied Operations
- Visualizations:
  - Bar chart comparing allowed vs denied operations
  - Line chart showing violations over time
  - Table displaying audit logs (user, action, date, status)

**Target Users:** Compliance Officers, System Administrator

---

### 3.3 Performance Dashboard
**Purpose:** Evaluate operational efficiency and mechanic performance.

**Components:**
- KPI Cards:
  - Total Inspections
  - Average Inspections per Mechanic
  - Total Faults Reported
- Visualizations:
  - Bar chart showing inspections per mechanic
  - Column chart of faults reported per month
  - Performance summary table

**Target Users:** Workshop Manager, Operations Supervisor

---

## 4. Analytical SQL Queries

```sql
-- Total registered cars
SELECT COUNT(*) AS total_cars FROM car;
```
<img width="1619" height="1023" alt="1" src="https://github.com/user-attachments/assets/c00c8d37-ca74-4e21-bd6c-3ec66351fe48" />

```sql
-- Cars currently under repair
SELECT COUNT(*) AS cars_under_repair
FROM car
WHERE status = 'REPAIR';
```
<img width="1640" height="985" alt="2" src="https://github.com/user-attachments/assets/2bc790c4-a275-47d7-8763-03ef75d7b2ea" />

```sql
-- Inspection pass rate
SELECT 
  ROUND(
    (SUM(CASE WHEN result = 'PASS' THEN 1 ELSE 0 END) / COUNT(*)) * 100, 2
  ) AS pass_rate_percentage
FROM inspection;
```
<img width="1557" height="985" alt="3" src="https://github.com/user-attachments/assets/46b961bf-8876-4cbf-bf6e-719954d27acf" />


```sql
-- High severity faults
SELECT COUNT(*) AS high_severity_faults
FROM fault_report
WHERE severity = 'HIGH';
```
<img width="1427" height="1014" alt="4" src="https://github.com/user-attachments/assets/da2b0d5b-592a-4d6a-9fbe-12f115e870c5" />


```sql
-- Inspections per mechanic
SELECT mechanic_id, COUNT(*) AS total_inspections
FROM inspection
GROUP BY mechanic_id;
```
<img width="1640" height="985" alt="5" src="https://github.com/user-attachments/assets/ff37c911-9802-4115-b131-762ae161342c" />


```sql
-- Audit violations
SELECT COUNT(*) AS total_violations
FROM audit_log
WHERE status = 'DENIED';
```
<img width="1343" height="986" alt="6" src="https://github.com/user-attachments/assets/e1f3e5d7-73a9-48a5-a1e9-6e4ce8b1e204" />

