# 05_Project_Development.md

## Phase 5: Build Execution, Code Artifacts & Development Log

### 1. Executive Summary & Build Overview
This document details the development execution, code artifacts, and configuration steps completed during Phase 5 for the **Implement Client Script & UI Policy (Incident)** project. It serves as the developer handoff documentation, containing the exact scripts, field property settings, and configuration steps applied within the ServiceNow instance.

---

### 2. Development Artifact Summary

| Artifact Name | Type | Target Table | Scope / Active | Trigger Event / Condition |
| :--- | :--- | :--- | :--- | :--- |
| **High Impact Control** | UI Policy | `incident` | Global / Active | `Impact == 1 - High` |
| **Auto set urgency for high impact** | Client Script (`onChange`) | `incident` | Desktop/All / Active | Field change on `impact` |
| **Prevent save if Assigned To missing** | Client Script (`onSubmit`) | `incident` | Desktop/All / Active | Form save/update submission |
| **Prevent state change via list edit** | Client Script (`onCellEdit`) | `incident` | Desktop/All / Active | Cell editing `state` in list view |

---

### 3. Step-by-Step Configuration & Code Implementation

#### 3.1 UI Policy: High Impact Control
1. **Header Configuration:**
   * **Table:** Incident (`incident`)
   * **Short Description:** `High Impact Control`
   * **Conditions:** `Impact` `is` `1 - High`
   * **Global:** `true` | **On load:** `true` | **Reverse if false:** `true`

2. **UI Policy Actions Configured:**
   * **Action 1:** `assignment_group` -> **Mandatory:** `True` | **Read-only:** `False` | **Visible:** `Leave alone`
   * **Action 2:** `urgency` -> **Mandatory:** `Leave alone` | **Read-only:** `True` | **Visible:** `Leave alone`

---

#### 3.2 Client Script 1: `onChange` (Impact Automation)
* **Name:** `Auto set urgency for high impact`[cite: 1]
* **Table:** `incident`[cite: 1]
* **UI Type:** `All`[cite: 1]
* **Type:** `onChange`[cite: 1]
* **Field Name:** `impact`[cite: 1]

```javascript
function onChange(control, oldValue, newValue, isLoading) {
    // Prevent execution during form loading or empty selections
    if (isLoading || newValue == '') {
        return;
    }
    
    // Auto-populate Urgency to High when Impact is High
    if (newValue == '1') {
        g_form.setValue('urgency', '1');
        g_form.addInfoMessage('Urgency set to High for High impact incident.');
    }
}
