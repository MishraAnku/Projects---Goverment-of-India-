# Ministry of Statistics and Programme Implementation, Government of India - Key Projects

# PROJECT 1 : Alternate Data Sources and Frontier Technologies for Policy Making

# Aim of the project

The aim of the project is to design an integrated policy analytics system that brings together multiple economic data sources such as taxation, payments, vehicle registrations, and household surveys to generate meaningful indicators that help understand real time economic activity and support evidence based decision making

# Purpose of the project

The purpose of the project is to build a unified analytical pipeline that can convert raw and fragmented government and alternate data sources into actionable insights for policymakers. 

It is designed to monitor economic activity, estimate informal sector behavior, detect anomalies, and forecast future trends so that timely interventions can be planned

# Challenges faced by the project

One major challenge was dealing with heterogeneous data sources coming from different systems with inconsistent formats and missing values. 

Another challenge was integrating structured transactional data with survey based data. Handling data quality issues such as duplicates, missing entries, and noisy observations was also critical.

In addition, building reliable indicators from proxy variables for informal sector estimation was conceptually challenging

# How the challenges were overcome

These challenges were addressed through a structured data engineering pipeline.

Data standardization techniques were used to unify column formats and naming conventions. Missing values were handled using statistical imputation methods such as median and mode. 

A systematic merge strategy using district as a common key helped integrate datasets. Machine learning techniques like Isolation Forest and Random Forest were used to handle noise, detect anomalies, and model complex relationships. 

Normalization ensured comparability across different indicators

# Skills and tools covered in this project and why these skills and tools were used

The project used SQL for efficient data extraction and aggregation because it allows scalable and structured querying of large datasets. 

Python was used for end to end pipeline development due to its flexibility and strong data ecosystem. 

Pandas was used for data manipulation because it simplifies tabular processing. Scikit learn was used for machine learning models like Isolation Forest and Random Forest because it provides robust and tested algorithms. 

Statsmodels was used for time series forecasting because it supports statistical models like exponential smoothing. 

Power BI or Tableau was used for visualization because it enables interactive dashboards for decision making. 

These tools were chosen because they collectively support the full lifecycle from data engineering to analytics and visualization

# Limitations of the project

The project relies heavily on the quality and completeness of input data, so biased or missing data can impact results. The informal sector estimation is based on proxy variables, so it may not fully capture real ground truth behavior. 

The forecasting model assumes stable trends and may not perform well during sudden economic shocks. Additionally, district level aggregation may hide micro level variations within regions

# Analytics performed in this project

The project performs multiple layers of analytics including descriptive analytics through aggregated summaries of sales, transactions, and registrations. 

Diagnostic analytics through anomaly detection to identify unusual patterns. 

Predictive analytics using Random Forest models for informal sector estimation and Exponential Smoothing for forecasting. 

It also performs composite index creation through weighted normalization to generate a weekly economic activity indicator. 

Correlation based feature engineering is implicitly used to derive meaningful economic relationships

# Key outcomes of the project

The project produces a unified economic intelligence system that generates a weekly economic index, estimates informal sector activity, identifies anomalies in economic behavior, and forecasts future economic trends. 

It also creates a dashboard ready dataset that enables real time monitoring of district level economic performance. 

The final outcome is a decision support system that helps policymakers track economic health and detect early warning signals

# Explanation of the project in paragraph form

This project develops an end to end policy analytics pipeline that integrates multiple data sources including GST transactions, digital payments, vehicle registrations, and household surveys to build a comprehensive view of economic activity. 

The system processes raw data through cleaning, integration, and feature engineering stages to generate meaningful indicators such as digital payment ratio, business density, and vehicle growth. 

Machine learning models are then applied to detect anomalies and estimate informal sector activity, while statistical techniques are used to construct a weekly economic index. 

Finally, time series forecasting is applied to predict future economic trends and a dashboard dataset is generated for visualization tools. 

The entire system is designed to support evidence based policymaking by transforming fragmented data into actionable economic intelligence

# Conclusion of the project and where it can be used

In conclusion, the project successfully demonstrates how alternate data sources and advanced analytics can be combined to build a powerful economic monitoring system.

It provides a scalable framework for understanding both formal and informal economic activity and enables predictive insights for future planning. 

This project can be used in government policy departments for economic monitoring, in statistical organizations for real time indicator tracking, in financial institutions for regional risk assessment, and in research institutions for studying economic behavior and development patterns

---------------------------------------------------------------------------------------------------------------------

# Drill Questions

# 1. PROJECT OVERVIEW QUESTIONS

### Q1. What was the objective of this project?

The objective was to build an integrated policy analytics system that combines alternate economic datasets such as GST transactions, digital payments, vehicle registrations, and household surveys to generate real-time economic indicators that support evidence-based policymaking.

---

### Q2. Why did you choose alternate data sources instead of traditional survey data?

Traditional surveys have:

* Low frequency
* High cost
* Publication lag

Alternate datasets provide:

* High-frequency insights
* Near real-time monitoring
* Better granularity at district levels.

---

### Q3. What policy problems does this solve?

It helps policymakers:

* Monitor economic activity weekly.
* Estimate informal sector behavior.
* Detect economic anomalies.
* Identify regional disparities.
* Support targeted interventions.

---

# 2. SQL DATA EXTRACTION QUESTIONS

---

## GSTN View

```sql
SUM(taxable_value)
```

### Q4. Why did you use SUM(taxable_value)?

**Why Used:**

To aggregate business transaction values.

**What did you get?**

District-wise economic activity.

**Alternative?**

AVG() would show average transaction size but not total economic activity.

---

```sql
COUNT(DISTINCT gstin)
```

### Q5. Why DISTINCT GSTIN?

**Why Used:**

To avoid duplicate businesses.

**What did you get?**

Number of active businesses.

**Without DISTINCT?**

Same GSTIN appearing multiple times inflates counts.

---

```sql
GROUP BY district,state,transaction_date
```

### Q6. Why GROUP BY?

**Why Used:**

To aggregate daily regional metrics.

**What did you get?**

District-day level indicators.

---

### Q7. Why create SQL Views instead of tables?

Views:

* Avoid data duplication.
* Always reflect latest source data.
* Simplify analytical queries.

---

# 3. DATA LOADING QUESTIONS

---

```python
create_engine()
```

### Q8. Why SQLAlchemy Engine?

**Why Used:**

Provides database abstraction.

**Benefits:**

* Supports multiple DBs.
* Connection pooling.
* Secure interaction.

---

### Q9. Why PostgreSQL?

Because PostgreSQL offers:

* Strong analytical capabilities.
* ACID compliance.
* Excellent support for large datasets.

---

```python
pd.read_sql()
```

### Q10. Why use read_sql()?

**Why Used:**

Directly loads SQL results into DataFrames.

**What obtained?**

Analysis-ready datasets.

---

```python
pd.read_csv()
```

### Q11. Why survey data from CSV?

Survey datasets are often shared externally in flat-file formats.

---

# 4. DATA CLEANING QUESTIONS

---

```python
.str.lower()
```

### Q12. Why convert column names to lowercase?

Avoid case-sensitive errors during merging.

---

```python
.str.strip()
```

### Q13. Why remove whitespaces?

Extra spaces cause merge failures.

---

```python
drop_duplicates()
```

### Q14. Why remove duplicates?

Duplicates bias:

* Economic indicators
* Model training
* Forecasting outputs

---

```python
fillna(median)
```

### Q15. Why median instead of mean?

Median is robust against outliers.

Example:

Income:

20,000

25,000

30,000

5,00,000

Mean = distorted.

Median = stable.

---

### Q16. Why mode for categorical variables?

Mode preserves the most frequent category.

---

### Q17. What are limitations of median imputation?

It ignores relationships among variables.

Advanced alternatives:

* KNN Imputation
* MICE
* Regression Imputation

---

# 5. DATA INTEGRATION QUESTIONS

---

```python
merge(on='district')
```

### Q18. Why district as joining key?

District is the common geographical identifier across all datasets.

