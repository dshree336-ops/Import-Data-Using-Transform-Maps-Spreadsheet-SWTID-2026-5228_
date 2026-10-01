# Import Data using Transform Maps (Spreadsheet)

Importing employee data from an Excel spreadsheet into **ServiceNow** using **Import Sets** and **Transform Maps**, with **Coalesce** to prevent duplicate records and **Reports and a Dashboard** to show the clean data.

**Team ID:** SWTID-2026-5228
**Platform:** ServiceNow Personal Developer Instance (PDI)

## Team

| Name | Role | Contribution |
|---|---|---|
| Divya Shree S | Team Leader | Data transformation and validation |
| Harini V | Team Member | Spreadsheet, tables and Import Set table setup |
| Harshini S | Team Member | Transform Map, reports and dashboard |
| Harshini K | Team Member | Duplicate prevention (Coalesce) |
| Harshini P | Team Member | Data insertion (new data in Excel format) |

## Project Deliverables

### 1. Ideation Phase
- **Problem statement:** How might we import bulk employee data from Excel into ServiceNow accurately and without duplicate records?
- Empathy map, brainstorming and idea prioritization completed.
- **Top priorities:** create the Transform Map, enable Coalesce on Employee ID, and validate the records.

### 2. Requirement Analysis
- Customer journey map with 5 stages: Entice, Enter, Engage, Exit and Extend.
- 7 functional requirements (table setup, data import, transform and mapping, duplicate prevention, validation, reporting, access control) and 6 non-functional requirements.
- Data flow: Excel file → Load Data → Import Set table → Transform Map (Coalesce) → Employee Test table → Reports and Dashboard.
- **Tech stack:** ServiceNow PDI, Import Sets, Transform Maps, Reports and Dashboards, Excel / Google Sheets.

### 3. Project Design Phase
- Problem-solution fit and proposed solution completed.
- **Solution:** load the Excel file into a staging table, map the fields, enable Coalesce on Employee ID, then show the data in reports on a dashboard.

### 4. Project Planning Phase
- 3 sprints of 6 days each, with 16 user stories and 36 story points.
- **Velocity:** 12 story points per sprint (2 per day).

### 5. Project Development Phase
- **Target table:** Employee Test (`u_employee_test`)
- **Staging table:** Employee Import (`u_employee_import`)
- **Transform Map:** Sample Spreadsheet Import, with Coalesce on Employee ID
- **Reports:** Employees by Department (pie), Employees by Location (bar), Employee List Report
- **Dashboard:** Employee Analytics Dashboards
- **Testing:** 11 of 11 UAT test cases passed. Import accuracy was 100% (5 of 5 rows). Re-importing the same sheet gave 0 inserts, 0 updates and 4 ignored, so no duplicates were created.

### 6. Project Documentation
- Final Project Report
- FSD Project Documentation
- **Demo video:** [add link here]
- **Dataset (Google Sheet):** [add link here]

## Future Scope
- Schedule imports automatically.
- Check the sheet for blanks, typos and duplicate IDs before loading.
- Add a role and ACLs for the Employee Test table.
