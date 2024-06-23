# 자연어 기반 기후기술분류 스터디 

DEC 2021 - FEB 2022

DACON link : https://dacon.io/competitions/official/235744/overview/description

분석 목적 : 국가 연구 개발과제를 '기후기술분류체계'에 맞추어 라벨링 하는 알고리즘 개발     
연구 과제명, 연구 목표, 요약문 등의 텍스트 내용을 바탕으로 기후기술 label로 분류하기   

> **NLP** ; Natural Language Processing     
> 컴퓨터가 인간의 언어를 이해하고 해석하여 처리할 수 있도록 하는 일    
> 토큰화 → 불용어 제거 → 인코딩 → 패딩    


#### 0. Read Data 
`train.csv` : 기후기술분류 label 포함. 174304 obs, 13 var   
`test.csv` : 기후기술분류 label 미포함. 43576 obs, 12 var    
`sample_submission.csv` : (43576, 2)      
`labels_mapping.csv` : label과 기후기술분류체계를 mapping 한 meta data

#### 1. Konlpy 자연어 처리 
konlpy 형태소 분석기 Kkma Mecab Okt → **Okt** 사용

1) `과제명` 변수 EDA
- Length : too short or too long 
- Duplicates : 동일한 과제명임에도 다른 label로 분류된 case 존재
- 영어, 숫자, 특수문자 처리

2) Preprocessing (전처리)
- Stopwords(불용어) 제거 
  - 조사 / 어미 / 접미사 / 접속사 파악 후 제거
  - 잘못 분류된 경우 제거 
  - 의미 없는 단어 & 자주 사용되는 단어 제거
  - 길이가 1인 경우 제거 
- 토큰화 & 정수 인코딩 & 패딩
  -  토큰화 : 말뭉치를 벡터화하고 단어 집합 생성
  -  정수 인코딩 : 텍스트 안 단어들을 숫자 시퀀스로
  -  패딩 : 병렬연산을 위해 문장의 길이를 동일하게. post padding 적용
- Word Embedding
  단어를 sparse represetation(one-hot encoding)이 아닌 dense vector 형태로 표현. 실수 값을 가지며 저차원. 훈련 데이터로부터 학습.
  
#### 2. Model
1) Modeling    
- tensorflow `Sequential()`       
- Embedding - (Batch Normalization) - Pooling(GAP) - (LSTM) - Dense(ReLU) - (Dropout) - Dense(Softmax) layer      
- 성능 향상을 위해 적절하게 layer 추가 및 제거     

2) Compile
- loss : cross entropy (for multiclass classification) 
- optimizer : adam
- metrics : accuracy

3) Fit
- epoch = 20
- 8:2 비율로 나누어 학습    
(+) use EarlyStopping to avoid overfitting 

#### 3. Predict and Submit 


