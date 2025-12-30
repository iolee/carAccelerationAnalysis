# Physics-Based Car Acceleration Prediction

## Project Overview
This project analyzes the relationship between vehicle specifications and acceleration performance using the classic Auto MPG dataset.

The goal was to build a robust, interpretable Ordinary Least Squares (OLS) regression model to predict vehicle acceleration (time to reach speed). Through iterative testing, the project demonstrates that a **Pure Physics Model**—relying solely on Power-to-Weight ratio and Displacement—outperforms complex models that rely on temporal or categorical features.

## The Data
- **Source:** Seaborn `mpg` dataset (originally from the UCI Machine Learning Repository).
- **Target:** `acceleration` (Time in seconds).
- **Key Features:** `horsepower`, `weight`, `displacement`.

## Methodology & Evolution
The analysis followed a rigorous feature selection process:

1.  **Baseline Model:** Started with raw features.
2.  **Feature Engineering:** Introduced `Power-to-Weight` ratio and Polynomial features to account for non-linear physics (Newton's Second Law).
3.  **Hypothesis Testing (The "Malaise Era"):**
    - We hypothesized that `model_year` would be significant due to 1970s emissions regulations.
    - We tested Quadratic terms, Piecewise Regression (1976 hinge), and SAE Gross/Net adjustments.
    - **Result:** All temporal variables proved statistically insignificant when controlled for physical specs.
4.  **Final Model:** We adopted a "Parsimonious" approach, removing all year and origin data. The drop in engine performance during the 1970s was already captured by the `horsepower` and `displacement` variables themselves.

## The Final Model
The finalized model uses a Log-Linear regression with the following predictors:
1.  **Power-to-Weight Ratio:** The primary driver of acceleration ($F=ma$).
2.  **Power-to-Weight Squared:** Captures diminishing returns at higher power levels.
3.  **Displacement:** A proxy for engine torque (launch force).

### Performance
- **Adjusted R-squared:** ~0.725
- **Cross-Validation (10-Fold):** Consistent performance (Low variance), confirming no overfitting.
- **Insight:** The model generalizes well across different eras (1970-1982) without needing specific year-based corrections.

## Installation & Usage

1. Clone the repository:
   ```bash
   git clone https://github.com/YOUR_USERNAME/car-acceleration-physics.git