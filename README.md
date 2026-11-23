Customer Retention & Churn Analysis (Power BI)
> \\\*\\\*Why do customers leave a subscription business, who is most at risk, and what can we do about it?\\\*\\\*
> An end-to-end retention analysis of 7,043 telecom subscribers, built with Power Query, DAX and Power BI.
Project for Future Interns – Data Science & Analytics, Task 2 (2026).
![Dashboard overview](images/01_executive_overview.png)
---
Table of Contents
Business Problem
Headline Findings
Dataset
Tools & Skills
Method
Dashboard Walkthrough
Detailed Insights
Recommendations
Limitations
Repository Structure
How to Open the Dashboard
Author
---
Business Problem
For subscription businesses, losing customers directly reduces recurring revenue. This project answers four questions a product, growth or retention team would ask:
Why are customers leaving?
Which segments are most likely to churn?
How long do customers typically stay?
What actions can improve retention?
---
Headline Findings
Metric	Value
Customers analysed	7,043
Customers who churned	1,869 (26.5%)
Monthly recurring revenue	$456K
Monthly revenue lost to churn	$139K (30.5% of total)
Avg tenure: churned vs. retained	18.0 vs. 37.6 months
Avg monthly bill: churned vs. retained	$74.44 vs. $61.27
The five things to remember:
Contract type is the biggest driver. 89% of all churners were on month-to-month contracts. Month-to-month churn is 42.7%, versus 11.3% (one-year) and 2.8% (two-year).
The first year is the danger zone. 55% of all churners left within their first 12 months, and about 16% of new customers are gone by month 12.
Fiber optic customers churn at more than twice the DSL rate (41.9% vs. 19.0%) while paying more, which points to a value or service-quality problem.
Protection services and auto-pay act like glue. Customers with all four protection add-ons churn at 5.3%, and auto-pay customers churn at 16.0% versus 34.7% for manual payers (electronic check: 45.3%).
One high-risk segment stands out. Month-to-month, fiber, first-year customers: 916 customers, 70.2% churn, responsible for 34% of all churn. 273 of them are still active and worth $22K per month: an immediate retention target list.
---
Dataset
Source: IBM sample Telco Customer Churn dataset (widely available on Kaggle). File: `WA\\\_Fn-UseC\\\_-Telco-Customer-Churn.csv`
Size: 7,043 customers, 21 columns, no duplicate IDs
Content: demographics, account information, subscribed services, billing details and a `Churn` flag
Group	Columns
Demographics	`gender`, `SeniorCitizen`, `Partner`, `Dependents`
Account	`tenure`, `Contract`, `PaperlessBilling`, `PaymentMethod`
Services	`PhoneService`, `MultipleLines`, `InternetService`, `OnlineSecurity`, `OnlineBackup`, `DeviceProtection`, `TechSupport`, `StreamingTV`, `StreamingMovies`
Billing	`MonthlyCharges`, `TotalCharges`
Target	`Churn` (Yes / No)
> \\\*\\\*Note:\\\*\\\* the dataset has no signup date and no churn-reason field. Cohorts are therefore built from \\\*\\\*tenure\\\*\\\*, and churn "reasons" are inferred from the attributes that best separate churned from retained customers.
---
Tools & Skills
Power Query: data cleaning and feature engineering
DAX: KPIs, revenue-at-risk measures, a Kaplan-Meier-style retention curve
Power BI: 4-page interactive dashboard, custom theme
Skills: churn analysis, cohort / survival analysis, segmentation, customer lifetime metrics, business storytelling
---
Method
1. Data cleaning (Power Query)
Issue	Fix
`TotalCharges` stored as text; 11 blank values (all customers with `tenure = 0`, not yet billed)	Replaced blanks with 0, converted to decimal
`SeniorCitizen` coded 0/1	Converted to No/Yes for consistency
"No internet service" and "No phone service" duplicating other columns	Standardised to "No" in 7 columns
2. Engineered features
Column	Purpose
`ChurnFlag`	1/0 version of Churn for rate calculations
`TenureGroup` (+ sort column)	0-12, 13-24, 25-48, 49-72 months
`AutoPay`	Auto-pay vs. manual payment
`ChargeBand`	Low (<$35), Mid ($35-70), High ($70+)
`ProtectionServices`	Count (0-4) of security, backup, device protection and tech support
3. Measures (DAX)
Churn rate, retention rate, revenue lost, revenue at risk %, average tenure and customer value, high-risk segment counts. See `docs/dax\\\_measures.md`.
4. Retention curve
A simple "customers with tenure ≥ m" curve wrongly counts still-active customers as lost, so a Kaplan-Meier-style survival curve is used instead. For each month, it works out the share of at-risk customers who survived and multiplies those shares together. Customers who are still active simply leave the at-risk pool when their history ends.
---
Dashboard Walkthrough
Page 1: Executive Overview
KPI cards (customers, churn rate, retention rate, revenue lost, average tenure), churn vs. retained donut, churn rate by tenure group and by contract, with slicers for contract, internet service and senior citizen.
![Executive Overview](images/01_executive_overview.png)
Page 2: Churn Drivers
Churn rate by contract, internet service, payment method, auto-pay, protection services and monthly charge band, a contract × tenure heatmap, and the Key Influencers visual.
![Churn Drivers](images/02_churn_drivers.png)
Page 3: Customer Lifetime & Cohorts
Retention curves by contract type, average tenure and customer value by contract, revenue lost by segment, and the high-risk segment tracker.
![Customer Lifetime](images/03_customer_lifetime.png)
Page 4: Recommendations
Prioritised actions for the business, tied to the evidence above.
![Recommendations](images/04_recommendations.png)
---
Detailed Insights
Who churns?
Driver	Highest-churn group	Lowest-churn group
Contract	Month-to-month 42.7%	Two-year 2.8% (one-year 11.3%)
Tenure	0-12 months 47.4%	49-72 months 9.5%
Internet service	Fiber optic 41.9%	No internet 7.4% (DSL 19.0%)
Payment method	Electronic check 45.3%	Credit card (auto) 15.2%
Tech support	No 31.2%	Yes 15.2%
Online security	No 31.3%	Yes 14.6%
Protection add-ons	1 service 38.9%	4 services 5.3%
Senior citizen	Yes 41.7%	No 23.6%
Dependents	No 31.3%	Yes 15.5%
Paperless billing	Yes 33.6%	No 16.3%
Monthly charge	$70+ 35.4%	Under $35 10.9%
Gender	Female 26.9%	Male 26.2% (no meaningful effect)
Combinations matter even more than single factors: month-to-month fiber customers churn at 54.6%, and fiber customers without tech support churn at 49.4% versus 22.6% with it.
How long do customers stay?
Months since signup	All customers	Month-to-month	One-year	Two-year
12	84.3%	70.3%	99.1%	~100%
24	78.9%	58.6%	97.8%	~100%
36	74.9%	49.1%	95.9%	99.9%
60	66.4%	29.7%	83.2%	98.6%
Average tenure is 18.0 months for month-to-month customers, 42.0 for one-year and 56.7 for two-year. Average billed per customer is $1,369, $3,033 and $3,707 respectively, so contract length is also a customer-value story.
The revenue impact
Churned customers represent $139K of $456K in monthly recurring revenue (30.5%). As a rough sizing: every 1 percentage point reduction in overall churn is worth about 70 customers and about $5K in monthly recurring revenue.
---
Recommendations
#	Action	Evidence
1	Move month-to-month customers to annual plans with a discount or perk offered in the first 90 days	89% of churn is month-to-month
2	Launch a first-90-days onboarding programme (welcome call, usage check-in, early support)	55% of churners leave in year one
3	Bundle tech support and online security free for a few months for fiber customers	Fiber churn 49.4% without tech support vs. 22.6% with
4	Promote auto-pay with a small bill credit	Manual payers churn at 34.7% vs. 16.0%
5	Investigate fiber pricing and service quality	Fiber churns at more than 2x DSL and pays more
6	Offer simpler plans and household bundles to seniors and customers without dependents	41.7% and 31.3% churn
7	Build a churn-risk flag (month-to-month, under 12 months, fiber, manual payment) and work the list of 273 active high-risk customers	70.2% churn in this segment
---
Limitations
Correlation, not causation. For example, customers with protection add-ons may simply be more engaged to begin with. A controlled test (A/B offer) is the way to confirm impact.
No churn-reason field or signup date. Cohorts are based on tenure, not signup month.
Snapshot data. Customers still active may churn later, so churn rates by tenure bucket describe the current customer base rather than a fixed starting cohort. The survival curve addresses this for the retention view.
Sparse tails. Retention estimates beyond about month 60 rest on fewer customers.
Lifetime estimates are rough. The expected-lifetime measure assumes a constant monthly churn rate.
---
Repository Structure
```
├── README.md
├── data/
│   ├── WA\\\_Fn-UseC\\\_-Telco-Customer-Churn.csv     # raw data
│   └── telco\\\_churn\\\_clean.csv                    # cleaned data with engineered features
├── dashboard/
│   ├── Customer\\\_Churn\\\_Dashboard.pbix            # Power BI file
│   └── churn\\\_dashboard\\\_theme.json               # custom theme
├── docs/
│   ├── dax\\\_measures.md                          # all measures and calculated tables
│   └── Churn\\\_Dashboard.pdf                      # exported dashboard pages
└── images/
    ├── 01\\\_executive\\\_overview.png
    ├── 02\\\_churn\\\_drivers.png
    ├── 03\\\_customer\\\_lifetime.png
    └── 04\\\_recommendations.png
```
---
How to Open the Dashboard
Download `dashboard/Customer\\\_Churn\\\_Dashboard.pbix`.
Open it in Power BI Desktop (free, Windows).
If prompted for the data source, go to Home → Transform data → Data source settings and point it to `data/WA\\\_Fn-UseC\\\_-Telco-Customer-Churn.csv`.
To restyle, import `dashboard/churn\\\_dashboard\\\_theme.json` via View → Themes → Browse for themes.
---
Author
[Sheba Ligeyi]
Data Analytics | Future Interns 2026
LinkedIn · GitHub
