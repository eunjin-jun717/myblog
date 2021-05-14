---
title: "[ML] Ensemble 학습방법"
date: 2021-05-14
tags: python
---
# 앙상블 학습 유형

---
- 가장 많이 알려진 앙상블 학습 유형: Voting, Bagging, Boosting, Stacking 등
![ensemble](/hueman_images/python/ml/ensemble.png)


## 1. 앙상블 학습

### 1-1. Voting, Averaging
- 둘은 서로 다른 알고리즘을 가진 Classifier를 결합하는 방식
- Voting(분류/Classifier), Averaging(회귀/Regressor)
> Categorical인 경우에는 Voting\
> Numerical인 경우에는 Averaging 이기도 함 
- 방법
      -> 일정 수의 base model과 predict를 만든다. 
      -> 훈련 데이터를 나누어 같은 알고리즘을 사용 or
      -> 훈련 데이터는 같지만 다른 알고리즘을 사용
      -> 여기서 여러가지 방법으로 voting을 진행 



### 1-2. Hard Voting = magjority voting = max voting = plurality voting
- 각 모델은 test data set의 결과를 예측
- 예측값들의 다수결로 예측값을 정함
- 즉, 이진분류에 있어서는 과반수 이상이 선택한 예측값을 최종 예측으로 선택


### 1-3. Soft Voting = weighted voting
- hard voting 방법과는 다르게, 좀 더 유연한 보팅 방법
- Test dataset의 결과 가능성을 예측함
- 이 가중치를 특정 연산을 하여 분류 label의 확률값을 계산
- 가중치의 연산 방법: 보통 평균사용, 원하는 방식으로 사용 가능
- Hard voting보다 유연한 결과 얻을 수 있으며, 예측 성능이 좋아 더 사용

## 2. Bagging
- 평균을 통해 분산 값을 줄여 모델을 더 일반화 시킴
- 최종적으로는 Voting을 사용
      -> 일정 수의 Base model을 만듬
      -> 모델들의 알고리즘은 모두 같음
      -> 각각의 모델은 훈련 데이터셋에서 랜덤으로 만든 서브 데이터셋을 각각 사용
      -> 서브 데이터셋을 만드는 과정: Boot Straping
      -> 배깅의 대표적인 알고리즘: RandomForest

## 3. Stacking & Blending

### 3-1. Stacking
- 머신러닝 알고리즘으로 훈련 데이터셋을 통해 새로운 데이터 셋을 만들고, 이를 데이터셋으로 사용하여 다시 머신러닝 알고리즘을 돌리는 것
- 보통은 서로 다른 타입의 모델들을 결합
      - 개별적인 기반 모델: 성능이 비슷한 여러 개의 모델
      - 최종 메타 모델: Base model들이 만든 예측 데이터를 학습 데이터로 사용할 최종 모델
> **결론**
> - 여러개의 개별 모델들이 생성한 예측 데이터를 기반으로 최종 메타 모델이 학습할 별도의 학습 데이터 셋과 예측할 테스트 데이터 셋을 재생성하는 기법
> - 모델을 통해 input을 만들고, 다시 모델에 넣는 구조 때문에 meta-model 이라고도 부름

### 3-2. Blending
- Blending은 Stacking과 비슷하지만 다른 점이 있다면 Blending은 Validation set을 활용한다. 
- 방법
      step1) training/validation/test set을 나눈다
      step2) training set에 모델 피팅을 한다
      step3) validation/test set에 대하여 예측을 한다
      step4) validation set과 그에 대한 예측이 새로운 모델을 만드는데 사용한다. 
      step5) 4의 모델을 최종 test set에 대한 예측을 하는데 사용된다. 
- 단점
      - 사용하게 되는 데이터의 양이 적어짐
      - 최종 모델이 holdout set에 대해 과적합될 수 있음
- Stacking보다는 간단하며 정보 누설의 위험을 줄임
- *참고 : Hold out이란 ?*
> - Hold out은 데이터셋을 train과 test set으로 나누는 과정을 의미
> - train이 작으면 모델 정확도의 분산이 커짐(과소적합의 가능성)
> - train이 커지면 test로부터 측정한 정확도의 신뢰도 하락(과대적합)


## 3-3. Stacking & Blending 차이점
- Stacking에서는 cross-fold-validation을 사용하고, blending에서는 holdout validation을 사용함
- 그렇기 때문에 blending의 결과는 holdout set에 과대적합이 된 결과를 얻을 가능성이 높음
- blending은 valid set에 대한 예측값을 training set에 이용하지만, stacking은 training set에 대한 예측값을 활용함
- blending은 예측값 뿐만 아니라 원래 feature도 활용하는 반면, stacking은 예측값만 활용

*references*
- [https://subinium.github.io/introduction-to-ensemble-1/](https://subinium.github.io/introduction-to-ensemble-1/)
- [https://3months.tistory.com/486](https://3months.tistory.com/486)
- [https://m.blog.naver.com/sjc02183/221739648990](https://m.blog.naver.com/sjc02183/221739648990)
