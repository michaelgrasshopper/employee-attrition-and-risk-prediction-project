# Employee-attrition-and-risk-prediction-project

# The 30-second version

This project uses data on 1,470 employees to understand why people leave a company, predict who is most likely to leave, group employees into types, and show the results in an interactive Power BI dashboard. The goal is to help HR teams spot risk early and focus retention efforts where they matter most.

# The problem

Losing employees is expensive, but companies usually find out only after someone resigns. In the IBM HR Analytics dataset, 237 of 1,470 employees (16.1%) left. Because far fewer people leave than stay, the problem is hard for a machine-learning model. A model that always predicts “stays” would be right 84% of the time and still be useless.

# What I did, step by step

1. Exploring the data. I compared employees who left with those who stayed.

Overtime: about 1 in 3 overtime workers left, versus about 1 in 10 of those without overtime. It was the strongest single signal.
Pay: leavers had a lower median monthly income (about $3,200 vs $5,200).
Tenure: leavers had a shorter median time at the company (about 3 years vs 6).
Age: attrition was concentrated among younger employees.

2. Preparing the features. Categories like job role and department were one-hot encoded, and numbers were scaled so that no feature dominates just because of its units.

3. Predicting who leaves. I trained seven models: logistic regression, KNN, decision tree, SVM, naive Bayes, random forest and gradient boosting. I judged them on precision, recall, F1 and ROC-AUC instead of accuracy alone. The first results showed the imbalance problem clearly, because most models caught only a small share of the real leavers.

4. Fixing the imbalance. I compared three approaches: leaving the data as it was, weighting the rare “left” class more heavily, and SMOTE, which creates synthetic examples of leavers during training. For logistic regression, SMOTE roughly doubled recall (0.38 to 0.79) at the cost of more false alarms.

5. Testing reliably. I used 5-fold stratified cross-validation, so the results don’t depend on one lucky split. I then tuned the four most promising models with grid search. The winner was an SVM, with a cross-validated F1 of 0.535. On the unseen test set it caught 64% of leavers with an F1 of 0.53 and a ROC-AUC of 0.79.

6. Predicting income. As a second task, I predicted monthly income with linear regression, a decision tree and a random forest. The random forest was best (R² 0.945, average error about $820). The score is high mainly because job level almost determines pay, so I treat this as a supporting result.

7. Finding employee groups. Without telling the algorithm who left, I clustered employees with K-Means and hierarchical clustering, and used PCA to draw them in 2D. K-Means gave four groups:

Early-career staff: 714 people, average age 34, income about $4,300, and 21% attrition.
A similar group on slightly higher pay: 195 people, 19% attrition.
Mid-career staff: 339 people, about 10 years at the company, and 10% attrition.
Senior staff: 222 people, average age 48, income about $15,400, and only 8% attrition.

The cluster structure is weak overall, so I use it as a descriptive guide, not a hard classification.

8. Scoring every employee. The final model gives each employee a probability of leaving, sorted into Low (under 30%), Medium (30–60%) or High (over 60%). To keep the scores honest, each employee is scored by a model that never saw them during training. The ranking works: actual attrition was 6.3% in Low, 17.5% in Medium and 48.5% in High. These are risk estimates, not predictions that a particular person will leave.

9. The Power BI dashboard. Five pages present the results: Workforce Overview, Attrition Analysis, Employee Risk, Model Performance and Employee Segmentation. The pages are filterable, so a manager can look at one department or job role.

# What the project shows

Attrition is concentrated among younger, lower-paid, shorter-tenure employees who work overtime. A model can rank employees by risk well enough to be useful. Handling class imbalance changes which employees the model catches, and a high accuracy score would have hidden that.

# Honest limitations

The dataset is fictional and a single snapshot, the test set contains only 47 leavers, and the model’s precision is moderate (more than half of the people it flags don’t leave). The risk scores should guide conversations and retention planning, not individual employment decisions.