---

```python
how='left'
```

### Q19. Why left join?

Preserves all GSTN records.

Since GST data is considered the primary economic indicator.

---

### Q20. What if inner join was used?

Many districts could be lost if matching records don't exist.

---

### Q21. Challenges during integration?

* Different granularities.
* Missing districts.
* Naming inconsistencies.
* Different update frequencies.

---

# 6. FEATURE ENGINEERING QUESTIONS

---

## Digital Payment Ratio

```python
total_payment_value / total_sales
```

### Q22. Why create Digital Payment Ratio?

Measures digitization of economic activity.

---

### Q23. Why is it useful?

Higher ratio indicates:

* Formalization.
* Digital adoption.

---

## Vehicle Growth

```python
pct_change()
```

### Q24. Why percentage change?

Absolute growth doesn't capture relative changes.

Percentage growth standardizes comparison.

---

### Q25. What does vehicle growth indicate?

Proxy for:

* Consumer confidence.
* Mobility demand.
* Economic expansion.

---

## Business Density

```python
active_businesses / population
```

### Q26. Why business density?

Measures economic concentration.

---

### Q27. What insights were obtained?

District entrepreneurial activity.

---

## Informal Activity Proxy

### Q28. Why assign weights 0.4, 0.3, 0.3?

Domain-driven weighting:

* Digital payments = strongest signal.
* Vehicle growth = medium importance.
* Business density = supportive factor.

---

### Q29. How were weights validated?

Can be validated through:

* Expert consultation.
* PCA.
* Regression coefficients.

---

# 7. ANOMALY DETECTION QUESTIONS

---

```python
IsolationForest()
```

### Q30. Why Isolation Forest?

Because:

* Unsupervised.
* Efficient for large datasets.
* No labeled anomalies required.

---

### Q31. How does Isolation Forest work?

Anomalies are isolated faster in random trees.

Shorter path lengths indicate anomalies.

---

```python
contamination=0.02
```

### Q32. Why 2% contamination?

Domain assumption:

Approximately 2% of observations are abnormal.

---

### Q33. What happens if contamination is 10%?

Model flags too many normal observations as anomalies.

---

### Q34. What alternatives exist?

* DBSCAN
* Local Outlier Factor
* One-Class SVM
* Autoencoders

---

# 8. RANDOM FOREST QUESTIONS

---

```python
RandomForestRegressor()
```

### Q35. Why Random Forest?

Because it:

* Captures nonlinear relationships.
* Handles multicollinearity.
* Reduces overfitting.

---

```python
n_estimators=100
```

### Q36. Why 100 trees?

Provides balance between:

* Accuracy
* Computation cost

---

### Q37. What if trees increase to 1000?

Accuracy may improve slightly.

Training time increases substantially.

---

```python
train_test_split(test_size=0.2)
```

### Q38. Why 80–20 split?

Industry standard:

* Enough training data.
* Reliable evaluation.

---

### Q39. How did you evaluate the model?

Using:

* RMSE
* MAE
* R² Score

---

### Q40. What if R² was low?

Would:

* Add features.
* Tune hyperparameters.
* Try XGBoost.

---

# 9. ECONOMIC INDEX QUESTIONS

---

```python
MinMaxScaler()
```

### Q41. Why MinMaxScaler?

Because indicators have different scales.

Scaling ensures comparability.

---

### Q42. Why not StandardScaler?

MinMax preserves 0–1 interpretation.

Suitable for composite indices.

---

### Q43. What is the formula?

(X − Min)/(Max − Min)

---

### Q44. Why weights 0.4, 0.35, 0.25?

Reflect relative importance:

* Sales strongest indicator.
* Transactions moderate.
* Registrations supportive.

---

### Q45. How could weights be optimized?

Using:

* PCA.
* Factor Analysis.
* Entropy Weighting.

---

# 10. FORECASTING QUESTIONS

---

```python
ExponentialSmoothing()
```

### Q46. Why Exponential Smoothing?

Captures trends while emphasizing recent observations.

---

```python
trend='add'
```

### Q47. Why additive trend?

Assumes constant absolute change over time.

---

### Q48. Why seasonal=None?

No clear seasonal pattern observed.

---

### Q49. Why forecast 12 weeks?

Balances short-term policy planning horizon.

---

### Q50. Alternatives?

* ARIMA
* SARIMA
* Prophet
* LSTM

---

# 11. DASHBOARD QUESTIONS

---

### Q51. Why Power BI/Tableau?

Interactive decision-making dashboards.

---

### Q52. Why KPI Cards?

Provide executive summaries.

---

### Q53. Why Heat Maps?

Highlight concentration of informal activity.

---

### Q54. Why Choropleth Maps?

Visualize regional disparities.

---

### Q55. Why anomaly dashboard?

Enables rapid investigation.

---

# 12. PIPELINE QUESTIONS

---

### Q56. Why modular architecture?

Improves:

* Maintainability.
* Reusability.
* Scalability.

---

### Q57. Why separate scripts?

Single Responsibility Principle.

---

### Q58. How was workflow automated?

The workflow was automated by developing an **end-to-end ETL and analytics pipeline in Python**, where each stage of the process was organized into independent modules and executed sequentially through a central **`main.py`** script. This eliminated manual intervention and ensured that the entire process—from data extraction to dashboard generation—ran consistently and efficiently.

### Q59. Biggest challenge in this project?

Integrating heterogeneous datasets with different structures and update frequencies.

---

### Q60. How did you overcome it?

By:

* Standardizing identifiers.
* Cleaning inconsistencies.
* Building reusable preprocessing modules.

---

### Q61. Why Random Forest instead of XGBoost?

I chose Random Forest because it provides strong predictive performance with minimal hyperparameter tuning and is easier to interpret.

Since the project focused on generating reliable and explainable policy insights rather than maximizing predictive accuracy, Random Forest offered a good balance between accuracy, robustness, and simplicity. 

XGBoost generally provides higher accuracy but requires extensive tuning and is computationally more complex.

### Q62. How would you detect data drift?

I would detect data drift by comparing the distribution of incoming production data with the training data using techniques such as Population Stability Index (PSI), Kolmogorov–Smirnov tests, and monitoring model performance metrics like RMSE and R². Significant deviations would indicate drift and trigger model review or retraining.

### Q63. How would you operationalize this in production?

I would operationalize the solution by implementing integrating dashboards with Power BI or Tableau for real-time reporting.

### Q64. How would you retrain models?

I would retrain models either on a scheduled basis, such as monthly or quarterly, or based on performance degradation and data drift indicators. 

The retraining process would involve collecting new data, validating data quality, regenerating features, retraining the model, evaluating performance against the existing model, and deploying only if improvements are observed.

### Q65. How would you validate economic index robustness?

I would validate the economic index using sensitivity analysis to assess the impact of different weights, backtesting against historical economic events, comparing it with established indicators like GDP growth, and obtaining expert validation from domain specialists to ensure reliability and consistency.

### Q66. How would you handle missing districts?

I would first investigate the cause of missing district data. Depending on the extent of missingness, I would use historical values, state-level averages, or neighboring district information for imputation while clearly flagging imputed records to maintain transparency and data quality.

### Q67. How would you improve anomaly detection accuracy?

I would improve anomaly detection accuracy by engineering additional features, tuning Isolation Forest parameters, incorporating domain knowledge, and combining multiple anomaly detection techniques such as Local Outlier Factor, One-Class SVM, or Autoencoders to reduce false positives and improve detection capability.

### Q68. What assumptions does Exponential Smoothing make?

Exponential Smoothing assumes that recent observations are more relevant than older observations, that underlying trends continue into the future, and that the structure of the time series remains relatively stable without major structural breaks or sudden changes.

### Q69. If policymakers disagree with the index weights, what would you do?

I would conduct sensitivity analysis to demonstrate how different weights affect the index outcomes, explore data-driven weighting approaches such as Principal Component Analysis (PCA) or Factor Analysis, and collaborate with policymakers and domain experts to establish weights that are both statistically sound and policy-relevant.

