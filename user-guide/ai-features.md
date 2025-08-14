# Using AI Features

Data Explorer leverages AI to simplify pipeline creation and data transformation using natural language instructions.

## 1. Natural Language Pipeline Creation

Ask questions in plain English and AI automatically builds complete pipelines.

### Examples

**Question**: "Show me all maintenance technicians and their assigned locations"

**Generated Pipeline**:

* **Source**: Users table
* **Filter**: Role = "Maintenance Technician"
* **Join**: Locations table on LocationID
* **Select**: UserName, Role, LocationName, LocationAddress

\
**Question**: "List all critical assets that are overdue for inspection"

\
**Generated Pipeline**:

* **Source**: Assets table
* **Filter**: CriticalityLevel = "Critical" AND LastInspectionDate < (Today - 90 days)
* **Join**: Locations table on LocationID
* **Select**: AssetName, AssetType, LocationName, LastInspectionDate, DaysOverdue

***

## 2. Copilot Assistant

Access the Copilot feature from the top-left corner to get AI assistance while building pipelines.

### Usage Examples

**Scenario**: You have a Users table and want to add filtering

**Copilot Input**: "Filter for users who are active and in the Engineering department"

**AI Action**: Adds a **Filter** step with conditions:

* Status = "Active"
* Department = "Engineering"

**Scenario**: You want to enhance your asset query

**Copilot Input**: "Add location details and sort by asset criticality"

**AI Action**:

* Adds a **Join** step to connect with the Locations table
* Adds a **Sort** step by CriticalityLevel (High to Low)

***

## 3. Custom Transform

Transform data using natural language descriptions without writing formulas.

### Examples

**Data**: Assets with LastMaintenanceDate field

**Transform Input**: "Calculate days since last maintenance and add a status column showing 'Overdue' if more than 60 days, 'Due Soon' if 45-60 days, otherwise 'Current'"

**Result**: Adds two columns:

* DaysSinceLastMaintenance: 45, 78, 23, 95
* MaintenanceStatus: "Due Soon", "Overdue", "Current", "Overdue"

**Data**: Users with JoinDate field

**Transform Input**: "Add a column showing years of service rounded to 1 decimal place"

**Result**: Adds YearsOfService column: 2.3, 5.7, 0.8, 3.2

***

## 4. Custom Expand

Convert single rows into multiple rows based on natural language instructions.

### Examples

**Source Data**:

| LocationName | AssignedUsers |
| ------------ | ------------- |
| Building A   | John,Mary,Tom |
| Building B   | Alice,Bob     |

**Expand Input**: "Split the AssignedUsers field so each user gets their own row"

**Result**:

| LocationName | AssignedUser |
| ------------ | ------------ |
| Building A   | John         |
| Building A   | Mary         |
| Building A   | Tom          |
| Building B   | Alice        |
| Building B   | Bob          |

**Source Data**:

| AssetID | MaintenanceSchedule      |
| ------- | ------------------------ |
| PUMP001 | Weekly,Monthly,Quarterly |

**Expand Input**: "Create separate rows for each maintenance type"

**Result**:

| AssetID | MaintenanceType |
| ------- | --------------- |
| PUMP001 | Weekly          |
| PUMP001 | Monthly         |
| PUMP001 | Quarterly       |

## Best Practices

* **Be specific**: Clear instructions produce better results
* **Use examples**: Include sample inputs/outputs when describing complex transformations
* **Iterate**: Use Copilot to refine pipelines step by step
* **Test incrementally**: Run pipelines after each AI-generated step to verify results
