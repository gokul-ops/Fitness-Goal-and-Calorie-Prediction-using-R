# Fitness-Goal-and-Calorie-Prediction-using-R
R mini project that predicts daily calories burned and 10,000-step goal achievement from Fitbit tracker data using linear regression, Random Forest and logistic regression.
1. About the project

Regular activity is easy to track but hard to interpret. This project uses R and real Fitbit tracker data to answer two questions:

How many calories will a person burn in a day? (regression)
Will the person reach the 10,000-step daily goal? (classification)
2. Dataset
Name: Fitbit Fitness Tracker Data (daily activity and sleep logs)
Source: Kaggle (Mobius), collected via Amazon Mechanical Turk, 12 March - 12 May 2016
Size: 1,400 rows x 18 columns after merging, 35 users; 1,236 rows after cleaning
Files used (in data/raw/): dailyActivity_period1.csv, dailyActivity_period2.csv, sleepDay_period2.csv
Target	Type	Definition
Calories	Numeric	Calories burned per day (kcal)
GoalAchieved	Binary	1 if TotalSteps >= 10000, else 0 (34.8% of days)
3. Project structure
FitnessGoalPrediction/
├── R/fitness_goal_prediction.R     # complete script (Q1 to Q5)
├── data/
│   ├── raw/                        # original Fitbit CSV files
│   └── fitness_clean.csv           # cleaned data (created by the script)
├── outputs/
│   ├── figures/                    # 10 plots (PNG)
│   ├── regression_results.csv
│   ├── classification_results.csv
│   ├── new_user_predictions.csv
│   ├── rf_calorie_model.rds / glm_goal_model.rds
│   └── console_output.txt          # full output of one run
├── docs/                           # Word report and PPT
└── README.md
4. Methodology
Stage	What is done
Data structures	Vector, factor, matrix, list, data frame, Date
Control structures	if / else if, for, while, user-defined functions, sapply, apply, aggregate
Cleaning	Merge sleep log, remove 3 duplicates and 161 non-wear days, median-impute sleep, create Weekend and GoalAchieved
Statistics	Shapiro-Wilk, correlation test, t-test, chi-square, ANOVA, IQR outliers, VIF
Split	80% train (991 rows) / 20% test (245 rows), set.seed(123)
Regression models	Simple, multiple, stepwise (AIC), polynomial, Random Forest (ranger)
Classification	Logistic regression (glm) and Random Forest
Prediction	predict_fitness() returns calories and goal probability for new users

Steps and distance are not used to predict the step goal, because they define the goal (data leakage).

5. Results (test set)

Calories (regression)

Model	RMSE	R-squared	MAE
Simple Linear	603.023	0.297	490.656
Multiple Linear	550.689	0.413	455.196
Stepwise Linear	549.694	0.416	454.156
Polynomial (deg 2)	549.081	0.417	455.249
Random Forest	422.337	0.655	339.467

10,000-step goal (classification)

Model	Accuracy	F1	AUC
Logistic Regression	0.849	0.7582	0.8994
Random Forest	0.898	0.8344	0.9612

Example prediction (new users)

Profile	Predicted calories	Goal probability	Goal
Office worker	2244	0.023	Not achieved
Regular jogger	2214	0.732	Achieved
Weekend walker	2679	0.184	Not achieved
6. How to run

Requirements: R (4.x) and RStudio.

r
# 1. install packages (once)
install.packages(c("ggplot2", "corrplot", "caret", "ranger"))

# 2. set the working directory to the project root
setwd("path/to/FitnessGoalPrediction")
getwd()      # should end with FitnessGoalPrediction

# 3. run the whole project
source("R/fitness_goal_prediction.R")

In RStudio you can also use Session > Set Working Directory > Choose Directory.

Expected output (last lines):

Done. Figures in outputs/figures, tables in outputs/

The script also creates data/fitness_clean.csv and all files in outputs/.

7. Limitations and future work
Only 35 users, and each user contributes many days, so rows are not fully independent.
Weight, age, height and gender are not in the data, which limits calorie accuracy (R-squared 0.655).
Future work: mixed-effects models, gradient boosting, more users and a Shiny web app.