-------------------------------------------------------------------------------------------------------------------------------

# PROJECT 2:  MoSPI IITF 2025 – Data Analytics Impact Summary

# Aim of the Project

The aim of this project was to design and implement an integrated data analytics framework for MoSPI IITF 2025 that captures, analyzes, and interprets visitor engagement across physical and digital platforms. 

The project aimed to transform raw visitor registrations, dashboard interactions, feedback responses, and portal activities into actionable insights that could measure public outreach effectiveness, improve citizen engagement strategies, and support data-driven decision making for future government exhibitions and awareness initiatives.

# Purpose of the Project

The purpose of this project was to evaluate the impact of MoSPI's participation in IITF 2025 by tracking the complete visitor journey from stall visits to digital engagement and registrations. 

The project sought to provide measurable evidence of outreach success through key performance indicators such as footfall growth, engagement levels, awareness improvement, registration conversions, and digital adoption. 

It also aimed to establish a scalable analytics framework that could be reused for future government events and public engagement programs.

### Challenges Faced in the Project

One of the major challenges was integrating multiple heterogeneous data sources such as QR visitor registrations, dashboard interaction logs, feedback surveys, and portal registration datasets.

Since these datasets originated from different systems, they had varying formats, structures, and levels of completeness.

Another challenge involved maintaining data quality. Duplicate visitor scans, missing interaction records, inconsistent timestamps, and invalid demographic information affected the reliability of analysis.

Measuring the true impact of awareness campaigns was also difficult because awareness is an abstract concept that cannot be directly observed. Quantifying improvements required deriving sentiment scores from visitor feedback.

Tracking the complete visitor lifecycle from physical stall visits to digital platform adoption posed another challenge because users interacted across multiple channels over time.

Forecasting post-event digital engagement was challenging due to the short event duration and limited historical observations available for predictive modeling.

Finally, generating near real-time dashboards with acceptable response times required optimization because large interaction datasets could negatively impact reporting performance.

# How the Challenges Were Overcome

The multi-source integration challenge was addressed by establishing visitor identifiers and time-based relationships between datasets, enabling the construction of an end-to-end visitor journey.

Data quality issues were mitigated through robust preprocessing techniques including duplicate removal, timestamp standardization, missing value handling, and validation checks for demographic attributes.

Awareness measurement challenges were overcome by applying sentiment analysis techniques to visitor feedback responses, allowing qualitative perceptions to be converted into quantitative indicators.

Visitor lifecycle tracking was enabled through funnel analytics that connected physical attendance, digital interactions, registrations, and feedback submissions into a unified analytical framework.

Forecasting limitations were addressed using trend-based machine learning techniques such as Linear Regression to estimate future engagement patterns despite limited data availability.

Dashboard performance challenges were resolved through SQL aggregation views, optimized queries, pre-computed KPIs, and Power BI dataset exports, significantly reducing retrieval times.

# skills and Tools Covered in This Project and Why They Were Used

This project involved strong data engineering, analytics, visualization, and predictive modeling skills.

Python was used because it provides powerful libraries for data processing, machine learning, and automation. Its flexibility allowed the development of a modular and scalable analytics pipeline.

Pandas was used for data cleaning, transformation, aggregation, and feature engineering because it efficiently handles structured tabular datasets.

NumPy was used for numerical computations and creating arrays required during forecasting and analytical calculations.

Scikit-learn was selected for predictive analytics because it provides reliable machine learning algorithms such as Linear Regression with simple implementation and strong performance for trend forecasting.

SQL was used to create reusable analytical views and perform efficient aggregation operations directly at the database layer, improving reporting performance.

Power BI was used for interactive visualization because it enables stakeholders to explore KPIs, trends, and insights through intuitive dashboards without requiring technical expertise.

Excel and CSV exports were utilized to facilitate seamless integration between analytical outputs and business reporting tools.

The selected tools were appropriate because they are industry-standard technologies that support the entire analytics lifecycle, from data ingestion to decision-making dashboards, while ensuring scalability, maintainability, and ease of deployment.

# Limitations of the Project

The forecasting model relied on a relatively short event duration, limiting the ability to capture long-term seasonal patterns and complex behavioral changes.

Linear Regression assumes a linear trend in digital engagement, which may not fully represent real-world fluctuations influenced by external factors.

Sentiment analysis outcomes depend heavily on the quality and volume of feedback collected and may not completely represent all visitor perceptions.

The project primarily focused on structured data sources and did not extensively utilize unstructured multimedia data such as images or video analytics.

Cross-device and anonymous user tracking limitations may affect the completeness of visitor journey mapping.

The findings are highly relevant to IITF 2025 but may require recalibration when applied to different events with varying audience demographics and engagement patterns.

# Analytics Performed in This Project

The project involved descriptive analytics to understand visitor demographics, footfall trends, interaction volumes, and module popularity.

Diagnostic analytics was performed to identify factors contributing to higher engagement rates, improved registrations, and reduced retrieval times.

Segmentation analytics categorized visitors into groups such as students, researchers, government officials, and industry professionals to understand audience behavior.

Time-series analytics identified peak visiting hours and daily engagement patterns.

Sentiment analytics measured changes in awareness levels by evaluating visitor feedback responses.

Funnel analytics tracked the progression from physical attendance to digital interactions and eventual registrations.

Predictive analytics forecasted future digital engagement trends following the event.

Efficiency analytics assessed improvements in resource utilization, including reductions in printed material wastage and enhancements in engagement per display unit.

# Key Outcomes of the Project

The project demonstrated a substantial increase in exhibition footfall, indicating improved public participation and outreach effectiveness.

Dashboard optimization reduced data retrieval times, enhancing user experience and enabling faster access to insights.

Digital registrations increased significantly, reflecting successful conversion from physical engagement to online participation.

Sentiment analysis revealed improvements in public awareness regarding MoSPI datasets and statistical initiatives.

Higher engagement efficiency per display unit validated the effectiveness of stall design and content placement strategies.

The adoption of QR-based content distribution contributed to a notable reduction in printed material wastage, supporting sustainability objectives.

The integrated analytics framework enabled comprehensive visitor journey analysis and established a foundation for predictive and prescriptive insights.

# Explanation of the Project

The MoSPI IITF 2025 Data Analytics Impact Summary project was designed to evaluate the effectiveness of the Ministry of Statistics and Programme Implementation's participation in the India International Trade Fair. 

The project integrated multiple data sources including QR-based visitor registrations, dashboard interaction logs, visitor feedback surveys, portal visits, and registration datasets into a unified analytics framework. 

Through data cleaning, feature engineering, KPI computation, predictive forecasting, and dashboard visualization, the project measured critical dimensions of event success such as visitor growth, digital engagement, awareness improvement, registration conversion, and operational efficiency. 

The implementation leveraged Python for analytics, SQL for data aggregation, machine learning for forecasting, and Power BI for reporting.

By connecting physical and digital touchpoints, the project provided evidence-based insights that supported strategic decision making and demonstrated the real-world impact of government outreach initiatives.

# Conclusion and Where It Can Be Used

This project successfully established a comprehensive analytics ecosystem capable of measuring the effectiveness of large-scale public engagement initiatives. 

By integrating multiple data sources and applying advanced analytical techniques, it transformed raw event data into meaningful performance indicators that supported evidence-based decision making. 

The project demonstrated how data analytics can enhance visitor experiences, optimize resource allocation, improve awareness campaigns, and strengthen digital transformation efforts within government organizations.

The framework developed in this project can be used in government exhibitions, public awareness campaigns, trade fairs, citizen engagement programs, digital transformation initiatives, smart city events, educational expos, healthcare awareness programs, and any large-scale event requiring end-to-end visitor analytics.

It can also be extended to support real-time monitoring, predictive planning, and strategic policy formulation in future public sector initiatives.

# Drill Questions

**Why was this project needed?**

Traditional event evaluations focus only on visitor counts, which provide limited insights. This project was needed to understand how visitors interacted with MoSPI's statistical products, measure awareness generation, assess digital adoption, and provide evidence-based insights for improving future outreach strategies.

