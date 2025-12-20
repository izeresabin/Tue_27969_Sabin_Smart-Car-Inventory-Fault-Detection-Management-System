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

