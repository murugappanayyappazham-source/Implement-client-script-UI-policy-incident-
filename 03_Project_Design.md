# 03_Project_Design.md

## Phase 3: Project Design & Architecture

### 1. Executive Summary
This document defines the detailed technical architecture, design specifications, and procedural workflows for the **Implement Client Script & UI Policy (Incident)** solution. It outlines the form-level event execution lifecycle, client-side data flows, UI Policy logic structures, and exact JavaScript implementation logic for each ServiceNow component.

---

### 2. Architecture & Design Diagram
+-----------------------------------------------------------------------------------+|                                 INCIDENT FORM / LIST                              |+-----------------------------------------------------------------------------------+|                                   |                                |v                                   v                                v+-----------------------+   +-------------------------------+   +-------------------+|  UI Policy Execution  |   |    onChange Client Script     |   |  onSubmit Script  || (High Impact Control) |   | (Auto set urgency high impact)|   | (Save Validation) |+-----------------------+   +-------------------------------+   +-------------------+| Condition:            |   | Trigger: Impact field changes |   | Trigger: Form     || Impact == '1' (High)  |   | Evaluation:                   |   | submission event  ||                       |   | If newValue == '1':           |   | Evaluation:       || Actions:              |   | - g_form.setValue('urgency')  |   | If impact == '1'  || - assignment_group    |   | - g_form.addInfoMessage()     |   | && assigned_to==""||   Mandatory = true    |   +-------------------------------+   | Actions:          || - urgency             |                                       | - showErrorBox()  ||   Read-only = true    |                                       | - return false    |+-----------------------+                                       +-------------------+|[Block Record Save]|+-------------------+|  onCellEdit Script|| (List View Edit)  |+-------------------+| Trigger: Double-  || click State cell  || Actions:          || - alert() modal   || - callback(false) |+-------------------+
---

### 3. Workflow & Data Flow Sequences

[User Selects Impact = "1 - High"]
│
├─► 1. UI Policy Triggers:
│      ├─► Sets Assignment Group -> Mandatory
│      └─► Sets Urgency -> Read-Only
│
├─► 2. onChange Script Triggers:
│      ├─► Auto-populates Urgency to "1 - High"
│      └─► Displays Info Message banner on form
│
[User Clicks "Submit" or "Update"]
│
└─► 3. onSubmit Script Triggers:
├─► Checks if Assigned To is empty
├─► IF empty: Shows error box under Assigned To & cancels save (return false)
└─► IF populated: Passes validation (return true) & saves record   [User Edits State Field in Incident List View]
│
└─► 4. onCellEdit Script Triggers:
├─► Displays alert: "State cannot be updated using list editing..."
└─► Cancels edit callback (callback(false))   
