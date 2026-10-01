# Import Data using Transform Maps (Spreadsheet)

Importing employee data from an Excel spreadsheet into **ServiceNow** using **Import Sets** and **Transform Maps**, with **Coalesce** to prevent duplicate records, and **Reports and a Dashboard** to present the clean data.

| | |
|---|---|
| **Team ID** | SWTID-2026-5228 |
| **Platform** | ServiceNow Personal Developer Instance (PDI) |
| **Team size** | 5 |
| **Date** | 01 October 2026 |

## Team

| Name | Role | Contribution |
|---|---|---|
| Divya Shree S | Team Leader | Data transformation and validation |
| Harini V | Team Member | Spreadsheet, tables and Import Set table setup |
| Harshini S | Team Member | Transform Map, reports and dashboard |
| Harshini K | Team Member | Duplicate prevention (Coalesce) |
| Harshini P | Team Member | Data insertion (new data in Excel format) |

## Project Overview

This micro project loads structured data from an Excel sheet into ServiceNow. The sheet is first loaded into an Import Set (staging) table. A Transform Map then maps the source fields to the target table (Employee Test), and running the transform creates the records. Coalesce on **Employee ID** means that importing the same employee again updates the existing record instead of creating a duplicate. The imported data is checked in the Employee Test table and shown in three reports on the **Employee Analytics Dashboards** dashboard.

**Key results**

- Load Data: State Complete, Completion code Success. Processed 5, inserts 5, errors 0 (**100% import accuracy**).
- Re-import with Coalesce enabled: Total 4, Inserts 0, Updates 0, **Ignored 4**, Errors 0 (no duplicates).
- **11 of 11** UAT test cases passed, with no defects recorded.

## Project Deliverables