**What business problem were you solving?**

The project addressed the challenge of quantifying the effectiveness of government awareness campaigns and exhibitions. It aimed to answer whether increased footfall translated into meaningful engagement, awareness, and digital conversions.

**What was your role in this project?**

I was responsible for designing the end-to-end analytics pipeline including data ingestion, preprocessing, feature engineering, KPI development, predictive modeling, SQL optimization, and dashboard reporting.

## Data Engineering and Pipeline Questions

**Why did you create a modular project structure?**

A modular architecture improves maintainability, scalability, and reusability. Separating ingestion, cleaning, feature engineering, analytics, forecasting, and export functionalities allows independent testing and easier modifications.

**Why was config.py used?**

The configuration file centralizes all environment-specific variables such as file paths and constants. This improves flexibility because changes can be made in one location without modifying the entire codebase.

**Why did you separate raw and processed data folders?**

Raw data preserves original information for auditing and reproducibility. Processed data contains cleaned datasets ready for analysis. This separation supports data governance and traceability.

**How would you handle millions of records instead of CSV files?**

Since the project used CSV files, I would handle millions of records by reading data in chunks using Pandas, optimizing memory usage through efficient data types

## Data Cleaning Questions

**Why did you convert timestamps into datetime format?**

Datetime conversion enables time-based operations such as sorting, filtering, extracting hours, trend analysis, and forecasting.

**Why did you remove duplicate visitor records?**

Duplicate scans may occur due to repeated QR scans by the same visitor. Removing duplicates prevents inflation of footfall metrics and ensures accurate KPI calculations.

**Why did you keep the first occurrence of visitor records?**

The first occurrence represents the earliest valid entry and avoids overcounting the same visitor multiple times.

**What limitations exist in this duplicate removal approach?**

If a visitor attends on multiple days, this approach may incorrectly remove valid visits. A better approach would involve using time-window-based deduplication.

**Why remove records with age less than or equal to zero?**

These represent invalid data entries and can distort demographic analyses and segmentation outcomes.

**How would you handle missing values instead of dropping them?**

I would evaluate the missingness pattern and apply suitable techniques such as mean imputation, median imputation, mode replacement, predictive imputation, or flagging missing categories.

**How would you identify outliers in visitor data?**

Using statistical methods such as Z-score, Interquartile Range, Isolation Forest, or domain-based thresholds.

## Feature Engineering Questions

**Why was visitor segmentation important?**

Segmentation enables understanding of how different audience groups engage with MoSPI services, allowing targeted outreach strategies.

**Why segment based on occupation?**

Occupation directly reflects the stakeholder category and provides meaningful insights into user interests and engagement patterns.

**How would you improve visitor segmentation?**

I would use clustering algorithms such as K-Means or Hierarchical Clustering using demographic and behavioral variables.

**Why analyze peak hours?**

Peak hour analysis helps optimize staffing, resource allocation, kiosk placement, and visitor management strategies.

**How would you validate that the segments are meaningful?**

I would compare engagement metrics across segments and assess whether significant behavioral differences exist.

## KPI and Analytics Questions

**Why were KPIs important in this project?**

KPIs translate raw data into measurable indicators that reflect event performance, engagement effectiveness, and strategic outcomes.

**How did you calculate footfall growth?**

Footfall growth was calculated as:

(Current Visitors − Previous Visitors) ÷ Previous Visitors × 100

**Why is footfall alone insufficient?**

High visitor numbers do not necessarily indicate meaningful engagement or awareness. Additional KPIs such as conversions and sentiment are needed.

**Why measure module popularity?**

It identifies datasets generating the highest interest, helping prioritize future content and resource allocation.

**How was engagement efficiency measured?**

By calculating interactions per display unit, enabling evaluation of kiosk effectiveness.

**How was lead conversion measured?**

Lead conversion was measured as:

Converted Users ÷ Total Visitors × 100

This indicates how effectively physical visitors transitioned to digital users.

**Why is awareness improvement important?**

Government initiatives aim not only to attract visitors but also to increase public understanding and awareness of official statistics.

## SQL Questions

**Why create SQL views instead of writing queries repeatedly?**

Views simplify complex queries, improve reusability, and provide a consistent layer for reporting tools.

**What advantages do SQL views provide?**

They improve maintainability, reduce query duplication, abstract complexity, and enhance security.

**How can query performance be improved?**

Using indexes, partitioning, materialized views, query optimization, and pre-aggregation strategies.

**Difference between a View and Materialized View?**

A View stores only the query definition and computes results dynamically.

A Materialized View stores precomputed results physically and improves query performance.

**Why use GROUP BY in these queries?**

GROUP BY enables aggregation calculations such as counts and averages across categories.

## Machine Learning and Forecasting Questions

**Why did you use Linear Regression for forecasting?**

The project required a simple, interpretable model to capture short-term trends in digital engagement.

**What assumptions does Linear Regression make?**

It assumes linear relationships, independence of observations, constant variance of errors, normal error distribution, and absence of multicollinearity.

**What are the limitations of Linear Regression here?**

It cannot capture seasonality, non-linear trends, or abrupt behavioral changes.

**What models could improve forecasting accuracy?**

ARIMA, SARIMA, Prophet, Random Forest Regressor, XGBoost, or LSTM models.

**Why predict future engagement?**

Forecasting helps anticipate resource requirements, server loads, and future outreach effectiveness.

**How would you evaluate forecasting performance?**

Using metrics such as RMSE, MAE, MAPE, and R-squared.

**What would you do if predictions become inaccurate?**

I would retrain models with new data, engineer additional features, and evaluate alternative algorithms.

## Sentiment Analysis Questions

**Why perform sentiment analysis?**

Sentiment analysis quantifies visitor perceptions and awareness improvements using feedback data.

**Why use TextBlob or VADER?**

They are lightweight NLP tools designed for polarity scoring and sentiment classification.

**What are limitations of sentiment analysis?**

Difficulty detecting sarcasm, contextual nuances, domain-specific language, and mixed sentiments.

**How would you improve sentiment analysis?**

Using transformer-based models such as BERT or fine-tuned domain-specific language models.

## Power BI Questions

**Why use Power BI?**

Power BI provides interactive dashboards that enable stakeholders to monitor KPIs and trends without technical expertise.

**What dashboards did you create?**

Footfall analysis dashboards, visitor segmentation reports, engagement trends, conversion funnels, sentiment summaries, and forecasting dashboards.

**Why export data to CSV for Power BI?**

CSV files provide a simple and widely compatible format for seamless dashboard integration.

**How would you enable real-time dashboards?**

Using DirectQuery mode, streaming datasets, and automated refresh schedules.

## Integration and Architecture Questions

**How did you integrate multiple datasets?**

Visitor IDs acted as common keys, while timestamps enabled temporal relationships between interactions and feedback.

**What challenges arise in multi-source integration?**

Data inconsistencies, missing identifiers, schema mismatches, and synchronization issues.

**How would you ensure data integrity during integration?**

Through validation checks, referential integrity constraints, reconciliation reports, and automated quality monitoring.

**Why is end-to-end visitor journey analysis valuable?**

It enables understanding of how physical interactions influence digital engagement and awareness outcomes.

## Scenario-Based Questions

**What if visitor IDs are missing?**

I would use probabilistic matching techniques based on timestamps, demographics, and interaction patterns.

**What if interaction logs double overnight?**

I would investigate system issues, validate duplicate events, and compare against historical baselines.

**What if feedback responses are highly imbalanced?**

I would apply stratified sampling or weighted sentiment calculations.

**What if dashboard performance degrades?**

I would optimize SQL queries, use materialized views, cache datasets, and reduce expensive joins.

**What if registrations increase but sentiment decreases?**

This indicates successful acquisition but poor user experience. Further investigation into usability issues would be necessary.

## Advanced Drill Questions

**How would you redesign this project for production deployment?**

