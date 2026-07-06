#🔐 Enterprise Sales Pipeline | Regional Access & Performance Analytics
> **A production-ready Power BI dashboard built with Dynamic Row-Level Security (RLS) — one report, four regions, fully controlled access. Each manager sees only their data. Leadership sees everything.**
---
Problem Statement
A company with four regional sales managers had a critical data governance gap — every manager could see every region's performance data inside the same Power BI report.
This meant:
Regional managers seeing competitors' (internal) numbers
Sensitive financial data exposed across the entire team
No control over who sees what
Risk of bad decisions based on data that wasn't theirs to act on
The traditional fix — build one report per region — creates a maintenance nightmare. Every update gets multiplied. Every change breaks four places.
The solution: One report. Dynamic RLS. Zero data leakage.
---
Project Objectives
Build a single unified sales dashboard for all four regions
Apply Dynamic Row-Level Security so each manager sees only their own region
Give leadership (CEO, CTO, COO) full unrestricted access in the same report
Add Month-on-Month trend tracking for Total Sales and Total Profit
Make the system scalable — new hire means one table update, not a report rebuild
---
✅ Expected Outcome
User	Access
manager.east@company.com	East region only
manager.west@company.com	West region only
manager.north@company.com	North region only
manager.south@company.com	South region only
ceo@company.com	All regions
cto@company.com	All regions
coo@company.com	All regions
---
Dashboard Preview
Overview — Full Access (Leadership View)
![Overview](Sales%20Pipeline%20Overview.png)
---
Regional View — East Manager (RLS Applied)
![RLS Regional View](RLS_Regional_View.png)
---
📁 Dataset Structure
The dataset is built as a star schema with the following tables:
Table	Rows	Description
`SalesData`	200	Core fact table — OrderID, Region, SalesRep, Product, TotalSales, Profit, OrderDate
`UserMapping`	4	Maps each manager's login email to their Region — powers the RLS logic
`DateTable`	—	Calendar table for time intelligence and MoM calculations
UserMapping Table (RLS Driver)
Email	Name	Region	Role
manager.east@company.com	Alice Johnson	East	Regional Manager
manager.west@company.com	Carlos Ruiz	West	Regional Manager
manager.north@company.com	Ethan Brown	North	Regional Manager
manager.south@company.com	George King	South	Regional Manager
> Leadership users (CEO, CTO, COO) are assigned directly in Power BI Service — not stored in UserMapping.
---
Tech Stack
Tool	Purpose
Power BI Desktop	Data modelling, DAX, dashboard development
DAX	KPI measures, RLS logic
Microsoft Excel	Raw data source
Power BI Service	Publishing, role assignment, access control
GitHub	Version control and portfolio documentation
---
🔐 RLS Implementation
Approach: Dynamic RLS with LOOKUPVALUE
Instead of creating one static role per region (which doesn't scale), this project uses Dynamic RLS — a single role that adapts based on whoever is logged in.
Role: `RegionalManager`
Applied on the `SalesData` table:
```dax
[Email] = USERPRINCIPALNAME()

```
How it works:
```
User logs in to Power BI
        ↓
USERPRINCIPALNAME() reads their login email
        ↓
Returns their assigned Region (e.g. "North")
        ↓
SalesData is filtered → Region = "North" only
        ↓
User sees North data only — nothing else is visible
```
Role: `Leadership`
No DAX filter — blank rule. Leadership users assigned in Power BI Service see all regions with zero restrictions.
---
📐 DAX Measures
Base Measures
```dax
Total Sales = SUM( SalesData[TotalSales] )
```
```dax
Total Profit = SUM( SalesData[Profit] )
```
Key Insights
South region leads in Total Sales at $332,565 — the strongest performing region
East region has the lowest sales at $195,470 — opportunity for strategy review
Laptops and Smartphones are the top two products across all regions by revenue
Notebooks generate the lowest revenue — likely high volume, low margin
MoM tracking reveals clear seasonal dips in June and September across the business
---
📁 Files in This Repo
File	Description
`RLS_Sales_Dataset_v2.xlsx`	Raw dataset — SalesData, UserMapping, DateTable
`Enterprise_Sales_Pipeline_RLS.pbix`	Power BI report file
`Sales Pipeline Overview.png`	Dashboard screenshot — Leadership full view
`RLS_Regional_View.png`	Dashboard screenshot — Regional Manager filtered view
---
How to Use This Project
Clone or download this repository
Open `Enterprise_Sales_Pipeline_RLS.pbix` in Power BI Desktop
If prompted, point the data source to `RLS_Sales_Dataset_v2.xlsx`
Go to Modeling → View as to test roles:
Select `RegionalManager` → tick Other user → enter any manager email
Select `Leadership` → confirm full data visibility
To publish: Home → Publish → assign emails to roles in Power BI Service under dataset Security
---
About
Developed by Olumide
Data Analyst & BI Engineer — building production-ready dashboards that solve real business problems.
![LinkedIn](https://www.linkedin.com/posts/olumide-david-79b17726a_powerbi-rowlevelsecurity-dataanalytics-activity-7472246826234974209-Btob?utm_source=share&utm_medium=member_desktop&rcm=ACoAAEHgVqcBelhbBzhkIpF2JSlZX6PrAp_NLVU)