1. [Ideation Phase](#1-ideation-phase)
2. [Requirement Analysis](#2-requirement-analysis)
3. [Project Design Phase](#3-project-design-phase)
4. [Project Planning Phase](#4-project-planning-phase)
5. [Project Development Phase](#5-project-development-phase)
6. [Project Documentation](#6-project-documentation)

---

## 1. Ideation Phase

### 1.1 Problem Statement

> **How might we import bulk employee data from Excel into ServiceNow accurately and without duplicate records?**

| PS | I am | I'm trying to | But | Because | Which makes me feel |
|---|---|---|---|---|---|
| PS-1 | A data administrator | Import bulk employee data from Excel into ServiceNow and keep it up to date | Duplicate employee records get created and old details are not updated | The transform map has no coalesce field (Employee ID) to match existing employees | Frustrated and worried about data accuracy |
| PS-2 | A manager or HR team member | View accurate employee reports by department and location | The report counts look inflated and some employee details are outdated | Duplicate records from repeated imports are counted more than once | Confused and unsure which numbers to trust |

### 1.2 Empathy Map Canvas

| Quadrant | What the administrator experiences |
|---|---|
| **Think & Feel** | Will this import create duplicates? Is my employee data accurate? Which field should I coalesce on? |
| **Say & Do** | Uploads the Excel file using Load Data, runs the transform, checks the history and compares rows with the table |
| **Hear** | "Fix the duplicates before the report goes out." "Just re-import the new sheet." |
| **See** | The same employee appearing twice, old details after a new import, inflated report counts |
| **Pain** | Duplicate employee records, time spent cleaning data by hand, outdated details |
| **Gain** | Accurate employee data, updates instead of duplicates (Coalesce), reliable reports |

### 1.3 Brainstorming and Idea Prioritization

Ideas were grouped into four themes:

| Theme | Ideas |
|---|---|
| Data preparation | Sample employee spreadsheet, custom table `u_employee_test`, Import Set staging table |
| Mapping & transform | Create the Transform Map, auto-map matching fields, run the transform and check the history |
| Data quality | Coalesce on Employee ID, validate records after the transform, re-import to confirm no duplicates |
| Reporting | Pie chart by department, bar chart by location, add reports to a dashboard |

**Top priorities** (Importance vs Feasibility grid): create the Transform Map, enable Coalesce on Employee ID, and validate the records after the transform. Reports and dashboard ideas were easy to build but lower in importance, so they came after the core import worked.

---

## 2. Requirement Analysis

### 2.1 Customer Journey Map

| Stage | Steps | Pain points |
|---|---|---|
| **1. Entice** | Receive the Excel sheet, review the data, plan the import | Blank cells and typos; unsure which field is unique |
| **2. Enter** | Create the Employee Test table and fields, load the Excel file with Load Data, create the Import Set table | Fields added one by one; wrong sheet number or header row causes errors |
| **3. Engage** | Create the Transform Map, map fields (Auto Map and Mapping Assist), enable Coalesce, run the transform | Easy to map a wrong column; duplicates without Coalesce |
| **4. Exit** | Check Transform History, validate records, re-import the same sheet to test | Columns need rearranging; old duplicates removed by hand |
| **5. Extend** | Create reports, add them to a dashboard, reuse the Transform Map for new files | Duplicates inflate report counts |

### 2.2 Solution Requirements

**Functional requirements**

| FR No. | Epic | Sub requirements |
|---|---|---|
| FR-1 | Table & Data Setup | Create the Employee Test table (`u_employee_test`), add the fields Employee ID, Employee Name, Email, Department, Location; prepare the Excel sheet with a header row |
| FR-2 | Data Import | Upload the Excel file with System Import Sets > Load Data; create the Import Set table Employee Import (`u_employee_import`); check the loaded rows |
| FR-3 | Transform & Field Mapping | Create the Transform Map "Sample Spreadsheet Import"; map fields with Auto Map Matching Fields and Mapping Assist; run the transform and check Transform History |
| FR-4 | Duplicate Prevention | Enable Coalesce on Employee ID; re-import the same sheet to confirm no duplicates |
| FR-5 | Data Validation | Validate the imported records in the Employee Test table; check all columns in the list view |
| FR-6 | Reporting & Dashboard | Pie chart by department, bar chart by location, Employee List Report, all added to the Employee Analytics Dashboards dashboard |
| FR-7 | Access Control (Roles & ACL) | Planned: role and ACLs for the Employee Test table (not covered in this release's UAT) |

**Non-functional requirements**

| NFR No. | Requirement | Description |
|---|---|---|
| NFR-1 | Usability | Uses standard ServiceNow modules; Auto Map matches most fields instantly; a saved Transform Map can be reused |
| NFR-2 | Security | Access can be controlled with a role and ACLs (planned, not covered in this UAT) |
| NFR-3 | Reliability | Coalesce updates existing records instead of creating duplicates; Transform History shows inserted, updated and ignored counts |
| NFR-4 | Performance | Spreadsheet loads and transforms within seconds, with a success message |
| NFR-5 | Availability | Available whenever the ServiceNow instance is running |
| NFR-6 | Scalability | The same Transform Map can be reused for new spreadsheets; more fields and scheduled imports can be added |

### 2.3 Data Flow Diagram

1. The sales, HR or support team sends the employee data as an Excel sheet to the data administrator.
2. The administrator reviews the sheet and creates the Employee Test table and fields in ServiceNow.
3. The administrator uploads the file with **System Import Sets > Load Data**, which loads the rows into the Import Set (staging) table.
4. A Transform Map maps source fields to target fields, with **Coalesce on Employee ID**.
5. The transform runs: new employees are inserted, existing employees are updated, and Transform History records the counts.
6. The administrator validates the data in the Employee Test table, and the viewer opens the reports and dashboard.

| Element | Description |
|---|---|
| External entities | Data source (Excel / Google Sheet), data administrator, dashboard viewer |
| Processes | 1 Load Data (Import Set), 2 Transform and Map (Coalesce), 3 Reports and Dashboard |
| Data stores | Employee Import (`u_employee_import`), Transform History, Employee Test (`u_employee_test`) |

### 2.4 Technology Stack

| Component | Technology |
|---|---|
| User Interface | ServiceNow web UI in a browser |
| Application logic | Import Sets (Load Data), Transform Maps (Auto Map, Mapping Assist, Coalesce), Reports and Dashboards |
| Database | ServiceNow tables (Employee Test, Employee Import) on the PDI database |
| File storage | Google Sheets / Microsoft Excel (.xlsx) from the local computer |
| External API / ML model | Not used |
| Infrastructure | ServiceNow-hosted Personal Developer Instance (cloud) |

---

## 3. Project Design Phase

### 3.1 Problem Solution Fit

| Canvas element | Description |
|---|---|
| Customer segment | Data administrators importing employee data from Excel; managers and HR viewing reports; team members sending the sheets |
| Jobs-to-be-done | Import data without missing rows, update instead of duplicating, avoid wrong mappings, keep reports accurate |
| Triggers | A new Excel sheet arrives; report counts look inflated; duplicate or outdated details are noticed |
| Emotions | Before: frustrated and worried. After: confident and trusting the data |
| Root cause | The transform map has no coalesce field (Employee ID), so every repeated import inserts the same employees again |
| Our solution | Import with an Import Set and a Transform Map, enable Coalesce on Employee ID, validate the result and show clean data in reports on a dashboard |

### 3.2 Proposed Solution

| Parameter | Description |
|---|---|
| Problem statement | Without a coalesce field such as Employee ID, the transform creates duplicates and does not update old details, so report counts cannot be trusted |
| Solution | Load the Excel file into a staging table, map fields with Auto Map and Mapping Assist, enable Coalesce on Employee ID, check Transform History and the Employee Test table, then show the data in reports on a dashboard |
| Novelty | Uses only built-in ServiceNow features, with no extra code; a saved Transform Map is reusable; a re-import test proves no duplicates |
| Business model | An internal efficiency tool on the existing ServiceNow platform; can be packaged as a reusable import template |
| Scalability | Reuse the Transform Map, add fields, schedule imports, extend to other tables and teams |

### 3.3 Solution Architecture

```
Excel / Google Sheet (.xlsx)
        |  (1) prepare sheet   (2) upload with Load Data
        v
Import Set (staging) table: Employee Import (u_employee_import)
        |  (3) rows saved   (4) Transform Map reads and maps fields
        v
Transform Map: Sample Spreadsheet Import  --(5)-->  Transform History
        |  (6) insert / update using Coalesce on Employee ID
        v
Employee Test table (u_employee_test)
        |  (7) reports read the table
        v
Reports (pie, bar, list)  -->  Employee Analytics Dashboards
```

---

## 4. Project Planning Phase

Three sprints of six days each. Planned scope: **36 story points** across **16 user stories**.

| Sprint | Epic | USN | User story / task | Points | Priority | Team member |
|---|---|---|---|---|---|---|
| Sprint-1 | Table & Data Setup | USN-1 | Create the Employee Test custom table | 2 | High | Harini V |
| | | USN-2 | Add the employee fields with the right types | 2 | High | Harini V |
| | | USN-3 | Prepare an Excel sheet with a header row matching the table fields | 3 | High | Harini V |
| | Import Data | USN-4 | Upload the Excel file using Load Data | 1 | High | Harini V |
| | | USN-5 | Open the staging table and check the loaded rows | 1 | Medium | Harini V |
| | Data Submission | USN-11 | Send the latest employee data in an Excel sheet to the administrator | 1 | Medium | Harshini P |
| Sprint-2 | Transform & Mapping | USN-6 | Create a Transform Map from the Import Set table to Employee Test | 2 | High | Harshini S |
| | | USN-7 | Map fields using Auto Map Matching Fields and Mapping Assist | 3 | High | Harshini S |
| | | USN-8 | Run the transform and check the Transform History | 2 | High | Divya Shree S |
| | Duplicate Prevention | USN-9 | Enable Coalesce on Employee ID | 5 | High | Harshini K |
| Sprint-3 | Validation | USN-10 | Validate the imported records in the Employee Test table | 3 | Medium | Divya Shree S |
| | Reporting | USN-12 | Create a pie chart of employees by department | 2 | Medium | Harshini S |
| | | USN-13 | Create a bar chart of employees by location | 2 | Medium | Harshini S |
| | | USN-14 | Show the saved reports together on a dashboard | 2 | Low | Harshini S |
| | Security (Roles & ACL) | USN-15 | Create a role and assign it to a user | 2 | Medium | Divya Shree S |
| | | USN-16 | Create ACLs giving read and write access to the table only to that role | 3 | Medium | Divya Shree S |

> USN-15 and USN-16 were planned in Sprint-3 but are not covered in the UAT of this release.

**Sprint schedule**

| Sprint | Story points | Duration | Start date | End date (planned) | Cumulative points |
|---|---|---|---|---|---|
| Sprint-1 | 10 | 6 days | 29 Sep 2026 | 04 Oct 2026 | 10 |
| Sprint-2 | 12 | 6 days | 05 Oct 2026 | 10 Oct 2026 | 22 |
| Sprint-3 | 14 | 6 days | 12 Oct 2026 | 17 Oct 2026 | 36 |

**Velocity:** 36 story points / 3 sprints = **12 story points per sprint**, which is **2 story points per day**.

---

## 5. Project Development Phase

No source code was written. The solution is configured with built-in ServiceNow features.

### 5.1 Setup and Implementation Steps

1. **Prepare the sheet:** create a spreadsheet with the header row `Employee ID, Name, Email, Department, Location` and some employee rows, then download it as `Sample Spreadsheet.xlsx`.
2. **Create the table:** Tables > Create New, label **Employee Test** (`u_employee_test`). In Form Layout, add the String fields Employee ID, Employee Name, Email, Department and Location.
3. **Load the data:** System Import Sets > Load Data, choose *Create table*, label **Employee Import** (`u_employee_import`), choose the file, Sheet number 1, Header row 1, then Submit.
4. **Create the Transform Map:** on the success page click *Create Transform Map*, name it **Sample Spreadsheet Import**, set the target table to Employee Test, click *Auto Map Matching Fields*, use *Mapping Assist* for the remaining fields, and Save.
5. **Enable Coalesce:** in the Field Maps related list, set Coalesce to `true` on the Employee ID field map.
6. **Run the transform:** click Transform, select the Import Set and Transform Map, and run it. Check the Transform History.
7. **Validate:** open the Employee Test table and use *Personalize List Columns* to arrange the columns.
8. **Create the reports:** Reports > New for Employee Test: pie chart grouped by Department, bar chart grouped by Location, and a list report with all five columns.
9. **Create the dashboard:** create **Employee Analytics Dashboards** and add the three reports with Share > Add to Dashboards.

> A PDI that has been idle goes to sleep and must be woken from the developer site before use.

### 5.2 Artefacts Created on the Instance

| Artefact | Name / details |
|---|---|
| Target table | Employee Test (`u_employee_test`): Employee ID, Employee Name, Email, Department, Location |
| Import Set (staging) table | Employee Import (`u_employee_import`) |
| Transform Map | Sample Spreadsheet Import, with Coalesce on Employee ID |
| Reports | Employees by Department (pie), Employees by Location (bar), Employee List Report (list) |
| Dashboard | Employee Analytics Dashboards |

### 5.3 Field Mapping

| Source field (Employee Import) | Target field (Employee Test) | Coalesce |
|---|---|---|
| `u_employee_id` | `u_employee_id` | **true** |
| `u_name` | `u_employee_name` | false |
| `u_email` | `u_email` | false |
| `u_department` | `u_department` | false |
| `u_location` | `u_location` | false |

### 5.4 Testing and Results (UAT)

Manual functional testing on the ServiceNow PDI. **11 test cases, 11 passed, 0 failed.**

| Test case | Scenario | Actual result | Result |
|---|---|---|---|
| TC-001 | Create the Employee Test table with its fields | Form shows all five fields | Pass |
| TC-002 | Upload the Excel file using Load Data | Complete / Success. Processed 5, inserts 5, errors 0 | Pass |
| TC-003 | Create the Transform Map and map the fields | 5 field maps created | Pass |
| TC-004 | Run the transform | Complete / Success | Pass |
| TC-005 | Validate the imported records | All 5 records shown in the list view | Pass |
| TC-006 | Import an updated sheet with a new employee | 6 records; SB0006 added, SB0001 to SB0005 appear once each | Pass |
| TC-007 | Re-import the same sheet (duplicate check) | Total 4, Inserts 0, Updates 0, Ignored 4, Errors 0 | Pass |
| TC-008 | Employees by Department pie chart | ServiceNow 11, Salesforce 4, Aiml 2 (17 employees) | Pass |
| TC-009 | Employees by Location bar chart | Hyderabad 16, Chennai 1 (17 employees) | Pass |
| TC-010 | Employee List Report | 17 of 17 records with all five columns | Pass |
| TC-011 | Add the reports to the dashboard | All three reports appear on the dashboard | Pass |

---

## 6. Project Documentation

| Document | Description |
|---|---|
| [Final Project Report](./6-Project-Documentation/Final_Project_Report.docx) | Introduction, ideation, requirements, design, planning, testing, results, advantages and disadvantages, conclusion, future scope and appendix |
| [FSD Project Documentation](./6-Project-Documentation/FSD_Project_Documentation.docx) | Full Stack Development style documentation: architecture, database schema, setup instructions, platform operations, authentication, UI screenshots, testing, known issues and future enhancements |

**Links**

- **Project demo video:** [add the demo video link here]
- **Dataset (Google Sheet):** [add the Google Sheet link here]

### Advantages

- Built-in features only, with no custom code or extra tools.
- No duplicate records: Coalesce on Employee ID updates existing employees.
- Fast and accurate import, with exact counts in Transform History.
- Reusable Transform Map for new spreadsheets with the same columns.
- Reliable reports and dashboard built on clean data.
- Easy for beginners to learn.

### Known Issues and Limitations

- Importing without Coalesce on Employee ID creates duplicates, so enable it before re-importing.
- The sheet needs a header row that matches the fields, and the correct sheet number and header row must be entered in Load Data.
- Mapping Assist can map a wrong column if the fields are not checked afterwards.
- Each employee needs a unique Employee ID for Coalesce to work.
- The role and ACLs for the Employee Test table (FR-7) are planned but not covered in this release.
- Tested on a developer instance with a small sample dataset.

### Future Scope

- Schedule imports so new spreadsheets load automatically.
- Check the sheet for blanks, typos and duplicate Employee IDs before loading.
- Add a role and ACLs for the Employee Test table and test access by impersonating users.
- Use transform scripts to clean data and add more fields.
- Reuse the approach for other tables and teams.
- Add reports and notifications on import activity.

### Conclusion

The project imported employee data from a spreadsheet into ServiceNow with an Import Set and a Transform Map, and kept the data clean with Coalesce on Employee ID. A re-import of the same sheet inserted and updated nothing, and all 11 UAT test cases passed with no defects.

---

## Repository Structure

```
Import-Data-Using-Transform-Maps-Spreadsheet/
├── README.md
├── 1-Ideation-Phase/
├── 2-Requirement-Analysis/
├── 3-Project-Design-Phase/
├── 4-Project-Planning-Phase/
├── 5-Project-Development-Phase/
└── 6-Project-Documentation/
    ├── Final_Project_Report.docx
    └── FSD_Project_Documentation.docx
```