I would implement cloud-based ETL pipelines, automated validation frameworks, scalable data warehouses, CI/CD deployment, monitoring dashboards, and model retraining pipelines.

**How would you detect anomalies in visitor behavior?**

Using Isolation Forest, One-Class SVM, Z-score analysis, or time-series anomaly detection techniques.

**How would you measure the success of this project?**

By evaluating improvements in footfall, engagement, awareness, registrations, operational efficiency, and stakeholder satisfaction.

**If given additional time, what enhancements would you implement?**

I would add real-time analytics, advanced NLP models, recommendation systems, predictive conversion models, and geospatial visitor analysis.

**What is the biggest learning from this project?**

The biggest learning was that meaningful event impact assessment requires integrating physical engagement metrics with digital behavior and sentiment analysis rather than relying solely on footfall statistics.

------------------------------------------------------------------------------------------------------------------------------

# SQL Questions

# Write a query to calculate peak engagement hours.

```sql
SELECT 
    HOUR(visit_time) AS hour_slot,
    COUNT(*) AS total_visitors
FROM visitor_logs
GROUP BY HOUR(visit_time)
ORDER BY total_visitors DESC;
```

# How would you identify duplicate visitors using SQL?

```sql
SELECT mobile_number, COUNT(*)
FROM visitor_data
GROUP BY mobile_number
HAVING COUNT(*) > 1;
```

------------------------------------------------------------------------------------------------------------------------------

# Project 3 : Press Conference on Release of the CPI Data – Base Year : 2024

# Aim of the Project

The aim of this project is to develop an end to end Consumer Price Index forecasting framework using statistical methods, machine learning, and deep learning techniques to predict future inflation trends accurately. 

The project seeks to transform raw CPI data into actionable insights that support evidence based policymaking, inflation monitoring, and economic planning.

# purpose of the Project

The purpose of this project is to build a robust analytical system that can analyze historical CPI patterns, identify key inflation drivers, forecast future CPI values, and present the findings through interactive dashboards. 

The project enables policymakers and economic analysts to make informed decisions related to monetary policy, inflation control, resource allocation, and economic stability.

# Challenges Faced by the Project

One of the major challenges was dealing with the time series nature of CPI data, where maintaining temporal order is critical during model development and evaluation. 

Another challenge involved selecting appropriate forecasting models capable of capturing trend, seasonality, and non linear inflation patterns. Limited availability of long historical datasets posed difficulties for deep learning approaches such as LSTM. 

Feature engineering was also challenging because inflation is influenced by multiple external economic factors. Additionally, comparing different forecasting approaches fairly using common evaluation metrics required careful experimentation.

# How the Challenges Were Overcome

The time dependency issue was addressed by implementing time series based train test splitting and time aware validation techniques. 

Stationarity checks using statistical tests helped in selecting suitable parameters for ARIMA based models. Feature engineering techniques such as lag features, moving averages, rolling volatility measures, and seasonal variables were introduced to capture inflation dynamics effectively. 

Hyperparameter tuning using cross validation techniques improved model performance. Multiple forecasting models were developed and evaluated using standardized metrics to ensure objective model selection. Economic indicators were incorporated through SARIMAX to improve forecasting accuracy and interpretability.

# Skills and Tools Covered in this Project and Why They Were Used

The project covered skills related to SQL, statistical analysis, machine learning, deep learning, time series forecasting, feature engineering, data visualization, and business intelligence reporting.

SQL was used to query, aggregate, and preprocess large CPI datasets efficiently because relational databases provide scalability and structured data management capabilities.

Python was used for data preprocessing, feature engineering, model development, and evaluation due to its extensive ecosystem of analytical libraries and flexibility in implementing forecasting techniques.

Pandas and NumPy were utilized for data manipulation and numerical computations because they simplify handling time series datasets.

Statsmodels was used for ARIMA, SARIMA, and SARIMAX models because it provides well established statistical forecasting frameworks suitable for economic data analysis.

Prophet was selected because it effectively models trend changes and seasonality with minimal parameter tuning.

XGBoost was used because it captures complex nonlinear relationships and interactions among multiple inflation related features while delivering strong predictive performance.

TensorFlow and Keras were employed for LSTM implementation because they provide robust deep learning capabilities for sequential data modeling.

Scikit learn was used for evaluation metrics, preprocessing, hyperparameter tuning, and validation because it offers standardized machine learning workflows.

Power BI and Tableau were used to build executive dashboards because they enable interactive visualization, drill down analysis, and effective communication of forecasting insights to decision makers.

These tools were chosen because they represent industry standard technologies commonly used in government analytics, economic forecasting, and enterprise data science projects.

# Limitations of the Project

The project relies heavily on the quality and completeness of historical CPI data, making forecasts sensitive to missing or inconsistent records. 

Traditional models such as ARIMA and SARIMA assume that historical patterns continue into the future, which may not hold during economic shocks. 

The forecasting models may not fully capture unexpected events such as geopolitical crises, pandemics, or sudden policy changes.

Deep learning models like LSTM may not perform optimally when limited historical observations are available. The project also assumes that external economic variables used in forecasting are accurately estimated for future periods.

# Analytics Performed in this Project

The project involved descriptive analytics to understand historical CPI trends and inflation behavior over time.

Diagnostic analytics were conducted through stationarity testing and statistical analysis to identify underlying data characteristics and relationships.

Predictive analytics formed the core of the project by forecasting future CPI values using ARIMA, SARIMA, SARIMAX, Prophet, XGBoost, and LSTM models.

Comparative analytics were performed to evaluate model performance using metrics such as MAE, RMSE, and MAPE.

Prescriptive analytics were supported through dashboards that provide insights for policy interventions and inflation management strategies.

# Key Outcomes of the Project

The project successfully developed an integrated forecasting pipeline capable of predicting CPI trends for future periods. Multiple forecasting models were implemented and systematically evaluated to identify the most effective approach. 

Feature engineering significantly improved predictive performance by incorporating historical dependencies and seasonal patterns. 

Interactive dashboards enabled stakeholders to monitor inflation trends and forecast outcomes effectively. The project demonstrated how advanced analytics can support evidence based economic policymaking and improve decision making processes.

# Explanation of the Project

This project focuses on developing a comprehensive Consumer Price Index forecasting system using statistical techniques, machine learning algorithms, and deep learning methods. 

Historical CPI data was collected and processed using SQL and Python based analytical workflows. Feature engineering techniques were applied to extract temporal patterns, seasonality, volatility, and lag based relationships from the data. 

Forecasting models including ARIMA, SARIMA, SARIMAX, Prophet, XGBoost, and LSTM were developed to predict future inflation trends. 

Hyperparameter tuning and model evaluation were performed using established performance metrics to identify the best forecasting approach. 

The forecasting outputs were integrated into Power BI and Tableau dashboards to provide policymakers and economic analysts with interactive tools for monitoring inflation and supporting strategic decision making. 

The project demonstrates the practical application of data science methodologies in addressing real world economic forecasting challenges.

# Conclusion of the Project and Where it Can Be Used

The project concludes that combining traditional statistical models with modern machine learning techniques provides a robust framework for CPI forecasting. 

While statistical models effectively capture trend and seasonality, machine learning approaches enhance predictive capability by modeling complex relationships within the data. 

The integration of forecasting outputs with business intelligence dashboards transforms analytical findings into actionable insights.

This project can be used by government agencies such as the Ministry of Statistics and Programme Implementation, Reserve Bank of India, NITI Aayog, economic research institutions, financial organizations, consulting firms, and policy think tanks to monitor inflation, support monetary policy decisions, perform economic planning, assess inflation risks, and facilitate evidence based policymaking.

# Drill Questions

### What is CPI and why is CPI forecasting important?

**Question:** What is CPI?

**Answer:** Consumer Price Index (CPI) measures the average change in prices paid by consumers for a basket of goods and services over time. It is a key indicator of inflation.

**Question:** Why is CPI forecasting important?

**Answer:** CPI forecasting helps governments and policymakers make decisions related to monetary policy, interest rates, inflation control, wage adjustments, and economic planning.

