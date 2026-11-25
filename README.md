# Student-dropout-analysis

Course: Data Visualization & Preprocessing using Python (CSE0315) — PBL
Student: Urugonda Varshitha (BETN1DS25015) | Guide: Dr. Nidhi Dandotiya
SDG: 4 — Quality Education

1. Problem Statement

Getting students enrolled is not the same as keeping them in education. Dropout is not one uniform problem: it is shaped by several overlapping factors such as academic performance, attendance, stress, family background and study habits. Without breaking the data down, it is easy to treat dropout as a single generic issue when the drivers differ between student groups.

This project analyses student-level data to find which students and which factors are most associated with dropout, using both traditional statistical methods and AI-assisted tools. The end goal is an interactive dashboard that helps an educator spot high-risk students at a glance.

2. Objectives
Clean and preprocess the dataset, with documented handling of missing values and outliers
Find the factors most strongly associated with dropout (attendance, GPA, stress, gender, department, year of study)
Compare traditional analysis with AI-assisted equivalents and critically evaluate the AI output
Build an interactive Power BI dashboard that highlights high-risk student groups
3. Dataset
Property	Detail
File	student_dropout_dataset_v3.csv
Size	10,000 rows × 19 columns
Target	Dropout (0 = stayed, 1 = dropped out)
Numeric	Age, Family_Income, Study_Hours_per_Day, Attendance_Rate, Assignment_Delay_Days, Travel_Time_Minutes, Stress_Index, GPA, Semester_GPA, CGPA
Categorical	Gender, Internet_Access, Part_Time_Job, Scholarship, Year_of_Study, Department, Parental_Education
Missing values	Family_Income (500), Study_Hours_per_Day (500), Stress_Index (500), Parental_Education (511)
Duplicates	None
Preprocessing steps
Missing numeric values filled with the median; Parental_Education filled with the mode (Bachelor)
Semester renamed to Year_of_Study (values are Year 1–4)
Outliers checked with the IQR method; Family_Income capped at the upper bound (683 values)
Isolation Forest compared against IQR (500 vs 414 flagged, 89 flagged by both)
Encoding: binary mapping, ordinal encoding (Parental_Education, Year_of_Study) and one-hot encoding (Department)
Numeric columns standardised with StandardScaler
New feature Attendance_Level (Low / Medium / High)
4. Tools Used
Tool	Purpose
Python (Google Colab)	Main analysis environment
Pandas, NumPy	Cleaning and aggregation
Matplotlib, Seaborn	Visualisation
Scikit-learn	Isolation Forest (AI outlier detection), StandardScaler
Statsmodels	One-way ANOVA
Sweetviz	AI-generated automated EDA report
AutoViz	Automated chart selection (planned)
Power BI	Interactive dashboard, including the Key Influencers AI visual
5. Dashboard

An interactive Power BI report (dashboard/pbl.powerbi.pbix) built on the cleaned dataset. It contains:

Dropout rate by attendance level
Dropout rate by year of study, split by department
GPA by dropout status
GPA vs. stress index scatter plot with trend line
Key Influencers AI visual (target: Dropped out)
KPI cards (see below)
6. KPI Cards

The dashboard's card visual summarises the headline numbers:

KPI	Value
Total students	10,000
Dropout rate	≈ 23%
Average attendance rate	≈ 81.7%
Average GPA	≈ 2.3
7. Visualisation

<img width="618" height="344" alt="Screenshot 2026-11-25 190858" src="https://github.com/user-attachments/assets/04fda45b-b8e8-4ba4-8029-2d4d0624ed55" />


Power BI Dashboard

Interactive Power BI dashboard: dropout by attendance level, year of study and department, GPA vs. stress, and Key Influencers.

Python Charts

Charts built in Python (Matplotlib) and exported to images/:

Pie chart: share of students who dropped out
Bar charts: dropout rate by department and by gender
Line chart: dropout rate from Year 1 to Year 4
Histograms: distributions of GPA, attendance rate and stress index
8. Key Findings
About 23% of students dropped out
Dropout is almost the same across gender (23.8% female, 23.2% male) and department (23.0%–24.0%), so these are weak factors
Dropped-out students have a much lower average GPA (1.43 vs 2.58) and higher stress (6.28 vs 5.27)
Students with low attendance and GPA below 1.5 had a 63.2% dropout rate (666 students)
Attendance level is significantly associated with GPA (ANOVA: F ≈ 195.8, p < 0.001); average GPA rises from 2.00 (low attendance) to 2.56 (high)
Stress_Index has a correlation of about 0.26 with dropout
Isolation Forest and IQR flagged largely different rows, so the two methods should not be treated as interchangeable
9. Conclusion

Academic performance (GPA), attendance and stress are the clearest indicators of dropout in this data, while gender and department make little difference. Students who combine low attendance with low GPA form a clear high-risk group and are the natural target for early intervention.

AI-assisted tools were useful for fast first reads (Sweetviz) and for surfacing outliers that the IQR method missed (Isolation Forest), but their output needed checking. Isolation Forest with contamination=0.05 always flags exactly 5% of rows, and GPA, Semester_GPA and CGPA overlap heavily and may leak information about dropout. The dataset is student-level, so it supports no state-wise or time-based forecasting.

10. Project Files
├── data/
│   ├── student_dropout_dataset_v3.csv          # raw dataset
│   ├── cleaned_student_dropout.csv             # cleaned dataset
│   └── cleaned_student_dropout_scaled.csv      # encoded + standardised
├── notebooks/
│   └── PBL.ipynb                               # Google Colab notebook
├── reports/
│   └── sweetviz_report.html                    # automated EDA report
├── dashboard/
│   └── pbl.powerbi.pbix                        # Power BI dashboard
├── images/                                     # exported charts
├── proposal/
│   └── pbl_proposal.docx
└── README.md
