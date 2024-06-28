# 운동 동작 분류 AI

MAR 2022 - MAY 2022       
DACON link : https://dacon.io/competitions/official/235689/overview/description  

분석 목적 : 3축 가속도계(accelerometer)와 3축 자이로스코프(gyroscope)를 활용해 측정된 센서 데이터를 이용해 운동 동작 인식 알고리즘 개발 

3125개의 ID별로 0~599 time 마다 가속도, 자이로스코프 측정    
![image](https://github.com/gitblankhub/Baf/assets/99718641/cc3a2703-2f21-4f41-a7a5-c20d14f63358)    
time 간격은 0.02초 즉, 한 ID별로 12초동안 운동 측정     
총 61가지 운동 동작 분류     


### 0. Read Data 
`train_features.csv`
: id별 600 time간 동작 데이터. 
- 3125 ids x 600 time = 1875000 obs    
- 8 vars (id, time, acc_x, acc_y, acc_z, gy_x gy_y gy_z)
  
`train_labels.csv` 
: id별 동작과 동작 label(61 kinds) 
- 3125 obs
- 3 vars (id, label, label_desc)

`test_features.csv` 
: id별 600 time간 동작 데이터. 
- 782 ids x 600 time = 469200 obs
- 8 vars

`sample_submission.csv` : submission 

### 1. Explore data 
Through visualization, 
- label 별 id 수 count -> It is imbalanced data that 'Non Exercise' label is absolutely high amount. 
- 5~6 id's time-acc time-gy plot & correlation heatmap -> apprehending the trend in each exercise, and see whether there is correlation between acc & gy measurement.     
  ex) label 1 Band Pull-Down Row plot the time-acc time-gy plot and see the exercise's features (like acc x axis movement is higher than other acc axises)

### 2. EDA 
- 2-1. 파생변수        
`acc` : acceleartion vector $\sqrt{acc_x^2+acc_y^2+acc_z^2}$     
`gy` : gyroscope vector $\sqrt{gy_x^2+gy_y^2+gy_z^2}$   

- 2-2. scaling      
Standard scaling : mean=0 var=1  $\frac{x-\mu}{\sigma}$   

- 2-3. reshaping       
train (1875000, 8)     
-> X (3125, 600, 8) : explanatory var, 3125 ids 600 times 8 features [3D input - Batchsize, Width, Channels]    
train_labels (3125, 3)     
-> y (3125, 61) : target var, 3125 ids & **61** one hot encoding labels     
test (469200, 8)     
-> X (782, 600, 8) : 782 ids 600 times 8 features     

### 3. Model 

- Model 1 Structure    
(Conv1d - Batch normalization - MaxPooling1D - Dropout) layers - (Flatten - Dense - Batch normalization/Activation) layers

- ***Model 2 Structure***     
(Conv1d - Batch normalization - Dropout) layers - (GlobalAveragePooling1D - Dense - Activation)  

Stratified K(=5) fold 이용 (imbalanced data를 위한 kfold)      
train - epochs=50, batchsize=64, using early stopping(criterion=validation loss, patience=8)   

convolution layer hyperparameter    
kernel_size = 60 (time step 60씩 보기)  
filters = 256 - 128 - 64 (output filter 수)   
strides = 3 (kernel 3씩 이동하면서)   
activation = relu   
(Pytorch `Conv1d` https://pytorch.org/docs/stable/generated/torch.nn.Conv1d.html)  


output layer   
dense = 61       
activation = softmax  
optimizer = Adam    
learning rate = 0.001    
loss = categorical crossentropy   
metrics = accuracy    


### 4. Predict 


### (+) Weakness in 1D CNN Model :(
- spends a loooooong time to train model -> fix layers / need to tune hyperparmeters 
- no way to explain how this model classifies the labels

*Questions*     
imbalanced data -> use augmentation -> but lower performance?, highly imbalanced data .. how to solve this problem?   
how to set layers and hyperparameters ?
approach as time series data .. ?     
functional data anaylsis .. ?    

