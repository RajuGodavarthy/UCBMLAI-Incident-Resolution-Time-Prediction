# UCBMLAI-Incident-Resolution-Time-Prediction
Predicting Incident Closure Time Using Machine Learning
Project Context: 
In today's fast-paced IT service management (ITSM) environment, incident management is crucial for ensuring smooth business operations. Efficiently predicting the time to resolve incidents can significantly improve resource allocation, meet Service Level Agreements (SLAs), and enhance customer satisfaction.
Key Problem:
The primary objective is to predict the time and date for final closure of an open ticket using a range of attributes related to incidents.
Given the multi-stage progression of incidents with potentially several updates, the challenge is to aggregate and extract meaningful patterns that help forecast how long it will take to fully resolve each incident.
Motivation:
In ITSM, accurately predicting incident resolution times enables:
•	Better planning for engineer workloads, ensuring balanced distribution of cases.
•	Improved customer satisfaction by providing more accurate estimates of incident resolution times.
•	Meeting SLAs by identifying potentially delayed incidents early and taking corrective action.
•	Resource optimization through effective incident management by anticipating workloads and avoiding bottlenecks.
Dataset Description: 
Source: UCI Machine Learning Repository 
URL: https://archive.ics.uci.edu/dataset/498/incident+management+process+enriched+event+log
Dataset Information:
This is an event log of an incident management process extracted from data gathered from the audit system of an instance of the ServiceNowTM platform used by an IT company. The event log is enriched with data loaded from a relational database underlying a corresponding process-aware information system. Information was anonymized for privacy. 
•	Number of instances: 141,712 events (24,918 incidents) 
•	Number of attributes: 36 attributes (1 case identifier, 1 state identifier, 32 descriptive attributes, 2 dependent variables)
•	Has missing values: Yes


Variable Information:
1.	number: incident identifier (24,918 different values)
2.	incident state: eight levels controlling the incident management process transitions from opening until   closing the case
3.	active: boolean attribute that shows whether the record is active or closed/canceled
4.	reassignment_count: number of times the incident has the group or the support analysts changed
5.	reopen_count: number of times the incident resolution was rejected by the caller
6.	sys_mod_count: number of incident updates until that moment
7.	made_sla: boolean attribute that shows whether the incident exceeded the target SLA
8.	caller_id: identifier of the user affected
9.	opened_by: identifier of the user who reported the incident
10.	opened_at: incident user opening date and time
11.	sys_created_by: identifier of the user who registered the incident
12.	sys_created_at: incident system creation date and time
13.	sys_updated_by: identifier of the user who updated the incident and generated the current log record
14.	sys_updated_at: incident system update date and time
15.	contact_type: categorical attribute that shows by what means the incident was reported
16.	location: identifier of the location of the place affected
17.	category: first-level description of the affected service
18.	subcategory: second-level description of the affected service (related to the first level description, i.e., to category)
19.	u_symptom: description of the user perception about service availability
20.	cmdb_ci: (confirmation item) identifier used to report the affected item (not mandatory)
21.	impact: description of the impact caused by the incident (values: 1 - High; 2 - Medium; 3 - Low)
22.	urgency: description of the urgency informed by the user for the incident resolution (values: 1 - High; 2 - Medium; 3 – Low)
23.	priority: calculated by the system based on 'impact' and 'urgency'
24.	assignment_group: identifier of the support group in charge of the incident
25.	assigned_to: identifier of the user in charge of the incident
26.	knowledge: boolean attribute that shows whether a knowledge base document was used to resolve the incident
27.	u_priority_confirmation: boolean attribute that shows whether the priority field has been double-checked
28.	notify: categorical attribute that shows whether notifications were generated for the incident
29.	problem_id: identifier of the problem associated with the incident
30.	rfc: (request for change) identifier of the change request associated with the incident
31.	vendor: identifier of the vendor in charge of the incident
32.	caused_by: identifier of the RFC responsible by the incident
33.	close_code: identifier of the resolution of the incident
34.	resolved_by: identifier of the user who resolved the incident
35.	resolved_at: incident user resolution date and time (dependent variable)
36.	closed_at: incident user close date and time (dependent variable)
Objective:
•	Predict the estimated closure time of an incident (in hours or days) from its creation, based on the attributes collected from multiple incident stages. 
•	The model predicts the Time to close: The total time (in hours/days) between when an incident is opened and when it is fully closed.
Solution Approach:
1.	Data Aggregation: Incidents may have multiple entries as they transition between states. We need to aggregate these records to build useful features (e.g., time spent in each state, total reassignment count, time from opened_at to resolved_at).
2.	Feature Engineering: Transform the dataset by creating features that describe the incident's history, such as:
o	Total time spent in each state (Active, Awaiting User Info, Resolved)
o	Reassignment and reopening counts
o	Whether the SLA was met, urgency, and priority
3.	Machine Learning Models: Train various regression models to predict the time to closure. Models include:
o	Linear Regression
o	Random Forest Regressor
o	Gradient Boosting Regressor
o	XGBoost
o	Support Vector Regressor (SVR)
4.	Model Evaluation: Evaluate each model's performance using metrics like Mean Absolute Error (MAE), Root Mean Squared Error (RMSE), and R² score. The model with the best performance will be selected for deployment.
Success Criteria:
•	Low error rates (low MAE and RMSE) indicate accurate predictions of the time to close incidents.
•	High R² score implies the model captures a significant portion of the variance in the incident closure time.
•	The model should generalize well, ensuring good performance on unseen data (i.e., a small gap between training and test set performance).
Benefits:
•	Operational Efficiency: Better prediction of ticket closure times can help teams manage workloads effectively, optimize workflows and prevent bottlenecks.
•	SLA Compliance: With accurate predictions, teams can better manage deadlines and ensure that tickets are resolved within SLA timeframes.
•	Customer Satisfaction: Providing customers with more accurate resolution time estimates leads to higher satisfaction and trust in the ITSM process.

Challenges:
•	Handling multiple entries per incident: As incidents move through different states, they generate multiple records. Aggregating these records into meaningful features without losing critical information is key.
•	Complex relationships between variables: Variables like reassignment counts, reopen counts, and time spent in different states may have complex non-linear relationships with the closure time, requiring the use of advanced models like Random Forest, Gradient Boosting, and XGBoost.
Potential Extensions:
•	Real-time monitoring: Extend the model to provide real-time predictions as new updates are made to incidents.
•	Integration with ITSM platforms: Embed the model in existing ITSM platforms (e.g., ServiceNow) to provide automated predictions as part of the ticketing workflow.
In summary, this project aims to enhance the incident management process by leveraging machine learning to predict incident closure times, thereby improving efficiency, meeting SLAs, and enhancing customer experience.

