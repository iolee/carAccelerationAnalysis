Physics-Based Car Acceleration Prediction
Project Overview
This project analyzes the relationship between vehicle specifications and acceleration performance using the classic Auto MPG dataset.
The goal was to build a robust, interpretable Ordinary Least Squares (OLS) regression model to predict vehicle acceleration (time to reach speed). Through iterative testing, the project demonstrates that a Pure Physics Model—relying solely on Power-to-Weight ratio and Displacement—outperforms complex models that rely on temporal or categorical features.
The Data
Source: Seaborn mpg dataset (originally from the UCI Machine Learning Repository).
Target: acceleration (Time in seconds).
Key Features: horsepower, weight, displacement.
Methodology & Evolution
The analysis followed a rigorous feature selection process:
Baseline Model: Started with raw features.
Feature Engineering: Introduced Power-to-Weight ratio and Polynomial features to account for non-linear physics (Newton's Second Law).
Hypothesis Testing (The "Malaise Era"):
We hypothesized that model_year would be significant due to 1970s emissions regulations.
We tested Quadratic terms, Piecewise Regression (1976 hinge), and SAE Gross/Net adjustments.
Result: All temporal variables proved statistically insignificant when controlled for physical specs.
Final Model: We adopted a "Parsimonious" approach, removing all year and origin data. The drop in engine performance during the 1970s was already captured by the horsepower and displacement variables themselves.
The Final Model
The finalized model uses a Log-Linear regression with the following predictors:
Power-to-Weight Ratio: The primary driver of acceleration (
F
=
m
a
F=ma
).
Power-to-Weight Squared: Captures diminishing returns at higher power levels.
Displacement: A proxy for engine torque (launch force).
Performance
Adjusted R-squared: ~0.725
Cross-Validation (10-Fold): Consistent performance (Low variance), confirming no overfitting.
Insight: The model generalizes well across different eras (1970-1982) without needing specific year-based corrections.
Installation & Usage
Clone the repository:
code
Bash
git clone https://github.com/iolee/carAccelerationAnalysis.git
Install dependencies:
code
Bash
pip install -r requirements.txt
Run the Jupyter Notebook:
code
Bash
jupyter notebook analysis.ipynb
Technologies Used
Python 3.x
Pandas & NumPy (Data Manipulation)
Statsmodels (Statistical Modeling)
Scikit-Learn (Cross-Validation & Preprocessing)
Seaborn & Matplotlib (Visualization)
Model
ThinkingThoughts
Expand to view model thoughts

chevron_right
Here is the revised README. It accurately reflects the workflow we established: Outlier analysis (decision to keep) 
→
→
 BIC Selection 
→
→
 Polynomial Expansion 
→
→
 P-value Backward Elimination.
Polynomial Regression Analysis of Car Acceleration
Project Overview
This project analyzes the relationship between vehicle specifications and acceleration performance using the classic Auto MPG dataset.
The goal was to build a robust, interpretable Ordinary Least Squares (OLS) regression model to predict vehicle acceleration (time to reach 60mph). The analysis evolves from a simple linear approach to a Polynomial Regression, demonstrating that capturing non-linear relationships and interaction effects yields a far superior fit (
R
2
≈
0.717
R 
2
 ≈0.717
) while remaining statistically parsimonious.
The Data
Source: Seaborn mpg dataset (originally from the UCI Machine Learning Repository).
Target: acceleration (Time in seconds).
Key Features: horsepower, weight, displacement.
Methodology & Evolution
The analysis followed a rigorous statistical workflow to balance model complexity with accuracy:
Outlier Analysis (IQR Method):
We performed an Interquartile Range (IQR) check to identify outliers in the acceleration data.
Decision: While statistical outliers were identified, we chose to retain all data points. We prioritized a generalizable model that accounts for unique vehicle configurations over a perfectly normal distribution of errors.
Initial Variable Selection (Minimizing BIC):
Using Stepwise Backward Elimination based on the Bayesian Information Criterion (BIC), we filtered out irrelevant features.
Variables like model_year, origin, and cylinders were dropped in favor of the three core physical drivers: displacement, horsepower, and weight.
Initial Linear 
R
2
R 
2
 
: ~0.61
Polynomial Expansion (The Physics Check):
Recognizing that acceleration follows physical laws (Newton's Second Law, 
F
=
m
a
F=ma
) rather than strict linearity, we introduced Degree-2 Polynomial features and Interaction terms.
This allowed the model to account for the curvature in engine power and how weight influences engine displacement requirements.
Final Refinement (P-Value Elimination):
We applied a second round of Backward Elimination on the polynomial features.
Non-significant terms (like horsepower*weight and weight^2) were removed.
Result: A highly efficient model where every remaining variable is statistically significant (
P
<
0.05
P<0.05
).
The Final Model
The finalized model is a Polynomial OLS Regression consisting of 5 specific terms:
Horsepower (
x
x
) & Horsepower
2
2
 
 (
x
2
x 
2
 
): Confirms that acceleration improves with power, but with diminishing returns (curvature).
Weight: A primary linear drag on acceleration.
Displacement: A proxy for engine size/torque.
Displacement 
×
×
 Weight (Interaction): A significant term indicating that the effect of engine size on acceleration is dependent on how heavy the car is.
Performance
R-squared: 0.717
F-statistic: 195.9 (Prob: ~0.00)
Model Note: We accept high multicollinearity (Condition Number) as it is structural and expected in polynomial models. We also accept the Omnibus test results due to our decision to retain outliers for broader model applicability.
Installation & Usage
Clone the repository:
code
Bash
git clone https://github.com/iolee/carAccelerationAnalysis.git
Install dependencies:
code
Bash
pip install -r requirements.txt
Run the Jupyter Notebook:
code
Bash
jupyter notebook analysis.ipynb
Technologies Used
Python 3.x
Pandas & NumPy: Data Manipulation and IQR calculation.
Statsmodels: OLS Regression, Backward Elimination, and Statistical Summaries.
Scikit-Learn: Polynomial Feature generation.
Seaborn & Matplotlib: Visualization.
