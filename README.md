# MNIST kNN vs Logistic Regression

## Overview
This project is a case study from **HarvardX PH125.8x Data Science: Machine Learning**. It compares k-Nearest Neighbors (kNN) and logistic regression for classifying a subset of the MNIST dataset, specifically distinguishing between the digits 2 and 7. The analysis explores the impact of different k values on kNN performance and contrasts it with logistic regression.

## Dataset
The dataset used is `mnist_27` from the `dslabs` package, which contains training and test sets for binary classification.

## Models Evaluated
- **k-Nearest Neighbors (kNN)**
  - `k = 5` (default): Moderate complexity, flexible decision boundaries.
  - `k = 1`: High variance, risk of overfitting.
  - `k = 401`: High bias, overly smooth decision boundaries.
- **Logistic Regression**
  - Provides linear decision boundaries with interpretability.

## Key Observations
- Lower k values in kNN result in overfitting, while higher k values oversmooth the decision boundary.
- Logistic regression provides a structured and interpretable decision boundary but may struggle with non-linearity.
- Visualization of decision boundaries helps in understanding model behavior.

## Code Structure
- **Data Loading & Visualization:** Plots test data distribution.
- **Model Training & Evaluation:** kNN models with different k values and logistic regression.
- **Visualization:** Decision boundaries for different models.

## Requirements
The following R packages are required:
```r
library(tidyverse)
library(caret)
library(dslabs)
library(gridExtra)
```
