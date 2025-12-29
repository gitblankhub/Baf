

# 2022 Statistical Data Analysis and Utilization Competition

**Project Period:** June 2022 - July 2022

This project participated in the '2022 Statistical Data Analysis and Utilization Competition,' utilizing public microdata (MDIS) to analyze environmental factors and meteorological information influencing large-scale wildfire risk in Korea.


## Large-Scale Wildfire Risk Environment Analysis using Meteorological Information

### 1. Data Collection

To analyze wildfire risk, data was collected from multiple sources:

* **Annual Wildfire Damage Records** (예년산불피해대장): Korea Forest Service, 2012~2021.
* **Meteorological Observation Statistics** (Ground, Hourly) (기상관측통계(지상, 시간)): MDIS, Korea Meteorological Administration (KMA), 2014~2020.
* **Synoptic Weather Observation (ASOS)** (종관기상관측): KMA Weather Data Open Portal, 2012, 2013, 2021.
* **Disaster Prevention Weather Observation (AWS)** (방재기상관측): KMA Weather Data Open Portal, 2012~2021.



### 2. EDA 

#### Data Cleaning

* **Variable Selection:**
    * **Meteorological Data Variables**: Year, Month, Day, Hour, Location_City/County, Temperature, Precipitation, Wind Speed, Wind Direction, Humidity, Vapor Pressure, Dew Point Temperature, Sea Level Pressure, Local Pressure, Sunshine Duration, Ground Temperature.
    * **Wildfire Data Variables**: Fire Occurrence Date (Year, Month, Day, Hour), Fire Extinction Date (Year, Month, Day, Hour), Fire Occurrence Location_City/County, Cause of Fire, Total Damaged Area.

* **Wildfire Data Error Handling:** Duplicates in wildfire data were identified and removed.

* **Meteorological Data Error & Missing Value Handling:**
    * Errors and missing values in meteorological data were addressed.
    * Missing values were imputed using nearby values based on time.

A unified dataset was created by merging wildfire damage information with meteorological data from 2012 to 2021, based on Year, Month, Hour, and Occurrence Location (City/County).

#### Feature Engineering 

Create derived variables which can help model to improve performance 

* **`산불발생일시` (Fire Occurrence Datetime) & `진화종료일시` (Extinction End Datetime) variables**: Combined Year + Month + Day + Hour into datetime objects.
* **`진화시간` (Extinction Time)**: Calculated the duration (time elapsed) from fire occurrence to extinction.
* **`대형산불` (Large-Scale Wildfire)**: A binary target variable created for fires with a damaged area of 30 hectares or more.

#### Normalization & Standardization

Selected variables underwent log or square root transformations, followed by standardization (e.g., Min-Max scaling or Z-score normalization) to normalize their distributions and scale them appropriately for modeling.



### 3. Unsupervised Learning: PCA & Clustering

#### Principal Component Analysis (PCA)

* Identified the presence of correlations among continuous meteorological variables.
* Performed PCA to reduce the dimensionality of these correlated meteorological features.

#### Clustering Analysis

Applied K-Means clustering based on the results of PCA (specifically `pc1` and `pc2`) to identify natural groupings within the data.



### 4. Classification Model

A classification model was developed to predict the likelihood of a `대형산불` (Large-Scale Wildfire) being the target variable.

* 7:3 Cross-Validation: Data was split into 70% training and 30% validation sets.
* SMOTE (Synthetic Minority Over-sampling Technique): Applied oversampling to the minority class (`대형산불`) to address class imbalance issues and improve model performance for rare events.

#### Model Comparison:

| Model                | PCA Variables Excluded (Accuracy, F1-score) | PCA Variables Included (Accuracy, F1-score) |
| :------------------- | :------------------------------------------ | :------------------------------------------ |
| Random Forest        | Acc=0.97, F1=0.22                           | Acc=0.93, F1=0.12                           |
| Light GBM            | Acc=0.94, F1=0.14                           | **Acc=0.93, F1=0.11** |
| Logistic Regression  | Acc=0.84, F1=0.25                           | Acc=0.90, F1=0.27                           |
| KNN                  | Acc=0.97, F1=0.49                           | Acc=0.97, F1=0.49                           |

**Feature Importance:** The feature importance results from the Random Forest model were examined to identify key variables influencing large-scale wildfire prediction.



### 5. Conclusion

Based on our analysis, the following variables were found to be important for classifying large-scale wildfires:

* **Calm and Humid Conditions:** Meteorological conditions with low wind and high humidity.
* **Month and Time of Occurrence:** Specific months and hours of the day.
* **Geographical Location:** Whether the fire occurred in Gangneung, Chuncheon, or Samcheok regions.
* **Sunshine Duration:** The amount of daylight.

It is suggested that wildfire surveillance areas should be selected by carefully considering meteorological observations, specifically wind speed, humidity, and sunshine duration, based on the observation date and time. Particular attention should be paid to and responses prepared for weather changes observed in Gangneung, Chuncheon, and Samcheok areas.



### 6. Drawbacks & Limitations

* **Clustering Significance:** The clustering analysis (PCA and K-Means) did not yield significantly clear or actionable conclusions regarding wildfire risk.
* **Limited Data Span:** The analysis was restricted to 10 years of weather information (2012-2021). A longer-term dataset, or the inclusion of additional geographical and firefighting-specific information, would likely improve the robustness and accuracy of the analysis.






