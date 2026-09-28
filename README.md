# README.md

# ServiceNow Incident Governance: Client Scripts & UI Policies

A comprehensive, production-ready ServiceNow ITSM configuration suite implementing robust client-side governance on the **Incident (`incident`)** table. 

This project enforces strict data integrity rules, automates priority field mapping, and safeguards lifecycle states from unauthorized inline modifications using a hybrid blend of declarative **UI Policies** and targeted **Client Scripts**.

---

## 📑 Project Structure & Documentation Suite

This repository contains the complete 7-phase project documentation lifecycle:

| File Name | Phase Description |
| :--- | :--- |
| [`01_Project_Planning_and_Management.md`](./01_Project_Planning_and_Management.md) | WBS, execution roadmap, milestones, and risk management matrix. |
| [`02_Requirement_Analysis.md`](./02_Requirement_Analysis.md) | Business requirements, functional specifications, and technical prerequisites. |
| [`03_Project_Design.md`](./03_Project_Design.md) | Technical architecture, event lifecycle, and data sequence flows. |
| [`04_Project_Demonstration.md`](./04_Project_Demonstration.md) | End-to-end execution narratives, UI interaction matrices, and walkthrough links. |
| [`05_Project_Development.md`](./05_Project_Development.md) | Build execution log, field property settings, and code artifacts. |
| [`06_Project_Testing.md`](./06_Project_Testing.md) | Unit test matrix, UAT scenarios, and defect verification log. |
| [`07_Project_Documentation.md`](./07_Project_Documentation.md) | Deployment runbook, admin handoff package, and maintenance guide. |

---

## 🛠️ Tech Stack & Key Components

* **Platform:** ServiceNow ITSM (Incident Management)
* **UI Types Supported:** Desktop, Mobile, and Service Portal (`UI Type: All`)
* **API Standards:** Standard ServiceNow Client APIs (`g_form`, `g_user`)

### Configured Artifacts

1. **UI Policy (`High Impact Control`)**
   * **Trigger:** `Impact == '1' (1 - High)`
   * **Actions:** Sets `Assignment group` to **Mandatory** and `Urgency` to **Read-Only**.
2. **Client Script: `onChange` (`Auto set urgency for high impact`)**
   * **Field:** `impact`
   * **Action:** Automatically sets `Urgency` to `'1'` and displays an info banner when high impact is selected.
3. **Client Script: `onSubmit` (`Prevent save if Assigned To missing`)**
   * **Action:** Intercepts record save events for high-impact incidents and blocks submission if `Assigned To` is empty.
4. **Client Script: `onCellEdit` (`Prevent state change via list edit`)**
   * **Field:** `state`
   * **Action:** Blocks direct cell editing on incident list views, enforcing record updates through the form UI.

---

## 🚀 Quick Deployment Guide

1. Clone or download the Update Set XML file (`Incident_Client_Governance_v1.0.xml`) from the repository releases.
2. Log in to your ServiceNow instance as a System Administrator.
3. Navigate to **System Update Sets > Retrieved Update Sets**.
4. Import the update set XML file, run **Preview Update Set**, and resolve any collisions.
5. Click **Commit Update Set** to deploy all configurations to the target environment.

---

## 🔬 Testing & Verification

Refer to [`06_Project_Testing.md`](./06_Project_Testing.md) for the complete 7-step unit test execution suite covering both positive and edge-case form interaction behaviors.
