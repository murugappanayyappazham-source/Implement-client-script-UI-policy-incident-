# 04_Implementation_and_Testing.md

## Phase 4: Implementation, Verification & Testing

### 1. Executive Summary
This document provides the step-by-step implementation guide, validation instructions, test cases, and deployment verification for the **Implement Client Script & UI Policy (Incident)** project. It serves as a blueprint for ServiceNow administrators and developers to build, test, and audit all client-side configurations across target instances.

---

### 2. Step-by-Step Implementation Guide

#### Step 1: Create the UI Policy (`High Impact Control`)
1. Navigate to **Service Catalog / System Policy > Rules > UI Policies** in ServiceNow.
2. Click **New** and configure the header parameters:
   * **Table:** `Incident [incident]`
   * **Short Description:** `High Impact Control`
   * **Global:** `true`
   * **On load:** `true`
   * **Reverse if false:** `true`
3. Under **Conditions**, set: `[Impact] [is] [1 - High]`.
4. Click **Submit** or **Save**.
5. Scroll to the **UI Policy Actions** related list and add two actions:
   * **Action 1:** Field Name: `assignment_group` | Mandatory: `True` | Read only: `False` | Visible: `Leave alone`
   * **Action 2:** Field Name: `urgency` | Mandatory: `Leave alone` | Read only: `True` | Visible: `Leave alone`

#### Step 2: Create the `onChange` Client Script (Auto-Set Urgency)
1. Navigate to **System Definition > Client Scripts** and click **New**.
2. Populate parameters:
   * **Name:** `Auto set urgency for high impact`
   * **Table:** `Incident [incident]`
   * **UI Type:** `All`
   * **Type:** `onChange`
   * **Field name:** `impact`
   * **Active:** `true`
3. Insert script logic into the script field:
   ```javascript
   function onChange(control, oldValue, newValue, isLoading) {
       if (isLoading || newValue == '') {
           return;
       }
       
       if (newValue == '1') {
           g_form.setValue('urgency', '1');
           g_form.addInfoMessage('Urgency set to High for High impact incident.');
       }
   }
