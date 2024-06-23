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
- 토큰화, 정수인코딩 & 패딩
- Word Embedding

#### 2. Model


#### 3. Predict and Submit 


#### 2. 
