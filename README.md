# Credit Card Payment Default Prediction

This project focuses on the analysis and modeling of credit card payment default risk. Using the "Default of Credit Card Clients" dataset from UCI, we performed an exploratory analysis, data cleaning, and finally, built a **scorecard model** based on **Weight of Evidence (WoE)** and **Logistic Regression** methodologies.

The objective is to generate a risk score for each client, enabling a financial institution to make informed credit management decisions.

### Project Methodology

The workflow followed in the notebook can be summarized in the following steps:

#### Exploratory Data Analysis (EDA) & Data Cleaning:
* We performed an initial analysis to identify variable distributions, null values, and data types.
* Inconsistencies in the categorical variables were corrected by grouping unknown or undocumented categories to improve model interpretability:
    * **EDUCATION:** Categories 0, 5, and 6 (unknown) were grouped into category 4 ('Others').
    * **MARRIAGE:** Category 0 (undocumented) was reassigned to category 3 ('Others').
    * **PAY_X:** Values -1 and -2 (on-time payment) were grouped into category 0 to unify the concept of "timely payment."

#### Variable Analysis & Feature Selection:
* We used a **Chi-square test** to evaluate the independence between the categorical variables (EDUCATION, MARRIAGE, SEX, PAY_X) and the target variable. All showed a statistically significant relationship.
* **ANOVA (f_oneway)** was employed to analyze the relationship between key numerical variables (LIMIT_BAL, AGE) and the target variable, confirming significant differences between groups.
* The **Weight of Evidence (WoE) transformation** was applied, and the **Information Value (IV)** was calculated for all variables. Those with an IV greater than 0.02 were selected for the final model, resulting in 15 of the 22 predictor variables being chosen.

#### Modeling & Scorecard Generation:
* The data was split into training (75%) and test (25%) sets.
* A **Logistic Regression** model was trained using the WoE-transformed variables.
* Finally, a **scorecard** was built that assigns points to each client characteristic. The sum of these points generates a final score that estimates the probability of default.

### Key Results

The model was evaluated on the test set, yielding the following results:

* **AUC (Area Under the Curve):** 0.77, which indicates a good ability to **discriminate** between good and bad clients.
* **Gini Index:** 0.54, confirming an acceptable **predictive power**.
* **Kolmogorov-Smirnov (KS) Statistic:** 40.76%, suggesting a moderate discriminatory power. 
* **Classification Metrics** (with a cutoff point of 0.22):
    * **Sensitivity (Recall):** 58% - The model correctly identifies 58% of the clients who will actually default.
    * **Specificity:** 82% - The model correctly identifies 82% of the clients who will not default.
    * **Positive Precision:** 49% - Of the clients the model predicts will default, 49% actually do.
* **Cutoff Score:** A cutoff score of 447 points was established. Clients with a score below this threshold are considered higher risk.

### Conclusions and Limitations

* **Conclusion:** The model is robust at identifying low-risk clients (high specificity), which is useful for avoiding the rejection of potentially good clients. However, its ability to detect clients who will default (sensitivity) is moderate, which could imply a financial risk for the institution if not supplemented with other strategies.
* **Limitation:** The model relies on historical payment variables (**PAY_X**), so its application is limited to existing clients. It is not suitable for evaluating the risk of new applicants without a credit history with the institution.
