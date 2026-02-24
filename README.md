# Hospital_Readmission_Risk_Analysis
<p align="center">
  <img src="Hospital_Dashbaord.png" width="800">
</p>

Business Problem

Hospitals face severe financial penalties from Medicare when patients are readmitted to the hospital within 30 days of being discharged. Furthermore, early readmission is a sign that the patient did not receive the right follow-up care. Administrators currently lack a predictive tool to identify which patients are at the highest risk before they leave the hospital.

Project Objective

My goal for this project was to analyze historical clinical data and build a Machine Learning model that predicts 30-day hospital readmissions. By identifying high-risk patients, hospitals can proactively allocate resources (like special care nurses or follow-up calls) to prevent readmissions and save millions in penalties.

🛠️ Technology Stack

Language: Python

Data Manipulation & Cleaning: Pandas, NumPy

Machine Learning: Scikit-Learn (Random Forest)

Visualization: Matplotlib, Seaborn, Power BI

Project Management: Jira, Confluence

📊 The Dataset

I used the publicly available Diabetes 130-US Hospitals Dataset (1999–2008), which contains over 100,000 clinical records. It includes patient demographics, hospital visit history, medications, lab procedures, and whether they were readmitted within 30 days.
Kaggle Dataset Link: Diabetes 130-US Hospitals Dataset (1999-2008)

Dataset Description:

### **Data Dictionary**

| Column Name | Description | Business Significance |
| :--- | :--- | :--- |
| **readmitted_binary** | **Target:** 1 = Readmitted within 30 days; 0 = No/Late readmission. | Primary outcome to predict to avoid Medicare penalties. |
| **age** | Patient age grouped in 10-year brackets (e.g., [70-80)). | Identifies high-risk demographic segments. |
| **time_in_hospital** | Total days between admission and discharge. | Longer stays correlate with higher patient complexity. |
| **num_lab_procedures** | Number of lab tests performed. | Indicates intensity of diagnostic effort. |
| **num_medications** | Number of distinct generic medications administered. | High counts increase risk of drug interactions. |
| **number_inpatient** | Inpatient visits in the preceding year. | Identifies "Frequent Flyers" (historically high-risk). |
| **number_emergency** | Emergency visits in the preceding year. | Signals instability or lack of outpatient support. |
| **diabetesMed** | If any diabetic medication was prescribed (Yes/No). | Links readmission to chronic disease management. |
| **total_hospital_activity** | **Engineered:** Sum of all past visits. | Consolidated "risk score" of patient history. |


🧠 Methodology (Step-by-Step)

1. Data Cleaning
   
Real-world data is messy. I started by preparing the dataset for analysis:

Identified missing values (which were recorded as ? instead of blanks) and converted them into standard empty values (NaN).

Dropped columns that were missing too much data to be useful (like weight and payer_code).

Created a clean "Target Variable" called readmitted_binary (1 = Readmitted in under 30 days, 0 = Not readmitted).

2. Exploratory Data Analysis (EDA)
   
Before building AI, I visualized the data to find human-readable clues.

Insight Discovered: I found a strong "Frequent Flyer" trend. Patients with a high number of previous hospital visits in the past year were more than twice as likely to be readmitted.

3. Feature Engineering

Feature Engineering means creating "smarter clues" out of existing data to help the AI learn better.

I combined previous inpatient, outpatient, and emergency visits into a single new metric called total_hospital_activity to better capture how sick a patient chronically is.

4. Machine Learning Model Used
   
I used a Random Forest Classifier to predict readmissions.

What is a Random Forest? (In Simple Terms)
Imagine asking 100 different doctors to look at a patient's chart and guess if they will return to the hospital. Each doctor looks at slightly different clues (some look at age, some look at medications). Then, all 100 doctors cast a vote. The majority vote wins.
A Random Forest does exactly this using math. It builds hundreds of tiny "decision trees" and combines their votes to make a highly accurate prediction.

Random Forest:

Random Forests are excellent for healthcare data because they handle complex, non-linear relationships well (e.g., being old doesn't necessarily mean you will return, but being old and taking 5 medications and having previous visits does).

5.Model Tuning & The "Accuracy Paradox"
 
At first, the model achieved 85% Overall Accuracy. However, because 89% of patients do not return, the AI was "cheating" by simply guessing "No" for almost everyone.

To fix this, I tuned the model using class_weight='balanced'.

The Trade-off: This lowered the overall accuracy to ~68%, but it drastically increased the Recall for high-risk patients.

Business Logic:

In healthcare, "Recall" (successfully catching the actual sick patients) is much more important than overall accuracy. The cost of a "false alarm" (checking on a healthy patient) is very low, but the cost of a "false negative" (missing a sick patient who then returns) is massive.

6.Power BI Dashboard Integration
   
Finally, I exported the AI's predictions and built an interactive dashboard for hospital management. The dashboard includes:

KPI Cards: Displaying total patients and the exact number of predicted high-risk patients.

Bar Charts: Visualizing readmission risk across different age brackets.

Donut Charts: Proving the "Frequent Flyer" trend by showing average past visits mapped to risk levels.

Interactive Slicers: Allowing administrators to filter the entire dashboard by specific medications.

💡 Business Impact

By deploying this predictive model and dashboard, hospital administrators can now:

Target Interventions: Instantly see a list of high-risk patients currently in the hospital.

Optimize Resources: Assign follow-up care (like phone calls or home visits) only to those who truly need it.

Reduce Costs: Meaningfully lower the 30-day readmission rate, avoiding heavy government financial penalties and improving overall patient health.

🚀 How to Run This Project
Download the diabetic_data.csv file.

Run the Hospital_Readmission_Analysis.ipynb Jupyter Notebook to clean the data and train the model.

Open the .pbix file in Power BI Desktop to interact with the dashboard.

## 🏁 Project Conclusion & Key Findings

After completing the analysis and predictive modeling, several key insights were uncovered:

1. **The "Frequent Flyer" Effect:** The number of previous inpatient visits is the strongest predictor of future readmission. Patients with higher "Total Hospital Activity" scores require specialized discharge planning to break the cycle of readmission.
2. **Age-Related Risk:** Patients in the 70-90 age bracket showed a significantly higher volume of high-risk flags, suggesting that geriatric-specific follow-up care could yield the highest return on investment for the hospital.
3. **Model Performance:** While the model achieved an overall accuracy of 68.31%, it was optimized for **Recall**. This ensures the hospital identifies as many high-risk patients as possible, prioritizing patient health and penalty avoidance over simple "correct guesses" on healthy patients.

### **Future Improvements**
If given more time and data, the project could be expanded by:
* Integrating **social determinants of health** (like zip code or income levels) to see if external factors affect recovery.
* Testing more advanced algorithms like **XGBoost** to see if the predictive power can be increased further without losing recall.
* Automating the Power BI dashboard to refresh daily with real-time patient discharge data.
