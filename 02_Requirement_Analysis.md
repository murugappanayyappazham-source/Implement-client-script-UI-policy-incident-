# 02_Requirement_Analysis.md

## Phase 2: Requirement Analysis & Technical Specifications

### 1. Executive Summary
This document translates the ideation phase concepts into actionable functional, non-functional, and technical requirements for the **Implement Client Script & UI Policy (Incident)** project[cite: 1]. It outlines the exact data schema, field behavior matrix, acceptance criteria, and system triggers necessary to build a robust client-side validation framework on ServiceNow[cite: 1].

---

### 2. Functional Requirements (FR)

| Requirement ID | Module / Area | Functional Description | Trigger / Condition | Expected Behavior |
| :--- | :--- | :--- | :--- | :--- |
| **FR-01** | UI Policy | Enforce mandatory `Assignment group` for high-impact incidents[cite: 1]. | `Impact == 1 - High`[cite: 1] | `Assignment group` field becomes mandatory[cite: 1]. If impact changes away from `1 - High`, the field reverts to optional (`Reverse if false`)[cite: 1]. |
| **FR-02** | UI Policy Action | Lock down `Urgency` field editing during high impact[cite: 1]. | `Impact == 1 - High`[cite: 1] | `Urgency` field becomes read-only on the form[cite: 1]. Reverts to editable when condition is false[cite: 1]. |
| **FR-03** | Client Script (`onChange`) | Auto-populate `Urgency` based on `Impact` selection[cite: 1]. | User changes `Impact` field to `1 - High`[cite: 1]. | Script sets `Urgency` value to `1 - High` automatically and displays an informational message[cite: 1]. |
| **FR-04** | Client Script (`onSubmit`) | Enforce `Assigned To` presence before record save[cite: 1]. | User clicks **Submit**, **Update**, or **Save** while `Impact == 1 - High`[cite: 1]. | If `Assigned To` is empty, record submission is aborted (`return false`) and an error box appears under the field[cite: 1]. |
| **FR-05** | Client Script (`onCellEdit`) | Restrict direct list editing on the `State` field[cite: 1]. | User double-clicks `State` field in list view[cite: 1]. | Editing is blocked (`callback(false)`), and an alert modal informs the user to open the form to change status[cite: 1]. |

---

### 3. Non-Functional Requirements (NFR)

* **NFR-01: Usability & Feedback:** Error messages and field highlights must provide immediate, clear feedback directly at the user interface level[cite: 1].
* **NFR-02: Performance:** Client scripts must execute efficiently without causing visible form rendering delays or blocking DOM interaction.
* **NFR-03: Maintainability:** Client scripts must adhere to standard ServiceNow API practices (`g_form`, `showErrorBox`, `addInfoMessage`) and avoid Direct DOM manipulation[cite: 1].
* **NFR-04: Scalability & Consistency:** UI Policies must use standard platform configurations (`Reverse if false`, `On load`) so behavior remains predictable across form updates[cite: 1].

---

### 4. Technical Artifact & Field Mapping Matrix

| ServiceNow Artifact | Name / Description | Target Table | Trigger Type / Event | Targeted Field(s) | Specific Action / Logic |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **UI Policy** | `High Impact Control`[cite: 1] | `Incident` (`incident`)[cite: 1] | `Impact == 1 - High`[cite: 1] | N/A | Evaluates policy condition on load and change[cite: 1]. |
| **UI Policy Action** | Action - Assignment Group[cite: 1] | `Incident` (`incident`)[cite: 1] | Inherited from Policy[cite: 1] | `assignment_group`[cite: 1] | `Mandatory: True`, `Read-only: False`[cite: 1] |
| **UI Policy Action** | Action - Urgency[cite: 1] | `Incident` (`incident`)[cite: 1] | Inherited from Policy[cite: 1] | `urgency`[cite: 1] | `Mandatory: Leave alone`, `Read-only: True`[cite: 1] |
| **Client Script** | `Auto set urgency for high impact`[cite: 1] | `Incident` (`incident`)[cite: 1] | `onChange` (`Impact`)[cite: 1] | `impact`, `urgency`[cite: 1] | Sets `urgency` to `1` and shows info message[cite: 1]. |
| **Client Script** | `Prevent save if Assigned To missing`[cite: 1] | `Incident` (`incident`)[cite: 1] | `onSubmit`[cite: 1] | `assigned_to`, `impact`[cite: 1] | Checks `assigned_to` value; blocks save if empty[cite: 1]. |
| **Client Script** | `Prevent state change via list edit`[cite: 1] | `Incident` (`incident`)[cite: 1] | `onCellEdit` (`State`)[cite: 1] | `state`[cite: 1] | Cancels edit callback and shows alert dialog[cite: 1]. |

---

### 5. Acceptance Criteria

1. **High Impact Form Behavior:** When `Impact` is set to `1 - High`, `Assignment group` must immediately display as mandatory (asterisk present) and `Urgency` must be set to read-only[cite: 1].
2. **Auto-Populate Execution:** Changing `Impact` to `1 - High` must automatically set `Urgency` to `1 - High` without manual user intervention[cite: 1].
3. **Save Prevention Validation:** Attempting to save a `1 - High` impact incident with an unpopulated `Assigned To` field must fail, showing an error box[cite: 1]. Populating `Assigned To` must allow the save to succeed[cite: 1].
4. **List Edit Prevention:** Double-clicking the `State` field on any incident in list view must display an alert modal and prevent value updates[cite: 1].
