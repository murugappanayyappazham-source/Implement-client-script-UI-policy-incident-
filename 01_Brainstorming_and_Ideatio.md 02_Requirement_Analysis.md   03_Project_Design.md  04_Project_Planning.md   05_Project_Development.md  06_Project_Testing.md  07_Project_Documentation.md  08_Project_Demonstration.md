# 01_Brainstorming_and_Ideation.md

## Phase 1: Problem Definition, Brainstorming & Ideation

### 1. Executive Summary & Context
In IT Service Management (ITSM), incident records serve as the foundation for operational response, SLA compliance, and service reporting[cite: 1]. However, relying purely on manual user compliance when filling out incident forms often results in incomplete data, inconsistent severity triage, and unauthorized list-level state bypasses[cite: 1]. 

This phase focuses on identifying core operational bottlenecks, evaluating architectural solutions in ServiceNow, and selecting the optimal combination of client-side controls (**UI Policies** and **Client Scripts**) to enforce data integrity dynamically[cite: 1].

---

### 2. Problem Statement & Operational Challenges
Service Desk analysts and platform users frequently submit or modify Incident records without providing essential details[cite: 1]. Key issues identified include:

* **Incomplete Triage Data:** High-impact incidents being logged without mandatory operational parameters like assignment groups or specific assignees[cite: 1].
* **Inconsistent Urgency Alignment:** Users manually configuring mismatched impact and urgency levels, leading to incorrect priority calculations[cite: 1].
* **Bypassing Workflow Controls:** Users altering critical lifecycle attributes (e.g., `State`) via list views without executing necessary form validations[cite: 1].
* **User Frustration & Inefficiency:** Server-side errors triggering after submission rather than immediate, dynamic feedback directly on the form[cite: 1].

---

### 3. Solution Brainstorming & Approach Comparison

During ideation, three primary platform approaches were evaluated to address data consistency:

| Criteria | Option A: Server-Side Validation (Data Lookup / Business Rules) | Option B: Pure Client Scripts (`g_form`) | Option C: Hybrid Client-Side Controls (UI Policies + Client Scripts) |
| :--- | :--- | :--- | :--- |
| **User Experience (UX)** | Reactive (Errors appear only after form submission / page reload)[cite: 1]. | Proactive, but requires high code maintenance for basic visibility/mandatory logic[cite: 1]. | **Optimal & Responsive** (Immediate form-level feedback with minimal code maintenance)[cite: 1]. |
| **Performance Impact** | Requires server roundtrips for validation[cite: 1]. | Runs in client browser, but excessive scripting bloats load times[cite: 1]. | **Lightweight** (UI Policies run natively; scripts execute only on targeted events)[cite: 1]. |
| **List View Governance** | Business rules block list edits server-side with generic messages[cite: 1]. | Limited list control except via targeted client scripts (`onCellEdit`)[cite: 1]. | **Targeted Protection** (`onCellEdit` provides explicit user guidance)[cite: 1]. |
| **Recommendation** | Rejected for form-level interactivity[cite: 1]. | Rejected due to high maintenance overhead[cite: 1]. | **Selected Solution**[cite: 1]. |

---

### 4. Selected Ideation Scope & Key Deliverables

The hybrid client-side governance model was selected for implementation[cite: 1]. The scope comprises four core scenarios[cite: 1]:

1. **High Impact Control (UI Policy):**
   * Automatically make `Assignment group` mandatory when `Impact == 1 - High`[cite: 1].
   * Set `Urgency` to read-only when `Impact == 1 - High`[cite: 1].
   * Ensure conditional changes automatically revert when `Impact` changes (`Reverse if false = true`)[cite: 1].

2. **Impact-Driven Automation (`onChange` Client Script):**
   * Automatically update `Urgency` to `1 - High` when `Impact` is changed to `1 - High`[cite: 1].
   * Display an informational feedback message to the user[cite: 1].

3. **Pre-Save Integrity Guard (`onSubmit` Client Script):**
   * Prevent saving a high-impact incident if `Assigned To` is left unpopulated[cite: 1].
   * Display targeted error message directly under the missing field[cite: 1].

4. **List Editing Governance (`onCellEdit` Client Script):**
   * Intercept list view updates to the `State` field[cite: 1].
   * Block list-level state updates and prompt users to perform state changes on the incident form[cite: 1].

---

### 5. Targeted Competencies
* **ServiceNow ITSM Incident Management**[cite: 1]
* **Client-Side Platform Architecture** (UI Policies vs. Client Scripts)[cite: 1]
* **Form UI Manipulation & Dynamic UX Design**[cite: 1]
* **Data Validation & Governance Strategies**[cite: 1]
