# Carbon-Footprint-Estimation-Dashboard
Carbon Footprint Dashboard integrating Python and Power BI to analyze business activity emissions and generate decarbonization strategies using rule-based AI.

## Problem Statement

Businesses today need to measure and reduce their greenhouse gas (GHG) emissions across Scope 1 (direct fuel), Scope 2 (electricity), and Scope 3 (commute, waste). However, traditional dashboards only visualize data — they don’t suggest actionable insights. This project addresses the gap by combining **Power BI** for dynamic emission reporting and **Python-based AI engine** for recommending decarbonization strategies for any given Company/Organization.


### Steps followed 

#### 1. Python (Jupyter Notebook) for Data Preparation
- Step 1 : Defined **lists** for Months, Locations and Energy Sources.
- Step 2 : Simulated monthly activity data resembling **IoT** outputs for Waste Generated (kg), Commute Distance (km), LPG (kg), Diesel Used (L), Electricity Used (kWh). Used python module "random" for specifying values to emission activities, Location, Energy Source, Employee Count, and Revenue.
- Step 3 : Created **dataframe** including 12 months (Jan - Dec) as rows and Location, Energy Sources, Revenue, Employee Count, Waste Generated (kg), Commute Distance (km), LPG (kg), Diesel Used (L), Electricity Used (kWh) as columns.
- Step 4 : Defined **dictionary** for Emission factors for all the 5 emission activities. Then calculated emission value for each of the 5 activities.
- Step 5 : Calculated Total emissions as sum of all emissions monthly.
- Step 6 : Calculated Emissions per Employee and Emission per Rs. Crore Revenue.
- Step 7 : Built a **rule-based AI engine** using Python `if-else` logic to generate recommendations.
<p align="center"><img src="https://github.com/user-attachments/assets/21d2b2f8-acd7-41e2-a5f1-5dad833a4a79" width="400" height="600"></p>


- Step 8 : Saved and exported this as "**.xlxs**" file as "Carbon_footprint.xlxs". 


####   2. Power BI Dashboard Building
- Step 9 : Loaded the processed Excel dataset with emissions + AI tips.
- Step 10 : Created an excel file "scope.xlxs" using MS Excel. Loaded it in PowerBI. With the help of Power Querry Editor, mapped scope for all the 5 emission activities in the following steps:-
  
   (a) Selected Waste Generated (kg), Commute Distance (km), LPG (kg), Diesel Used (L), Electricity Used (kWh) columns. Unpivoted these columns in the main table.
  
  (b) Renamed Attribute as "Activity" and Value as "Emission (kg CO2e)".
  
  (c) Created a new column "Scope" using the following DAX:-
  
  Scope = SWITCH(TRUE(), CONTAINSSTRING([Activity], "Electricity"), "Scope 2", CONTAINSSTRING([Activity], "Diesel") || CONTAINSSTRING([Activity], "LPG"), "Scope 1", CONTAINSSTRING([Activity], "Commute") || CONTAINSSTRING([Activity], "Waste"), "Scope 3", "Unknown")
  
  (d) In Model View, created relationship between "Activity"(sheet1) and "Activity"(sheet2/ scope table).

- Step 11 : Created a new measure showing which month has the highest emissions using the following DAX:-
  
    max em mnth = VAR SummaryTable = ADDCOLUMNS(VALUES(Sheet1[Months]), "TotalEmissions", CALCULATE(SUM('Sheet1'[Total Emissions (kg CO2e)]))) VAR MaxEmissions = MAXX(SummaryTable, [TotalEmissions]) VAR ResultMonth = CALCULATE(MAX('Sheet1'[Months]), FILTER(SummaryTable, [TotalEmissions] = MaxEmissions)) RETURN ResultMonth

- Step 12 : Built KPIs, line chart, column charts, pie chart, and scatter plot to showcase various analysis of the carbon footprints. Customized layout, color theme, tooltips, and interactions.
- Step 13 : Visual filters (Slicers) were added for 3 fields namely "City", "Months" and "Energy Source".
- Step 14 : Added a dedicated "AI Recommendation" page and linked contextually through visuals.

 
 # Report Snapshot (Power BI DESKTOP)



<img width="1300" height="731" alt="Carbon Footprint(pg1)" src="https://github.com/user-attachments/assets/e00684dd-ee00-452f-95aa-3a0f10610775" />


<img width="1296" height="691" alt="Carbon footprint (AI)" src="https://github.com/user-attachments/assets/da48794b-677e-485d-87ad-c416121b3bc1" />


# Insights

- Electricity was the highest contributor to Scope 2 emissions.
- Higher emissions with employee count between 20-25.
- Seasonal trends indicated higher emissions in summer months (potential overuse of HVAC systems).
- AI Tips included energy-saving upgrades, waste reduction strategies, and transport shifts.


# Challenges Faced and Resolved

-  Power BI visuals didn't sort months chronologically (Jan–Dec), therefore, used custom **Month Number column** to sort visuals chronologically using Power Querry Editor.
-  Circular dependency errors while using DAX for scope classification. Refactored DAX logic to eliminate circular dependency (using calculated columns instead of measures).
-  "Page navigation buttons" not working in Power BI (tried all configurations). Replaced broken navigation buttons with **text-based guidance + clean layout**.
