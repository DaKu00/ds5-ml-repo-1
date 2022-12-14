# 제주도 도로 교통량 예측
제주도 도민의 증가와 입도 여행객의 증가로 인한 교통체증 문제에 해결에 도움이 되고자 프로젝트를 진행
제주도 도로 정보 데이터를 통해 특정 구간에서의 평균 속도를 예측하여 교통체증 여부를 판별

## 1. 데이터
### - 데이콘에서 주최하는 '제주도 도로 교통량 예측 AI 경진대회'에서 제공하는 데이터를 사용
https://dacon.io/competitions/official/235985/data

### - 총24개의 컬럼이 있는 데이터의 2개의 컬럼을 추가하여 진행
  - 주말컬럼과 월컬럼을 추가
    - 추가 작업 : 주말컬럼에는 공휴을을 포함시켜 휴일과 비휴일 형태로 구성(약간의 성능 향상)
<img src="https://user-images.githubusercontent.com/87750521/202093281-3a33f059-6051-4c2d-ac4c-5aa2d5cf1e95.png" width="430" height="400"/>

### - 학습에 필요하지 않거나, 도움이 되지 않는다 판단되는 컬럼을 삭제
  - 빨간 박스로 체크된 컬럼을 drop하여 학습 진행
  - target컬럼은 학습 데이터에서 삭제하고 정답 데이터로서 학습에 사용
<img src="https://user-images.githubusercontent.com/87750521/202093609-79b5b954-8349-4127-95bd-e5921a80fc0b.png" width="230" height="400"/>

### - 데이터셋 설정
  - 데이콘에서 받은 데이터는 train데이터와 test데이터로 나뉘어 있지만 test데이터에는 target이 없어 모델의 성능을 평가하기 힘듦
  - train데이터를 나누어 test데이터로 사용
    - test데이터는 22년 8월 데이터, train데이터는 22년 8월 이전의 데이터
<img src="https://user-images.githubusercontent.com/87750521/202096392-e786d998-0ba0-49f2-b66f-6fb3f0515455.png" width="1000" height="300"/>




## 2. 모델
### LightGBM모델을 사용하여 머신러닝 진행
- 예측 알고리즘과 정형데이터에서 좋은 성능을 보임
- 학습 속도가 빨라 데이터 수가 많을 때 적합

### MAE를 사용하여 모델평가
- 평균절대 오차로 예측값을 측정하여 오차값이 줄어드는 것을 직관적으로 확인하기 위함


## 3. 하이퍼파라미터 튜닝
- max_depth, num_leaves, learning_rate, n_estimators 4개의 파라미터가 오차값에 큰 영향이 확인됨
- 4개의 파라미터튜닝만으로 MAE값이 5.04대에서 3.55대로 감소
<img src="https://user-images.githubusercontent.com/87750521/202098537-a8619390-01c6-4302-b86e-2836ca86d0a2.png" width="300" height="130"/>
<img src="https://user-images.githubusercontent.com/87750521/202098021-966239c3-ab48-427a-b49f-8df0c3bcb57e.png" width="400" height="40"/>

## 4. 학습된 모델 테스트
- 학습이 완료된 모델이 얼마나 근사치로 구간 도로에서의 속도를 예측할 수 있는지를 
<img src="https://user-images.githubusercontent.com/87750521/203452058-2471a007-4a80-44e4-a043-2411c61d243f.png" width="500" height="350"/>



## 4. Feature importance
- 학습이 완료된 후에 feature importance를 뽑아 학습에 주요하게 사용된 컬럼을 확인
<img src="https://user-images.githubusercontent.com/87750521/202099476-6d8dba71-4d7c-48e3-9b53-4bee189ff3e6.png" width="500" height="350"/>


## 5. 후기
- 완성된 모델이 어떻게 예측되는지 확인한는 것보다 MAE값을 줄이는 것에 시간을 많이 소모한 것 같다. 
- 데이터 EDA과정에서 알아본 상관계수와 학습이 완료된 후에 Feature importance의 차이가 많은 것으로 보아 상관계수를 머신러닝의 컬럼 선정에 사용하는 것은 안된다는 것을 알 수 있었다.
- 공휴일을 주말 컬럼에 추가해 성능향상이 있었는데 연휴까지 계산하여 컬럼에 추가시킨다면 더 좋을 듯 하다.

