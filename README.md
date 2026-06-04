# Stochastic Interest Rate Modelling and Prediction

![Python Version](https://img.shields.io/badge/Python-3.8%2B-blue)
![Dependencies](https://img.shields.io/badge/Dependencies-SciPy%20%7C%20NumPy%20%7C%20Pandas-orange)
![Optimization](https://img.shields.io/badge/Optimization-L--BFGS--B-success)

**Author:** Mamun Chowdhury  
**Institution:** Indian Institute of Technology (IIT) Roorkee  

---

## Project Overview

This repository contains a complete, end-to-end quantitative finance framework for modeling, calibrating, and forecasting interest rate dynamics using stochastic differential equations (SDEs). The project progresses from raw yield curve data engineering to the deployment of continuous diffusion models, ultimately extending into complex discontinuous jump-diffusion frameworks to model severe market shocks. 

## Core Concepts & Terminology 

Before diving into the code, it is helpful to understand the theoretical foundations driving the models in this repository.

* **Short Rate ($r_t$):** The annualized interest rate at which an entity can borrow money for an infinitesimally short period of time. It acts as the foundational building block for the entire yield curve.
* **Cox-Ingersoll-Ross (CIR) Model:** A mathematical formula used to project interest rates. Unlike simpler models (like Vasicek), CIR ensures that interest rates are always strictly positive, which is more realistic for standard economic environments.
* **Mean Reversion:** The financial theory that interest rates will naturally tend to move back towards their long-term historical average ($\theta$) over time. If rates spike, gravity pulls them down; if they crash, gravity pulls them up. The speed of this pull is dictated by the parameter $\kappa$.
* **Feller Condition ($2\kappa\theta \geq \sigma^2$):** A strict mathematical constraint enforced during model calibration. It ensures that the upward drift of the interest rate (driven by mean reversion) is always strong enough to overcome the downward pull of market volatility, mathematically guaranteeing the rate never drops below zero.
* **Maximum Likelihood Estimation (MLE):** A rigorous statistical method used to find the optimal parameters for the model. It works by calculating the specific set of parameters ($\kappa$, $\theta$, $\sigma$) that make the historically observed yield curve data the most statistically probable.
* **Jump-Diffusion:** An extension of the standard model. Standard diffusion assumes rates move in continuous, smooth paths. A "jump" process (driven by a Poisson distribution) allows the model to simulate sudden, violent, and discontinuous rate shocks—such as unexpected central bank hikes or geopolitical crises.

---

## Pipeline Architecture

The project is structured sequentially into four primary modules within the main Jupyter Notebook (`project-2.ipynb`).

### Section A: Data Engineering & Preprocessing
* Ingests high-frequency, multi-maturity yield curve data.
* Systematically identifies and interpolates anomalies using Interquartile Range (IQR) filtering.
* Validates dataset integrity to ensure absolute positivity, a prerequisite for the CIR model.

### Section B: CIR Model Implementation & Calibration
* Defines the base continuous stochastic process: $dr_t = \kappa(\theta - r_t)dt + \sigma\sqrt{r_t}dW_t$
* Calibrates the model using **Maximum Likelihood Estimation (MLE)** as the primary engine to capture non-central chi-squared transition densities.
* Implements bounded optimization algorithms (L-BFGS-B) to strictly enforce the Feller condition.
* Features an Ordinary Least Squares (OLS) calibration fallback mechanism to ensure pipeline stability if the primary optimizer fails to converge.

### Section C: Yield Curve Prediction & Validation
* Projects future yield curve states based on the calibrated SDE parameters.
* Deploys out-of-sample forecasting and backtesting.
* Validates model efficacy using rigorous statistical metrics, including $R^2$ and Root Mean Square Error (RMSE).

### Section D: Jump-Diffusion Extension
* Augments the base model to account for market incompleteness and fat-tailed risk distributions: $dr_t = \kappa(\theta - r_t)dt + \sigma\sqrt{r_t}dW_t + J_t dN_t$
* Simulates systemic macro-level shocks that cannot be captured by constant-volatility continuous models.
* Analyzes the immediate gravitational pull ("reversion hump") triggered by violent rate dislocations.

---

## Installation & Setup

1. **Clone the repository:**
   ```bash
   git clone [https://github.com/yourusername/stochastic-rate-modeling.git](https://github.com/yourusername/stochastic-rate-modeling.git)
   cd stochastic-rate-modeling
