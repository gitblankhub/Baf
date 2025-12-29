
# Seoul Bike (따릉이) Rental Prediction Project

**Project Period:** September 2021 - November 2021

This project aims to predict the hourly rental demand for Seoul's public bicycle sharing service. We developed a regression model using various meteorological variables as predictors to forecast the `count` of rentals.

**Dacon Competition Link:** [https://dacon.io/competitions/official/235576/data](https://dacon.io/competitions/official/235576/data)


## 1. Data Overview

The dataset consists of hourly records, including weather conditions and corresponding 따릉이 rental counts.

* `train.csv`: 1,459 observations, 11 variables
* `test.csv`: 715 observations, 10 variables (lacks the target variable `count`)

### Variables:

* `id`: Unique identifier for each record
* `hour`: Time of the day (0-23)
* `temperature`: Air temperature (°C)
* `precipitation`: Binary indicator (0 for no rain, 1 for rain)
* `windspeed`: Average wind speed (m/s)
* `humidity`: Relative humidity (%)
* `visibility`: Visibility (m) – indicates atmospheric clarity
* `ozone`: Ozone concentration (ppm)
* `pm10`: PM10 (particulate matter less than 10 micrometers in diameter) concentration (µg/m³)
* `pm2.5`: PM2.5 (particulate matter less than 2.5 micrometers in diameter) concentration (µg/m³)
* `count`: **Target Variable** - Number of Ttareungyi rentals (available in `train.csv` only)


## 2. Exploratory Data Analysis (EDA)

The EDA phase involved gaining insights into the dataset's structure, distributions, and relationships between variables.

* **Visualization:** Various plots (e.g., histograms, scatter plots, box plots) were used to understand the distribution of each variable and their correlation with the target variable (`count`).
* **Missing Value Imputation:** Identified and handled missing values using appropriate imputation techniques to ensure data completeness for modeling.
* **Outlier Detection:** Detected and addressed outliers that could adversely affect model performance.
* **Dummy Variable Creation:** Converted categorical variables (if any, e.g., `precipitation` if treated as categorical for certain models) into dummy variables.



## 3. Modeling

Our primary goal was to build a robust regression model to predict `count`.

* **Cross-Validation:** Employed cross-validation techniques to ensure the model's generalization ability and mitigate overfitting.
* **Random Forest Regressor:** Select Random Forest Regressor as the core prediction model due to its robustness and strong performance in handling various data types and capturing non-linear relationships.
* **Hyperparameter Tuning:** Optimized the model's performance by tuning key hyperparameters using Grid Search. The hyperparameters tuned included:
    * `n_estimators`: The number of trees in the forest.
    * `min_samples_split`: The minimum number of samples required to split an internal node.
    * `max_depth`: The maximum depth of the tree.
* **Training for Submission:** The final model was trained on the processed training data (`train.csv`) and used to generate predictions on the test data (`test.csv`) for submission to the Dacon competition.
* **Evaluation Criterion:** The model's performance was evaluated using the *RMSE score*. 
