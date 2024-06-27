# 운동 동작 분류 AI

MAR 2022 - MAY 2022       
DACON link : https://dacon.io/competitions/official/235689/overview/description  

분석 목적 : 3축 가속도계(accelerometer)와 3축 자이로스코프(gyroscope)를 활용해 측정된 센서 데이터를 이용해 운동 동작 인식 알고리즘 개발 

3125개의 ID별로 0~599 time 마다 가속도, 자이로스코프 측정    
time 간격은 0.02초 즉, 한 ID별로 12초동안 운동 측정     
총 61가지 운동 동작 분류     

## 0. Read Data 
`train_features.csv`
: id별 600 time간 동작 데이터. 
- 3125 ids x 600 time = 1875000 obs    
- 8 vars (id, time, acc_x, acc_y, acc_z, gy_x gy_y gy_z) 
`train_labels.csv` 
: id별 동작과 동작 label(61 kinds) 
: 3125 obs and 3 vars 
`test_features.csv` 
: id별 600 time간 동작 데이터. 782 ids x 600 time = 469200 obs 
: 8 vars

`sample_submission.csv` : submission 
