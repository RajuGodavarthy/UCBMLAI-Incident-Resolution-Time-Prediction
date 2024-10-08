# Predicting Incident Closure Time Using Machine Learning

## Project Context
In today's fast-paced IT service management (ITSM) environment, incident management is crucial for ensuring smooth business operations. Efficiently predicting the time to resolve incidents can significantly improve resource allocation, meet Service Level Agreements (SLAs), and enhance customer satisfaction.

## Key Problem
The primary objective is to predict the time and date for final closure of an open ticket using a range of attributes related to incidents. Given the multi-stage progression of incidents with potentially several updates, the challenge is to aggregate and extract meaningful patterns that help forecast how long it will take to fully resolve each incident.

## Motivation
In ITSM, accurately predicting incident resolution times enables:
- Better planning for engineer workloads, ensuring balanced distribution of cases.
- Improved customer satisfaction by providing more accurate estimates of incident resolution times.
- Meeting SLAs by identifying potentially delayed incidents early and taking corrective action.
- Resource optimization through effective incident management by anticipating workloads and avoiding bottlenecks.

## Dataset Description
- **Source**: UCI Machine Learning Repository  
- **URL**: [Incident Management Process Enriched Event Log](https://archive.ics.uci.edu/dataset/498/incident+management+process+enriched+event+log)  
- **Dataset Information**: This is an event log of an incident management process extracted from data gathered from the audit system of an instance of the ServiceNow™ platform used by an IT company. The event log is enriched with data loaded from a relational database underlying a corresponding process-aware information system. Information was anonymized for privacy.
  - **Number of instances**: 141,712 events (24,918 incidents)
  - **Number of attributes**: 36 attributes (1 case identifier, 1 state identifier, 32 descriptive attributes, 2 dependent variables)
  - **Has missing values**: Yes

### Variable Information
1. **number**: incident identifier (24,918 different values)
2. **incident state**: eight levels controlling the incident management process transitions from opening until closing the case
3. **active**: boolean attribute that shows whether the record is active or closed/canceled
4. **reassignment_count**: number of times the incident has the group or the support analysts changed
5. **reopen_count**: number of times the incident resolution was rejected by the caller
6. **sys_mod_count**: number of incident updates until that moment
7. **made_sla**: boolean attribute that shows whether the incident exceeded the target SLA
8. **caller_id**: identifier of the user affected
9. **opened_by**: identifier of the user who reported the incident
10. **opened_at**: incident user opening date and time
11. **sys_created_by**: identifier of the user who registered the incident
12. **sys_created_at**: incident system creation date and time
13. **sys_updated_by**: identifier of the user who updated the incident and generated the current log record
14. **sys_updated_at**: incident system update date and time
15. **contact_type**: categorical attribute that shows by what means the incident was reported
16. **location**: identifier of the location of the place affected
17. **category**: first-level description of the affected service
18. **subcategory**: second-level description of the affected service (related to the first level description, i.e., to category)
19. **u_symptom**: description of the user perception about service availability
20. **cmdb_ci**: (confirmation item) identifier used to report the affected item (not mandatory)
21. **impact**: description of the impact caused by the incident (values: 1 - High; 2 - Medium; 3 - Low)
22. **urgency**: description of the urgency informed by the user for the incident resolution (values: 1 - High; 2 - Medium; 3 – Low)
23. **priority**: calculated by the system based on 'impact' and 'urgency'
24. **assignment_group**: identifier of the support group in charge of the incident
25. **assigned_to**: identifier of the user in charge of the incident
26. **knowledge**: boolean attribute that shows whether a knowledge base document was used to resolve the incident
27. **u_priority_confirmation**: boolean attribute that shows whether the priority field has been double-checked
28. **notify**: categorical attribute that shows whether notifications were generated for the incident
29. **problem_id**: identifier of the problem associated with the incident
30. **rfc**: (request for change) identifier of the change request associated with the incident
31. **vendor**: identifier of the vendor in charge of the incident
32. **caused_by**: identifier of the RFC responsible for the incident
33. **close_code**: identifier of the resolution of the incident
34. **resolved_by**: identifier of the user who resolved the incident
35. **resolved_at**: incident user resolution date and time (dependent variable)
36. **closed_at**: incident user close date and time (dependent variable)

## Objective
- Predict the estimated closure time of an incident (in hours or days) from its creation, based on the attributes collected from multiple incident stages.
- The model predicts the Time to close: The total time (in hours/days) between when an incident is opened and when it is fully closed.

## Solution Approach
1. **Data Aggregation**: Incidents may have multiple entries as they transition between states. We need to aggregate these records to build useful features (e.g., time spent in each state, total reassignment count, time from `opened_at` to `resolved_at`).
2. **Feature Engineering**: Transform the dataset by creating features that describe the incident's history, such as:
   - Total time spent in each state (Active, Awaiting User Info, Resolved)
   - Reassignment and reopening counts
   - Whether the SLA was met, urgency, and priority
3. **Machine Learning Models**: Train various regression models to predict the time to closure. Models include:
   - Linear Regression
   - Random Forest Regressor
   - XGBoost
4. **Model Evaluation**: Evaluate each model's performance using metrics like Mean Absolute Error (MAE), Root Mean Squared Error (RMSE), and R² score. The model with the best performance will be selected for deployment.

## Success Criteria
- Low error rates (low MAE and RMSE) indicate accurate predictions of the time to close incidents.
- High R² score implies the model captures a significant portion of the variance in the incident closure time.
- The model should generalize well, ensuring good performance on unseen data (i.e., a small gap between training and test set performance).

## Findings
### Performance Metrics for Various Regression Models
| Model                | MAE       | RMSE      | R²        |
|----------------------|-----------|-----------|-----------|
| Linear Regression     | 0.39195   | 0.529234  | 0.999946  |
| Random Forest         | 1.49919   | 5.546661  | 0.994063  |
| Original XGBoost     | 1.49919   | 5.546661  | 0.994063  |
| 1st Tuned XGBoost    | 0.17985   | 0.835051  | 0.999865  |
| 2nd Tuned XGBoost    | 0.13522   | 0.773848  | 0.999884  |

### Interpretation of Performance Metrics
1. **Mean Absolute Error (MAE)**:
   - Linear Regression: MAE of 0.391950 indicates an average error of about 0.39 time units in predicting the closure time.
   - Random Forest & Original XGBoost: Both have a significantly higher MAE of 1.499185, showing larger average prediction errors compared to Linear Regression.
   - 1st Tuned XGBoost: The MAE improves drastically to 0.179850, indicating much better performance in predicting the target variable.
   - 2nd Tuned XGBoost: Further tuning leads to an even lower MAE of 0.135220, suggesting excellent predictive capability.

2. **Root Mean Squared Error (RMSE)**:
   - Linear Regression: RMSE of 0.529234 suggests a reasonable level of prediction error, but it's higher than the tuned XGBoost models.
   - Random Forest & Original XGBoost: With an RMSE of 5.546661, both models perform poorly in prediction accuracy compared to Linear Regression and tuned XGBoost models.
   - 1st Tuned XGBoost: The RMSE is significantly lower at 0.835051, indicating closer predictions to actual values on average.
   - 2nd Tuned XGBoost: The RMSE improves further to 0.773848, showing refined predictive performance after tuning.

3. **R-squared (R²)**:
   - Linear Regression: An R² value of 0.999946 indicates that the model explains approximately 99.99% of the variance in the target variable, which is excellent.
   - Random Forest & Original XGBoost: With an R² value of 0.994063, these models explain about 99.41% of the variance, which is strong but not as effective as Linear Regression.
   - 1st Tuned XGBoost: R² increases to 0.999865, suggesting improved explanatory power post-tuning.
   - 2nd Tuned XGBoost: R² reaches 0.999884, reflecting the model's ability to account for nearly all variability in the closure time prediction.

### Conclusion
- The final model selection is based on performance metrics, with the second tuned XGBoost model showing superior predictive capability, closely followed by Linear Regression.
- The analysis indicates that incident closure times can be effectively predicted, aiding in better resource management and customer satisfaction.

## Dependencies
- Python 3.x
- pandas
- numpy
- scikit-learn
- xgboost
- matplotlib
- seaborn

## Installation
To run the project, clone the repository and install the required packages:

```bash
git clone <repository-url>
cd <repository-directory>
pip install -r requirements.txt
