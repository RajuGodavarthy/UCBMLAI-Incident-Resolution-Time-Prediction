# Predicting Incident Closure Time Using Machine Learning

## Project Context

In today's fast-paced IT service management (ITSM) environment, incident management is crucial for ensuring smooth business operations. Efficiently predicting the time to resolve incidents can significantly improve resource allocation, meet Service Level Agreements (SLAs), and enhance customer satisfaction.

## Key Problem

The primary objective is to predict the **time and date for final closure** of an open ticket using a range of attributes related to incidents. Given the multi-stage progression of incidents with potentially several updates, the challenge is to aggregate and extract meaningful patterns that help forecast how long it will take to fully resolve each incident.

## Motivation

In ITSM, accurately predicting incident resolution times enables:
- Better planning for engineer workloads, ensuring balanced distribution of cases.
- Improved customer satisfaction by providing more accurate estimates of incident resolution times.
- Meeting SLAs by identifying potentially delayed incidents early and taking corrective action.
- Resource optimization through effective incident management by anticipating workloads and avoiding bottlenecks.

## Dataset Description

**Source:** [UCI Machine Learning Repository](https://archive.ics.uci.edu/dataset/498/incident+management+process+enriched+event+log)

### Dataset Information:
- **Number of instances:** 141,712 events (24,918 incidents)
- **Number of attributes:** 36 attributes (1 case identifier, 1 state identifier, 32 descriptive attributes, 2 dependent variables)
- **Missing values:** Yes

### Variable Information:
1. **number:** Incident identifier (24,918 unique values)
2. **incident state:** Eight levels controlling the incident management process transitions from opening to closing.
3. **active:** Boolean attribute indicating if the record is active or closed/canceled.
4. **reassignment_count:** Number of times the incident has been reassigned.
5. **reopen_count:** Number of times the incident resolution was rejected.
6. **sys_mod_count:** Number of incident updates until that moment.
7. **made_sla:** Boolean attribute showing if the incident exceeded the target SLA.
8. **caller_id:** Identifier of the affected user.
9. **opened_by:** Identifier of the user who reported the incident.
10. **opened_at:** Incident user opening date and time.
11. **sys_created_by:** Identifier of the user who registered the incident.
12. **sys_created_at:** Incident system creation date and time.
13. **sys_updated_by:** Identifier of the user who updated the incident.
14. **sys_updated_at:** Incident system update date and time.
15. **contact_type:** How the incident was reported.
16. **location:** Identifier of the location of the place affected.
17. **category:** First-level description of the affected service.
18. **subcategory:** Second-level description of the affected service.
19. **impact:** Impact caused by the incident (1 - High, 2 - Medium, 3 - Low).
20. **urgency:** Urgency for resolution (1 - High, 2 - Medium, 3 - Low).
21. **priority:** Calculated priority based on impact and urgency.
22. **assignment_group:** Support group in charge of the incident.
23. **assigned_to:** User in charge of the incident.
24. **knowledge:** Boolean showing if a knowledge base document was used.
25. **u_priority_confirmation:** Boolean showing if the priority field was confirmed.
26. **notify:** Indicates whether notifications were generated.
27. **problem_id:** Identifier of the associated problem.
28. **rfc:** Request for change identifier associated with the incident.
29. **vendor:** Identifier of the vendor in charge of the incident.
30. **caused_by:** Identifier of the RFC responsible for the incident.
31. **close_code:** Identifier of the resolution of the incident.
32. **resolved_by:** Identifier of the user who resolved the incident.
33. **resolved_at:** Incident resolution date and time (dependent variable).
34. **closed_at:** Incident closure date and time (dependent variable).

## Objective

- Predict the **estimated closure time** of an incident (in hours or days) from its creation, based on attributes collected at multiple stages.
- The model predicts the **Time to close**: The total time between when an incident is opened and fully closed.

## Solution Approach

1. **Data Aggregation:** 
   - Incidents may have multiple entries as they transition between states. We aggregate these records to build useful features (e.g., time spent in each state, total reassignment count).
   
2. **Feature Engineering:** 
   - Create features describing the incident's history, such as:
     - Total time spent in each state (Active, Awaiting User Info, Resolved).
     - Reassignment and reopening counts.
     - SLA status, urgency, and priority.
   
3. **Machine Learning Models:** 
   - Train various regression models to predict time to closure, including:
     - Linear Regression
     - Random Forest Regressor
     - Gradient Boosting Regressor
     - XGBoost
     - Support Vector Regressor (SVR)
   
4. **Model Evaluation:** 
   - Use metrics like **Mean Absolute Error (MAE)**, **Root Mean Squared Error (RMSE)**, and **R² score**. Select the best-performing model for deployment.

## Success Criteria

- Low **MAE** and **RMSE** indicate accurate predictions of the time to close incidents.
- High **R² score** means the model captures a significant portion of variance in incident closure time.
- The model should generalize well, ensuring minimal performance gap between training and test datasets.

## Benefits

- **Operational Efficiency:** Better prediction of closure times helps manage workloads, optimize workflows, and avoid bottlenecks.
- **SLA Compliance:** Accurate predictions enable teams to resolve incidents within the required SLA timeframes.
- **Customer Satisfaction:** Providing accurate resolution time estimates improves satisfaction and trust in ITSM processes.

## Challenges

- **Handling Multiple Entries Per Incident:** Aggregating multiple records for each incident while retaining meaningful information.
- **Complex Relationships:** Variables like reassignment counts, reopen counts, and time spent in different states may have non-linear relationships, requiring advanced models.

## Potential Extensions

- **Real-Time Monitoring:** Extend the model for real-time predictions as new incident updates occur.
- **ITSM Platform Integration:** Integrate the model into existing ITSM platforms (e.g., ServiceNow) for automated predictions in ticketing workflows.

## Conclusion

This project enhances incident management by leveraging machine learning to predict incident closure times, improving operational efficiency, SLA compliance, and customer satisfaction.
