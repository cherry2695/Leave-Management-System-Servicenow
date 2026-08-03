# 🗓️ Leave Management System

A fully automated, self-service Leave Management application built natively on the **ServiceNow** platform.

Employees submit leave requests through a Service Portal; requests are validated in real time against live leave balances; approvals route automatically to the correct manager via Flow Designer; and leave balances update themselves the instant a request is approved — with zero manual HR intervention.

![Platform](https://img.shields.io/badge/Platform-ServiceNow-3C4E93?style=flat-square)
![Category](https://img.shields.io/badge/Category-Transportation-blue?style=flat-square)

---

## ✨ Key Features

- Fully automated request lifecycle — numbering, duration calculation, balance validation, and balance deduction, with no manual data entry
- Dynamic manager routing via Flow Designer — resolves each employee's actual Manager at runtime, no hardcoded approver
- Real-time balance validation — over-limit requests are blocked at submission, not caught later
- Country-ready policy engine via a dedicated Leave Calculator table
- Audit-safe by design — every request keeps a frozen snapshot of balance at submission time
- Role-based security enforced through ACLs (Employee / Manager / HR Admin)

---

## 🏗️ Data Model

```mermaid
erDiagram
    LEAVE_CALCULATOR ||--o{ LEAVE_BUCKET : "defines policy for"
    LEAVE_BUCKET ||--o{ LEAVE_REQUEST : "funds"
    USER ||--o{ LEAVE_REQUEST : "submits"

    LEAVE_CALCULATOR {
        string leave_type
        float leaves
        reference country
    }
    LEAVE_BUCKET {
        reference employee
        string leave_type
        float accured
        float taken
        float balance
    }
    LEAVE_REQUEST {
        string number
        reference requested_by
        string leave_type
        float duration
        string approval
    }
```

| Table | Purpose |
|---|---|
| **Leave Calculator** | Policy config - leave entitlement per type/country |
| **Leave Bucket** | Live ledger - current balance per employee/leave type |
| **Leave Request** | Individual requests, with an audit snapshot of balance |

---

## 🔄 Request Lifecycle

```mermaid
flowchart TD
    A[Employee submits request] --> B{Validate dates & duration}
    B -->|Invalid| B1[❌ Blocked]
    B -->|Valid| C{Check balance}
    C -->|Insufficient| C1[❌ Blocked]
    C -->|OK| D[✅ Saved]
    D --> E[Flow: Ask For Approval]
    E --> F{Manager Decision}
    F -->|Approve| G[Balance deducted]
    F -->|Reject| H[No change]
```

---

## 🧩 Components

| Component | Count |
|---|---|
| Custom Tables | 3 |
| Business Rules | 5 |
| Client Scripts | 3 |
| UI Policies | 2 |
| UI Actions | 2 |
| Flow Designer Flows | 1 |
| Custom Roles | 3 |
| Portal Widgets | 4 |

---

## 🛠️ Tech Stack

ServiceNow Scoped Application · Business Rules & GlideRecord · Flow Designer · Service Portal Widgets · ACL-based security

---

## 🚀 Setup

1. Import the application into your instance (Studio or Update Set).
2. Assign roles: `employee`, `manager`, `hr_admin`.
3. Configure Leave Calculator policies for your countries/leave types.
4. Create initial Leave Bucket records per employee.
5. Publish the Service Portal pages and activate the Approval Flow.

---
 
## 📸 Screenshots & Demo
 
| Leave Request Form | User Records |
|---|---|
| ![Leave Request](./Project%20Demo/Leave_Request.png) | ![User Records](./Project%20Demo/User_Records.png) |
 
| Leave Management Table | System Studio |
|---|---|
| ![Table](./Project%20Demo/Leave_Management_Table_Request.png) | ![System Studio](./Project%20Demo/Leave_Management_System_Studio.png) |
 
**Application form:**
 
![Leave Management Form](./Project%20Demo/Leave_Management_Form.png)
 
**🎥 Demo Video:** [Watch the full walkthrough](./Project%20Demo/Demo%20Video.mp4)
 
---

## 📂 Documentation

| Phase | Covers |
|---|---|
| [Phase-1 Requirement Analysis and Planning](./docs/Phase-1-Requirement-Analysis-and-Planning.md) | Objectives, scope, stakeholders |
| [Phase-2 Backend Development & Configurations](./docs/Phase-2-Backend-Development-and-Configurations.md) | Data model, business rules |
| [Phase-3 UI-UX Development & Customization](./docs/Phase-3-UI-UX-Development-and-Customization.md) | Portal, dashboards |
| [Phase-4 Data Migration, Testing & Security](./docs/Phase-4-Data-Migration-Testing-and-Security.md) | QA, ACLs, migration |
| [Phase-5 Deployment](./docs/Phase-5-Deployment.md) | Release, demo |
| [Project Conclusion](./docs/Project-Conclusion.md) | Outcomes, roadmap |

---

⭐ If you found this project useful, consider giving it a star!
