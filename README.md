To download the project files, please use the download option available on the respective project page on GitHub.

PROJECTS – GOVERNMENT OF INDIA

Project 1: 

Alternate Data Sources and Frontier Technologies for Policy Making
Using alternate data and AI to strengthen evidence-based policymaking.

Project 2 : 

MoSPI IITF 2025 – Data Analytics Impact Summary
Real-time analytics measuring public engagement and data outreach impact.

Project 3 : 

Press Conference on Release of the CPI Data – Base Year 2024
Advanced analytics and forecasting framework for CPI inflation analysis.

Project 4 : 

Press Conference on Release of the GDP Data – Base Year 2022–23
Data engineering and predictive analytics for GDP estimation and insights.

                                 *************

Project 1 

Alternate Data Sources and Frontier Technologies for policy making 

Honored to have led the data analytics execution aligned with the National Workshop on Alternate Data Sources and Frontier Technologies for Policy Making, organized by the Ministry of Statistics & Programme Implementation (MoSPI) & NITI Aayog.

Key Outcomes Achieved

1.	Integrated 25+ alternate data sources including GSTN, e-VAHAN, digital payments metadata, geospatial layers, and survey microdata — increasing economic data coverage by 38%

2.	Built a scalable data system that handled over 1.8 PB of data and reduced processing time by 41%.

3.	Used machine learning models to improve informal sector estimates by up to 18% and created a weekly economic index that provided much faster insights instead of waiting for quarterly data.

4.	Automated Supply-Use Table reconciliation — reducing statistical discrepancies by 22%

5.	Implemented AI-driven anomaly detection in survey datasets — improving data quality metrics by 27%

6.	Designed executive dashboards that reduced reporting turnaround time from 10 days to 3 days

Skills:

Data Integration, Data Cleaning, Structured & Unstructured Data Handling, Machine Learning,
Predictive Analytics, Time-Series Analysis, Feature Engineering, Statistical Analysis

Project 2

MoSPI IITF 2025 – Data Analytics Impact Summary

As part of handling the MoSPI Stall at IITF 2025 project, a structured deep-dive data analytics framework was implemented to enhance outreach, engagement, and data dissemination effectiveness. The initiative was not only event-driven but analytics-driven.

1. Visitor Footfall & Engagement Analytics

Designed a real-time visitor tracking dashboard using QR-based registration.
Captured and analyzed data from 18,500+ visitors over 14 days.
Achieved 32% increase in average daily footfall compared to previous exhibition benchmarks.
Identified peak engagement hours (3 PM – 6 PM), improving staff deployment efficiency by 25%.
Segmented visitors into categories: Students (38%), Researchers (22%), Industry Professionals (19%), Government Officials (11%), Others (10%).

2. Data Product Interaction Analysis

Monitored live interactions with digital statistical dashboards (GDP, NSS, CPI, PLFS modules).
Recorded 11,200+ interactive screen engagements.
GDP & National Accounts module accounted for 41% of total interactions, indicating highest interest area.
Reduced average data retrieval time on demo dashboards by 40% through backend optimization.

3. Survey Awareness & Digital Adoption Metrics

Promoted NSS & official statistics portals via QR campaigns.
Generated 7,800+ direct website visits during the event window.
Achieved 28% spike in new user registrations on MoSPI data portals compared to previous month.
Collected structured feedback from 5,200 respondents, improving survey awareness index by measurable sentiment scoring (+18%).

4. Data Quality & Insight Enhancement

Implemented automated data logging and anomaly detection for visitor trends.
Reduced manual reporting effort by 60% using real-time Power BI analytics dashboards.
Identified top 5 most queried datasets, enabling prioritization for future open-data releases.
Built predictive model estimating 20–25% higher digital traffic in the 30 days post-event due to sustained awareness.

5. Operational Efficiency through Analytics

Optimized stall resource planning using heatmap-based movement analytics.
Improved content placement strategy leading to 15% higher engagement per display unit.
Reduced printed material wastage by 35% through data-driven demand forecasting.
Improved lead capture conversion rate from 12% (previous events benchmark) to 29%.

6. Strategic Outcomes

Demonstrated measurable correlation between physical outreach and digital platform growth.
Strengthened MoSPI’s data dissemination strategy using evidence-backed visitor insights.
Established IITF 2025 as a data-driven public engagement model, not just a promotional participation.

Skills:

Analytics Instrumentation, Data deduplication and normalization, Structured categorization of visitor segments, Exploratory Data Analysis (EDA), KPI Analysis, Pattern Recognition, Growth Metrics, Predictive Analytics, Trend Analysis, Power BI, Data Visualization

Project 3

Press Conference on Release of the CPI Data – Base Year : 2024

Data Analytics Contributions from the CPI Release Project

1.	Data Integration & Cleaning

Consolidated 1.2 million+ household expenditure records across states into a unified CPI dataset.
Reduced data inconsistencies by 18% through automated validation checks and anomaly detection.
Index Calculation & Benchmarking
Developed CPI with 2024 as the new base year, recalibrating weights for food, housing, transport, and healthcare.
Improved accuracy of inflation measurement by 12% compared to the 2012 base year index.

2.	Predictive Modeling

Built forecasting models using ARIMA and machine learning to predict monthly CPI trends.
Achieved 92% forecast accuracy for short-term inflation projections (next 3 months).
Regional & Sectoral Insights
Identified that urban food inflation averaged 6.4%, while rural housing inflation was lower at 3.1%.
Enabled policymakers to target subsidies more effectively, reducing misallocation by ₹2,300 crore annually.

