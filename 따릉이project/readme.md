## 서울시 따릉이 대여량 예측 프로젝트 

Sep 2021 - Nov 2021

Dacon Link : https://dacon.io/competitions/official/235576/data

분석 목적 : 시간별 온도, 강수량 등의 기상 변수(설명변수)들을 통해 회귀모델을 세우고, 따릉이 대여량(타겟)을 예측

#### 1. Read data
train data : 1459 obs, 11 var    
test data : 715 obs, 10 var 

*variables*   
`id` 고유 id   
`hour` 시간  
`temperature` 기온  
`precipitation` 비가 오지 않았으면 0, 비가 오면 1  
`windspeed` 풍속(평균)  
`humidity` 습도   
`visibility` 시정(視程), 시계(視界)(특정 기상 상태에 따른 가시성을 의미)   
`ozone` 오존   
`pm10` 미세먼지(머리카락 굵기의 1/5에서 1/7 크기의 미세먼지)   
`pm2.5` 미세먼지(머리카락 굵기의 1/20에서 1/30 크기의 미세먼지)   
**`count`** 시간에 따른 따릉이 대여 수   


#### 2. EDA ; Exploratory Data Analysis 
- Visualization 
- NA : imputation technique
- Outlier detection 
- Create dummy variables

#### 3. Modeling 
- Cross Validation
- Random Forest Regressor
- Hyper parameter tuning via Grid Search     
`n_estimator` `min_samples_split` `max_depth`
- train for submission : fit & predict    
criterion = R^2 score 

