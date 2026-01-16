# Polynomial Regression Analysis of Car Acceleration

## Project Overview
This project analyzes the relationship between vehicle specifications and acceleration performance using the classic Auto MPG dataset.

The goal was to build a robust, interpretable Ordinary Least Squares (OLS) regression model to predict vehicle acceleration (time to reach 60mph). The analysis evolves from a simple linear approach to a **Polynomial Regression**, demonstrating that capturing non-linear relationships and interaction effects yields a far superior fit ($R^2 \approx 0.717$) while remaining statistically parsimonious.

## The Data
- **Source:** Seaborn `mpg` dataset (originally from the UCI Machine Learning Repository).
- **Target:** `acceleration` (Time in seconds).
- **Key Features:** `horsepower`, `weight`, `displacement`.

## Methodology & Evolution
The analysis followed a rigorous statistical workflow to balance model complexity with accuracy:

1.  **Outlier Analysis (IQR Method):**
    - We performed an Interquartile Range (IQR) check to identify outliers in the `acceleration` data.
    - **Decision:** While statistical outliers were identified, we chose to **retain all data points**. We prioritized a generalizable model that accounts for unique vehicle configurations over a perfectly normal distribution of errors.

2.  **Initial Variable Selection (Minimizing BIC):**
    - Using **Stepwise Backward Elimination** based on the Bayesian Information Criterion (BIC), we filtered out irrelevant features.
    - Variables like `model_year`, `origin`, and `cylinders` were dropped in favor of the three core physical drivers: `displacement`, `horsepower`, and `weight`.
    - *Initial Linear R-squared: ~0.61*

3.  **Polynomial Expansion (The Physics Check):**
    - Recognizing that acceleration follows physical laws (Newton's Second Law, $F=ma$) rather than strict linearity, we introduced **Degree-2 Polynomial features** and **Interaction terms**.
    - This allowed the model to account for the curvature in engine power and how weight influences engine displacement requirements.

4.  **Final Refinement (P-Value Elimination):**
    - We applied a second round of Backward Elimination on the polynomial features.
    - Non-significant terms (like `horsepower*weight` and `weight^2`) were removed to prevent overfitting.
    - **Result:** A highly efficient model where every remaining variable is statistically significant ($P < 0.05$).

## The Final Model
The finalized model is a **Polynomial OLS Regression** consisting of 5 specific terms:

1.  **Horsepower ($x$) & Horsepower$^2$ ($x^2$):** Confirms that acceleration improves with power, but with diminishing returns (curvature).
2.  **Weight:** A primary linear drag on acceleration.
3.  **Displacement:** A proxy for engine size/torque.
4.  **Displacement $\times$ Weight (Interaction):** A significant term indicating that the effect of engine size on acceleration is dependent on how heavy the car is.

### Performance
- **R-squared:** 0.717
- **F-statistic:** 195.9 (Prob: ~0.00)
- **Model Note:** We accept high multicollinearity (Condition Number) as it is structural and expected in polynomial models. We also accept the Omnibus test results due to our decision to retain outliers for broader model applicability.

## Installation & Usage

1. Clone the repository:
   ```bash
   git clone https://github.com/iolee/carAccelerationAnalysis.git
2. Install dependencies:
   ```bash
   pip install -r requirements.txt

3. Run the Jupyter Notebook:
   ```bash
   jupyter notebook analysis.ipynb

Technologies Used
Python 3.x
Pandas & NumPy: Data Manipulation and IQR calculation.
Statsmodels: OLS Regression, Backward Elimination, and Statistical Summaries.
Scikit-Learn: Polynomial Feature generation.
Seaborn & Matplotlib: Visualization.