### Raw Data and Data Preparation

**Question:** Why did you use raw CPI data?

**Answer:** Raw CPI data serves as the primary source of inflation measurements. It provides historical trends required for forecasting future CPI values.

**Question:** Why convert the month column to datetime format?

**Answer:** Time-series models require date-time formatted indices to capture temporal dependencies, seasonality, and trends accurately.

```python
cpi['month'] = pd.to_datetime(cpi['month'])
```

**Question:** Why aggregate CPI data monthly?

**Answer:** CPI is typically reported monthly. Aggregation ensures consistency and reduces noise in forecasting.

```python
monthly_cpi = cpi.groupby('month')['cpi_index'].mean()
```

### SQL Aggregation Layer

**Question:** Why use SQL before machine learning?

**Answer:** SQL efficiently handles large datasets by performing filtering, aggregation, joins, and feature extraction before modeling.

**Question:** What types of SQL operations were used?

**Answer:** GROUP BY, JOIN, AVG(), window functions, and date-based aggregations.

### Train-Test Split

**Question:** Why use a train-test split?

**Answer:** It allows evaluation of model performance on unseen future data.

**Question:** Why use the last 12 months as the test set?

**Answer:** The last 12 months represent one complete annual cycle, preserving seasonality patterns.

```python
train = monthly_cpi[:-12]
test = monthly_cpi[-12:]
```

**Question:** Why not use random train-test splitting?

**Answer:** Random splitting causes data leakage in time-series forecasting because future observations may influence past predictions.

---

### Feature Engineering

### Lag Features

**Question:** What are lag features?

**Answer:** Lag features are previous observations used as predictors for future values.

**Question:** Why use lag_1?

**Answer:** It captures the immediate previous month's inflation impact.

```python
lag_1 = CPI(t−1)
```

**Question:** Why use lag_3?

**Answer:** It captures quarterly inflation effects.

**Question:** Why use lag_6?

**Answer:** It captures medium-term inflation trends over six months.

**Question:** Why use lag_12?

**Answer:** It captures yearly seasonality by comparing the same month in the previous year.

### Moving Average Features

**Question:** What is a moving average?

**Answer:** A moving average smooths short-term fluctuations and highlights long-term trends.

**Question:** Why use MA_3?

**Answer:** It captures short-term inflation trends.

```python
MA_3 = Average of previous 3 months
```

**Question:** Why use MA_6?

**Answer:** It captures medium-term inflation patterns.

**Question:** Why use MA_12?

**Answer:** It captures annual inflation trends and seasonality.

### Rolling Volatility

**Question:** What is rolling volatility?

**Answer:** Rolling volatility measures variation or instability in CPI values over a specific period.

```python
Volatility = Rolling Standard Deviation
```

**Question:** Why use a 6-month rolling window?

**Answer:** It balances responsiveness and stability in measuring inflation fluctuations.

### Seasonal Features

**Question:** Why extract month as a feature?

**Answer:** Different months exhibit distinct inflation behaviors due to festivals, weather, and agricultural cycles.

**Question:** Why extract quarter?

**Answer:** Quarterly patterns may influence inflation because of fiscal policies and economic cycles.

**Question:** Why extract year?

**Answer:** It helps capture long-term trends and structural changes.

### Missing Value Handling

**Question:** Why drop missing values?

**Answer:** Lag and moving average calculations generate NaN values. Most machine learning algorithms cannot process missing values directly.

```python
monthly_cpi.dropna()
```

---

### Evaluation Metrics

### Mean Absolute Error (MAE)

**Question:** What is MAE?

**Answer:** MAE measures the average absolute difference between actual and predicted values.

Formula:

MAE = Σ |Actual − Predicted| / n

**Question:** Why use MAE?

**Answer:** It provides an easily interpretable measure of forecast accuracy.

### Root Mean Squared Error (RMSE)

**Question:** What is RMSE?

**Answer:** RMSE measures the square root of average squared errors.

Formula:

RMSE = √[Σ(Actual − Predicted)² / n]

**Question:** Why use RMSE?

**Answer:** It penalizes larger errors more heavily than MAE.

### Mean Absolute Percentage Error (MAPE)

**Question:** What is MAPE?

**Answer:** MAPE expresses prediction error as a percentage.

Formula:

MAPE = (1/n) × Σ |(Actual − Predicted)/Actual| × 100

**Question:** Why use MAPE?

**Answer:** It provides scale-independent interpretability.

## ARIMA Model

### ARIMA Parameters

**Question:** What does ARIMA stand for?

**Answer:** AutoRegressive Integrated Moving Average.

### Autoregressive Component (p)

**Question:** What is the parameter p?

**Answer:** p represents the number of lag observations used in the model.

**Question:** Why was p = 2 selected?

**Answer:** PACF showed significant spikes up to lag 2, indicating dependency on the previous two observations.

### Integrated Component (d)

**Question:** What is d?

**Answer:** d is the number of differencing operations required to achieve stationarity.

**Question:** Why was d = 1 selected?

**Answer:** The original CPI series was non-stationary according to the ADF test, and first differencing achieved stationarity.

### Moving Average Component (q)

**Question:** What is q?

**Answer:** q represents the number of lagged forecast errors used.

**Question:** Why was q = 2 selected?

**Answer:** ACF plots showed significant autocorrelation up to lag 2.

### Stationarity Testing

**Question:** What is stationarity?

**Answer:** A stationary series has constant mean, variance, and covariance over time.

**Question:** Why is stationarity important?

**Answer:** ARIMA assumes the underlying series is stationary.

### Augmented Dickey-Fuller Test

**Question:** What is the ADF test?

**Answer:** A statistical test used to determine whether a time series is stationary.

**Question:** How do you interpret the p-value?

**Answer:** p-value < 0.05 indicates stationarity. p-value > 0.05 indicates non-stationarity.

## SARIMA Model

**Question:** What does SARIMA stand for?

**Answer:** Seasonal AutoRegressive Integrated Moving Average.

**Question:** Why use SARIMA instead of ARIMA?

**Answer:** SARIMA captures both trend and seasonal patterns.

### Seasonal Parameters

**Question:** What is seasonal_order = (P,D,Q,m)?

**Answer:** It defines seasonal autoregression, seasonal differencing, seasonal moving average, and seasonal periodicity.

### Seasonal AR Parameter (P)

**Question:** What is P?

**Answer:** Number of seasonal autoregressive terms.

### Seasonal Differencing (D)

**Question:** What is D?

**Answer:** Number of seasonal differencing operations required.

### Seasonal MA Parameter (Q)

**Question:** What is Q?

**Answer:** Number of seasonal moving average terms.

### Seasonal Period (m)

**Question:** Why was m = 12 selected?

**Answer:** CPI data is monthly, and inflation patterns repeat annually.

## SARIMAX Model

**Question:** What does SARIMAX stand for?

**Answer:** Seasonal ARIMA with Exogenous Variables.

**Question:** Why use SARIMAX?

**Answer:** CPI is influenced by external economic indicators, not just historical CPI values.

### Exogenous Variables

**Question:** What are exogenous variables?

**Answer:** External variables that influence the target variable.

### Food Inflation

**Question:** Why include food inflation?

**Answer:** Food constitutes a major portion of the CPI basket.

### Fuel Inflation

**Question:** Why include fuel inflation?

**Answer:** Fuel prices affect transportation and production costs.

### WPI

**Question:** What is WPI?

**Answer:** Wholesale Price Index measures inflation at the producer level.

**Question:** Why include WPI?

**Answer:** Producer price changes often precede consumer price changes.

### Repo Rate

**Question:** What is Repo Rate?

**Answer:** The interest rate at which the central bank lends money to commercial banks.

**Question:** Why include Repo Rate?

**Answer:** Monetary policy influences inflation dynamics.

### Confidence Intervals

**Question:** What are confidence intervals?

**Answer:** They represent the range within which future CPI values are expected to lie with a certain probability.