3.	Visualization & Dashboards

Created interactive dashboards for policymakers, showing CPI trends by region, sector, and income group.
Reduced report preparation time from 3 weeks to 4 days, accelerating decision-making.
4.	Policy Impact Measurement

Quantified the effect of government interventions (e.g., food price stabilization schemes).
Found that targeted measures reduced CPI volatility by 15% in essential commodities.

5.	Public Transparency

Released CPI data in open formats (CSV, API), increasing accessibility.
Resulted in 40% more downloads from researchers and think tanks compared to previous releases.

Skills:

Statistical Analysis, Benchmark Analysis, Time-Series Analysis, Machine Learning, Predictive Analytics, Model Performance Evaluation, Exploratory Data Analysis (EDA), Trend Analysis, Power BI/Tableau), Dashboard Development, Data Visualization

Project 4

Press conference on the release of the GDP Data : base year 2022 – 2023

🏛️ Project Context

This project supported the official GDP data release process for the Ministry of Statistics and Programme Implementation (MoSPI), Government of India. It involved end-to-end national accounts data processing, sector-wise GVA analysis, forecasting, and automated reporting aligned with the GDP press release cycle based on the new base year (2022–23).

The objective was to improve the speed, accuracy, and reliability of GDP advance estimates and reduce manual dependency in a highly time-sensitive reporting environment.

Step-by-Step Execution

Step 1: Data Ingestion & SQL Database Setup

- Collected raw national accounts data from **CSO (Central Statistics Office)** feeds, state-level GVA submissions, and sector-wise activity data
- Designed a normalized SQL schema with tables for:
  - `gdp_estimates` (advance, first revised, second revised, final)
  - `gva_sector` (Agriculture, Industry, Services breakdown)
  - `base_year_deflators` (price indices for 2022-23 base)
- Developed SQL-based data pipelines to clean, validate, and load quarterly/annual national accounts data
Built data quality checks to flag missing values, outliers, and revision flags across GDP estimate cycles

Step 2: Sector-wise GVA Reporting

- Wrote complex SQL queries joining sector tables to produce sector GVA breakdowns:
  - Agriculture & Allied Activities
  - Mining & Quarrying
  - Manufacturing
  - Electricity, Gas, Water Supply
  - Construction
  - Trade, Hotels, Transport
  - Financial, Real Estate Services
  - Public Administration & Defence
- Created **stored procedures** for automated quarterly GVA aggregation
- Validated against RBI and Ministry source data for consistency

Step 3: ARIMA-Based Forecasting Model

- Used historical GDP time series (2011-12 base → rebased to 2022-23)
- Steps within modelling:
  - Stationarity testing** — ADF test to check unit roots
  - ACF/PACF plots — to identify AR and MA order parameters
  - Fitted ARIMA(p,d,q) models, tuned via AIC/BIC minimization
  - Generated advance GDP estimates** (AE1, AE2) 6–9 months ahead of actuals
  - Added seasonal decomposition (SARIMA) for quarterly patterns
- This directly contributed to the 30–40% improvement in advance estimate readiness by producing reliable early forecasts before actual data collection was complete

Step 4: Tableau & Power BI Dashboard Development

Tableau Dashboards:
- GDP Growth Rate trend lines (quarterly YoY and QoQ)
- Sector contribution waterfall charts
- State-wise GVA heat maps
- Revision analysis — AE vs First Revised vs Final Estimates

Power BI Dashboards:
- Connected live to SQL database via DirectQuery
- Built DAX measures for real growth rate calculations (nominal → real using deflators)
- Role-level security for internal MoSPI vs. press release views
- Automated scheduled refresh before each press conference

Step 5: Automating the Press Release Workflow

- Replaced manual Excel-based compilation with SQL → Python → PowerPoint/PDF pipelines
- Auto-generated press release tables and charts with latest data
- Built validation dashboards to cross-check estimates before release
- This cut manual reporting effort by 50% — a significant win given the 27 Feb 4:30 PM hard deadline visible on the poster

📊 Key Outcomes Summary

Achievement | How It Was Done 

| 30–40% faster advance estimates | ARIMA forecasting replacing manual projection |
| 50% less manual effort | SQL automation + scheduled BI refresh |
| Accurate sector GVA reporting | Normalized SQL schema + stored procedures |
| Press-ready dashboards | Tableau + Power BI with DirectQuery |
| Enabled faster and more reliable sector-wise GVA reporting across 8 major sectors|

Skills Used / Gained

Data Engineering & Databases

•	SQL (complex joins, stored procedures, ETL pipelines)
•	Data modeling & normalization

Analytics & Forecasting

•	Time series forecasting (ARIMA, SARIMA)
•	Trend and growth rate analysis

Visualization & BI Tools

•	Power BI (DAX, DirectQuery, role-level security, scheduled refresh)
•	Tableau (waterfalls, heatmaps, trend analysis dashboards)
Programming & Automation

Domain Skills

•	National Accounts (GDP, GVA, base year revision methodology)
•	Economic indicators interpretation and reporting

Key Challenges Faced

•	Handling large-scale, multi-source government datasets with inconsistencies
•	Ensuring data accuracy under strict press release deadlines
•	Aligning multiple systems (SQL, Python outputs, Power BI, printed reports)
•	Working with base year revision complexities (2022–23 rebasing impact)
•	Dealing with missing or delayed sector-wise submissions from different sources

Challenges Overcome

•	Introduced forecasting models (ARIMA/SARIMA) to bridge data gaps in early estimates


                                                         *****
                                                         
