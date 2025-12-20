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

### KPI 4: High Severity Faults
- **Description:** Number of faults classified as high severity
- **Source Table:** FAULT_REPORT
- **Filter:** SEVERITY = 'HIGH'
- **Decision Supported:** Risk management

### KPI 5: Audit Violations
- **Description:** Number of denied database operations
- **Source Table:** AUDIT_LOG
- **Filter:** ACTION_STATUS = 'DENIED'
- **Decision Supported:** Policy compliance

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

-- Cars currently under repair
SELECT COUNT(*) AS cars_under_repair
FROM car
WHERE status = 'REPAIR';

-- Inspection pass rate
SELECT 
  ROUND(
    (SUM(CASE WHEN result = 'PASS' THEN 1 ELSE 0 END) / COUNT(*)) * 100, 2
  ) AS pass_rate_percentage
FROM inspection;

-- High severity faults
SELECT COUNT(*) AS high_severity_faults
FROM fault_report
WHERE severity = 'HIGH';

-- Inspections per mechanic
SELECT mechanic_id, COUNT(*) AS total_inspections
FROM inspection
GROUP BY mechanic_id;

-- Audit violations
SELECT COUNT(*) AS total_violations
```
FROM audit_log
WHERE action_status = 'DENIED';