**Question:** Why are confidence intervals important?

**Answer:** They quantify forecast uncertainty.

## Prophet Model

**Question:** Why use Prophet?

**Answer:** Prophet handles trend changes, seasonality, and missing data effectively.

### yearly_seasonality=True

**Question:** Why enable yearly seasonality?

**Answer:** CPI exhibits annual seasonal patterns.

### weekly_seasonality=False

**Question:** Why disable weekly seasonality?

**Answer:** CPI data is monthly, making weekly effects irrelevant.

### daily_seasonality=False

**Question:** Why disable daily seasonality?

**Answer:** Daily patterns do not exist in monthly CPI data.

### changepoint_prior_scale=0.05

**Question:** What is changepoint_prior_scale?

**Answer:** It controls Prophet's flexibility in detecting trend changes.

**Question:** Why use 0.05?

**Answer:** It balances overfitting and underfitting by allowing moderate trend flexibility.

## XGBoost Model

**Question:** Why use XGBoost?

**Answer:** XGBoost captures complex non-linear relationships and interactions among variables.

### n_estimators

**Question:** What is n_estimators?

**Answer:** Number of trees built sequentially.

**Question:** Why test 100 and 200?

**Answer:** To balance predictive performance and computational cost.

### max_depth

**Question:** What is max_depth?

**Answer:** Maximum depth of each tree.

**Question:** Why tune max_depth?

**Answer:** Larger depth captures complexity but increases overfitting risk.

### learning_rate

**Question:** What is learning_rate?

**Answer:** It controls the contribution of each new tree.

**Question:** Why use 0.01, 0.05, and 0.1?

**Answer:** Lower values improve generalization but require more trees.

### subsample

**Question:** What is subsample?

**Answer:** Fraction of observations used to build each tree.

**Question:** Why use 0.8 and 1?

**Answer:** Subsampling reduces overfitting while maintaining diversity.

### TimeSeriesSplit

**Question:** Why use TimeSeriesSplit?

**Answer:** It preserves temporal ordering during cross-validation.

### GridSearchCV

**Question:** Why use GridSearchCV?

**Answer:** It systematically searches for optimal hyperparameter combinations.

## LSTM Model

**Question:** What is LSTM?

**Answer:** Long Short-Term Memory is a recurrent neural network designed for sequential data.

**Question:** Why use LSTM for CPI forecasting?

**Answer:** LSTM captures long-term dependencies and non-linear temporal patterns.

### MinMaxScaler

**Question:** Why use MinMaxScaler?

**Answer:** Neural networks perform better when inputs are normalized.

### time_step = 12

**Question:** What does time_step = 12 mean?

**Answer:** The model uses the previous 12 months to predict the next month.

**Question:** Why choose 12?

**Answer:** It captures one complete annual cycle.

### LSTM Units = 64

**Question:** What do 64 units represent?

**Answer:** Number of memory cells in the first LSTM layer.

**Question:** Why choose 64 units?

**Answer:** It provides sufficient capacity to learn complex patterns without excessive computation.

### return_sequences=True

**Question:** What does return_sequences=True do?

**Answer:** It returns outputs for all time steps, enabling stacking of LSTM layers.

### Dropout = 0.2

**Question:** What is dropout?

**Answer:** Dropout randomly deactivates neurons during training.

**Question:** Why use 0.2?

**Answer:** It reduces overfitting while preserving learning capability.

### Second LSTM Layer = 32 Units

**Question:** Why use a second LSTM layer?

**Answer:** Multiple layers capture higher-level temporal abstractions.

### Dense Layer

**Question:** Why use a Dense layer?

**Answer:** It transforms learned features into the final CPI prediction.

### Adam Optimizer

**Question:** What is Adam?

**Answer:** An adaptive optimization algorithm combining momentum and RMSProp.

**Question:** Why use Adam?

**Answer:** It converges quickly and efficiently for deep learning problems.

### Loss = MSE

**Question:** Why use Mean Squared Error as the loss function?

**Answer:** MSE penalizes large forecasting errors heavily, improving prediction accuracy.

### Epochs = 100

**Question:** What is an epoch?

**Answer:** One complete pass through the training dataset.

**Question:** Why use 100 epochs?

**Answer:** It provides adequate learning opportunities while monitoring overfitting.

### Batch Size = 16

**Question:** What is batch size?

**Answer:** Number of samples processed before updating weights.

**Question:** Why choose 16?

**Answer:** It balances training stability and computational efficiency.

## Model Selection

**Question:** Why compare multiple models?

**Answer:** Different models capture different characteristics of CPI behavior, and comparison ensures selection of the best-performing approach.

**Question:** Which metric determines the best model?

**Answer:** The model with the lowest MAE, RMSE, and MAPE is preferred.

**Question:** When is ARIMA preferred?

**Answer:** When data exhibits simple linear temporal patterns without seasonality.

**Question:** When is SARIMA preferred?

**Answer:** When strong seasonal inflation patterns exist.

**Question:** When is SARIMAX preferred?

**Answer:** When external economic variables significantly influence CPI.

**Question:** When is Prophet preferred?

**Answer:** When trend shifts and structural changes occur frequently.

**Question:** When is XGBoost preferred?

**Answer:** When inflation is driven by complex non-linear interactions among multiple variables.

**Question:** When is LSTM preferred?

**Answer:** When large historical datasets with long-term temporal dependencies are available.

### Forecasting and Deployment

**Question:** Why forecast the next 12 months?

**Answer:** A 12-month horizon aligns with annual policy planning and inflation target setting.

**Question:** Why save trained models?

**Answer:** Saving models avoids retraining and supports deployment.

```python
joblib.dump(model, 'model.pkl')
```

**Question:** Why save forecasts as CSV files?

**Answer:** CSV files facilitate reporting, dashboard creation, and integration with downstream systems.

**Question:** Why use Power BI or Tableau dashboards?

**Answer:** Dashboards provide interactive visualization of inflation trends, forecasts, confidence intervals, and model comparisons for policymakers and stakeholders.

-----------------------------------------------------------------------------------------------------------------------------

# Project 4 : Press conference on the release of the GDP Data : base year 2022 – 2023 

Step 1: SQL Data Extraction from NAS

SELECT
    year,
    quarter,
    sector_code,
    sector_name,
    gva_current_prices,
    gdp_current_prices
FROM nas_gdp_statistics
WHERE year >= '2022-23';

Step 2: SQL Data Extraction from GST

SELECT
    filing_period,
    state_code,
    industry_code,
    taxable_value,
    cgst_amount,
    sgst_amount,
    igst_amount
FROM gst_transactions
WHERE filing_period >= '2022-04';

Step 3: SQL Data Extraction from ASI

SELECT
    survey_year,
    state_code,
    nic_code,
    total_output,
    intermediate_consumption,
    gross_value_added
FROM asi_industries
WHERE survey_year >= 2022;

Step 4: SQL Data Extraction from HCES

SELECT
    survey_year,
    state_code,
    sector,
    expenditure_category,
    monthly_consumption_expenditure
FROM hces_household_expenditure;

Step 5: SQL Data Extraction from CPI

SELECT
    reference_month,
    state_code,
    commodity_group,
    cpi_index,
    inflation_rate
FROM cpi_statistics
WHERE reference_month >= '2022-04';

Step 6: Load Data into Python

import pandas as pd

nas = pd.read_csv("nas.csv")
gst = pd.read_csv("gst.csv")
asi = pd.read_csv("asi.csv")
hces = pd.read_csv("hces.csv")
cpi = pd.read_csv("cpi.csv")

Step 7: Data Profiling/ EDA/ data understanding

print(nas.shape)
print(nas.info())

print(gst.shape)
print(gst.info())

print(asi.shape)
print(asi.info())

print(hces.shape)
print(hces.info())

print(cpi.shape)
print(cpi.info())
Step 8: Missing Value Analysis/ individual ran code for datasets
print(nas.isnull().sum())
print(gst.isnull().sum())
print(asi.isnull().sum())
print(hces.isnull().sum())
print(cpi.isnull().sum())

