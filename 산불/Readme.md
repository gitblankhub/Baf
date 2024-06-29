# 2022 통계 데이터 분석 활용 대회 

JUN 2022 - JUL 2022 

공공용 마이크로 데이터 (MDIS) 사용 

## 기상 정보를 활용한 대형 산불 위험 환경 분석 

### 1. 데이터 수집 

- 예년산불피해대장, 산림청, 2012~2021
- 기상관측통계(지상, 시간), MDIS 기상청, 2014~2020
- 종관기상관측(ASOS), 기상청 기상자료개방포털, 2012,2013,2021
- 방재기상관측(AWS), 기상청 기상자료개방포털, 2012~2021


### 2. EDA 및 전처리 

#### 데이터 정제 

- 활용할 변수 선택    
기상데이터 변수 : 연도, 월, 일, 시, 발생장소_시군구, 기온, 강수량, 풍속, 풍향, 습도, 증기압, 이슬점온도, 해면기압, 현지기압, 일조, 지면온도    
산불데이터 변수 : 발생일시_년, 발생일시_월, 발생일시_일, 발생일시_시간, 진화종료시간_년, 진화종료시간_월, 진화시간_일, 진화시간_시간, 발생 장소_시군구, 발생원인_구분, ‘피해면적_합계

- 산불 데이터 오류값(중복) 삭제
  
- 기상 데이터 오류값 및 결측치 처리
  시간에 따른 근처 값으로 대체

**연도, 월, 시, 발생장소_시군구를 기준으로 2012~2021년 산불피해정보와 기상정보 병합하여 데이터셋 생성**

- 파생 변수 생성     
  산불발생일시, 진화종료일시 변수 : 년+월+일+시간을 조합하여 datetime형 변수로      
  진화시간 : 진화하는데 소요된 시간     
  대형산불 : 피해면적 30ha 이상 이진형 변수    

- 정규화 및 표준화
  일부 변수 로그 및 루트 변환, 표준화

### 3. Unsupervised Learning : PCA & Clustering 

- 주성분 분석
  연속형 변수 간 상관관계 존재 
  기상 관련 연속형 변수 주성분 분석을 통해 차원 축소 

- 군집 분석
  주성분 분석 결과 pc1, pc2를 기준으로 k-means clustering

### 4. Classification Model         
대형 산불 여부를 target 변수로 하는 분류 모형. 
- 7:3 Cross Validation
- SMOTE(Synthetic Minority Over-sampling Technique) Oversampling 으로 불균형 해소

- Model Comparison
  
||PCA 변수 추가 X | PCA 변수 추가|
|------|---|---|
|Random Forest|acc=0.97 f1=0.22|acc=0.93 f1=0.12|
|Light GBM|acc=0.94 f1=0.14|**acc=0.93 f1=0.11**|
|Logistic Regression|acc=0.84 f1=0.25|acc=0.90 f1=0.27|
|KNN|acc=0.97 f1=0.49|acc=0.97 f1=0.49|

Check Random Forest Feature importance result. 

### 5. Conclusion 
바람이 불지 않고 습한 조건, 발생월과 시간대, 화재 지역 강릉/춘천/삼척 여부, 일조량을 나타내는 변수가 대형 산불을 분류하는데 중요한 변수.      
관측일과 시간에 따라 기상 관측값 중 풍속과 습도, 일조량을 고려하여 산불 감시 구역을 선정     
특히 강릉, 춘천, 삼척 지역에서 나타나는 기상 변화를 주의 깊게 관찰하고 대응


### Drawback 

- not significant conclusion through Clustering
- limitation of only using 10 years weather information. Need longer term dataset or geographical/firefighting informative datasets. 