Step 9: Missing Value Treatment – ran code for all datasets 

gst["taxable_value"] = gst["taxable_value"].fillna(
    gst["taxable_value"].median()
)

cpi["inflation_rate"] = cpi["inflation_rate"].fillna(
    cpi["inflation_rate"].mean()
)

Step 10: Duplicate Detection– ran code for all datasets 

gst.duplicated().sum()

Step 11: Remove Duplicates / business requirement is to keep only one record per GSTIN  

gst = gst.drop_duplicates()
gst = gst.drop_duplicates(
    subset=["GSTIN"],
    keep="first"
)

Step 12: Data Type Conversion- – ran code for all datasets 

gst["filing_period"] = pd.to_datetime(
    gst["filing_period"]
)

nas["year"] = nas["year"].astype(str)

Step 13: Outlier Detection (IQR)- – ran code for all datasets 

Q1 = gst["taxable_value"].quantile(0.25)
Q3 = gst["taxable_value"].quantile(0.75)

IQR = Q3 - Q1

lower = Q1 - 1.5 * IQR
upper = Q3 + 1.5 * IQR

outliers = gst[
    (gst["taxable_value"] < lower) |
    (gst["taxable_value"] > upper)
]

Step 14: Data Integration

gdp_df = (
    nas
    .merge(
        gst,
        on=["state_code"],
        how="left"
    )
)
Multiple datasets:
gdp_df = (
    nas
    .merge(gst, on="state_code")
    .merge(asi, on="state_code")
    .merge(hces, on="state_code")
)

Step 15: Feature Engineering

gdp_df["gst_growth"] = (
    gdp_df["total_turnover"].pct_change()
)

gdp_df["consumption_growth"] = (
    gdp_df["total_consumption"].pct_change()
)

Step 16: Exploratory Data Analysis

gdp_df.describe()

gdp_df.corr(numeric_only=True)

Step 17: Prepare Data for ARIMA

ARIMA works only on the target time-series (GDP values).
gdp_series = gdp_df["gdp_current_prices"]
Step 18: Train-Test Split for ARIMA (Chronological Split)
Since ARIMA is a Time Series model, we do not use train_test_split().
train = gdp_series[:-5]
test = gdp_series[-5:]

Explanation
•	Training Data = All historical GDP values except the last 5 periods.
•	Test Data = Last 5 periods.
•	Maintains chronological order.

Why did you keep the last 5 periods as test data?

The last 5 periods simulate future unseen data. This allows us to evaluate how well the model predicts upcoming GDP values.

Step 19: ARIMA Model Training

from statsmodels.tsa.arima.model import ARIMA

model = ARIMA(
    train,
    order=(2,1,2)
)

result = model.fit()

Explanation
•	p = 2 → Auto-Regressive terms
•	d = 1 → Differencing
•	q = 2 → Moving Average terms

Explanation: 

How did you choose ARIMA(2,1,2)? What do p, d, q mean?
Meaning of p, d, q
ARIMA(p,d,q)

1. p = Auto-Regressive (AR) Term
•	Number of past observations used to predict the current value.
•	p = 2 means the model uses GDP values from the previous 2 periods.

2. d = Differencing Order
•	Used to make the time series stationary.
•	Removes trend and non-stationarity.

3. q = Moving Average (MA) Term
•	Uses past forecast errors to improve future predictions.
•	q = 2 means the model considers the previous two error terms.

How did you choose p=2, d=1, q=2?

I first checked whether the GDP data had a trend over time. Since it was increasing over the years, I applied first-order differencing (d=1) to make the data stable. Then I tried different ARIMA parameter combinations and selected ARIMA(2,1,2). 
Step 20: ARIMA Forecasting
forecast = result.forecast(steps=5)

Explanation: 

After training the ARIMA model, I used forecast(steps=5) to generate predictions for the next 5 time periods. The model used historical GDP patterns and trends to estimate future GDP values."

Step 21: ARIMA Evaluation

from sklearn.metrics import (
    mean_absolute_percentage_error,
    mean_squared_error
)

arima_mape = mean_absolute_percentage_error(
    test,
    forecast
)

arima_rmse = mean_squared_error(
    test,
    forecast
) ** 0.5

print(arima_mape)
print(arima_rmse)

Step 22: Prepare Features for Random Forest

X = gdp_df[
    [
        "total_turnover",
        "gva",
        "total_consumption",
        "cpi_index"
    ]
]

y = gdp_df["gdp_current_prices"]

Explanation

Features:
•	GST Turnover
•	Gross Value Added (GVA)
•	Consumption
•	CPI

Target:
•	GDP

Step 23: Train-Test Split for Random Forest

from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(
    X,
    y,
    test_size=0.2,
    random_state=42
)

Explanation
•	80% Training Data
•	20% Testing Data

Step 24: Train Random Forest Model

from sklearn.ensemble import RandomForestRegressor

rf = RandomForestRegressor(
    n_estimators=100,
    random_state=42
)

rf.fit(
    X_train,
    y_train
)

Step 25: Random Forest Prediction

y_pred = rf.predict(
    X_test
)

Step 26: Random Forest Evaluation

rf_mape = mean_absolute_percentage_error(
    y_test,
    y_pred
)

rf_rmse = mean_squared_error(
    y_test,
    y_pred
) ** 0.5

print(rf_mape)
print(rf_rmse)

Step 27: Tableau / Power BI Dashboard

How I Connected the Final Dataset to Tableau & Power BI

After completing data cleaning, feature engineering, and forecasting, I exported the final dataset:
gdp_df.to_csv(
    "GDP_Dashboard_Data.csv",
    index=False
)

How I Connected the Final Dataset to Tableau & Power BI

After completing data cleaning, feature engineering, and forecasting, I exported the final dataset:
gdp_df.to_csv(
    "GDP_Dashboard_Data.csv",
    index=False
)

Tableau Dashboard Development Steps

Step 1: Connect Data Source
•	Open Tableau Desktop
•	Click Connect → Text File
•	Select GDP_Dashboard_Data.csv

Power BI Dashboard Development Steps

Step 1: Import Data
•	Open Power BI Desktop
•	Get Data → CSV
•	Import GDP_Dashboard_Data.csv

You built both Tableau and Power BI dashboards on the same GDP project. Why did you create different dashboards instead of replicating the same visualizations?

Tableau Dashboard Development 

I used Tableau for exploratory analysis, forecasting, and trend visualization. I created three dashboards: a GDP Forecasting Dashboard to analyze Year-wise and Quarter-wise GDP trends, GDP Growth %, and ARIMA forecasted GDP values; an Economic Driver Analysis Dashboard to study the impact of GST Turnover, GVA, Consumption Growth, and CPI on GDP using correlation and trend analysis; and a State-wise Economic Analysis Dashboard to compare GDP, GST Collection, Consumption Expenditure, and Inflation Rate across states. Tableau helped me create interactive visualizations, forecasting views, and geographical analysis for deeper economic insights.

Power BI Dashboard Development 

I used Power BI for executive reporting and KPI monitoring. I developed three dashboards: an Executive Economic Performance Dashboard containing Total GDP, GDP Growth %, Total GVA, Total Consumption, Inflation Rate, and Forecast GDP; a Sector Performance Dashboard analyzing Manufacturing, Services, and Agriculture GVA along with Sector Contribution %; and a Tax & Consumption Dashboard focusing on GST Collection, GST Growth %, Rural Consumption, Urban Consumption, and Category-wise Consumption. Power BI's DAX measures and KPI-focused reporting made it ideal for management-level decision-making and performance tracking.

I used Tableau for analytical exploration and forecasting, while Power BI was used for executive reporting, KPI monitoring, and business decision support.

Interview Summary (Project Flow):

SQL → Data Extraction → Python (Cleaning, Missing Values, Duplicates, Outliers, Integration) → EDA → Feature Engineering → ARIMA/Random Forest Forecasting → Model Evaluation → Tableau/Power BI Dashboard Creation.

                                            *****
