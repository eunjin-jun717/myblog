---
title: "[ML] Gradient Boosting"
date: 2021-04-15
tags: python
---
# [Machine Learning] Gradient Boosting 
- Catboost, XGBoost, LightGBM의 기반이 되는 부스팅이다.
## 1. 과정
### 1) 최초, 평균 값으로 예측
![](/hueman_images/python/ml/gb1.png)
- Height, favorite color, gender로 weight을 ‘일반적’으로 예측하는 방법은, 먼저 weight의 평균값으로 예측하는 것이다. (avg weight = 71.2)

### 2) 예측 값과 실제 값의 오차 구하기
- Residual(오차)들을 계산해보자. 당연히 클 수밖에 없다. 
![](/hueman_images/python/ml/gb2.png)

### 3) 오차 값을 예측하는 Tree 만들기
- 위에서 계산한 오차를, dataset의 feature들(height, favorite color, gender)을 가지고 트리를 만들어 예측한다. 
![](/hueman_images/python/ml/gb3.png)
- Leaf node에 분류되 값들이 여러 개 있는 경우에는 평균을 내서 하나의 값으로 만들자
![](/hueman_images/python/ml/gb4.png)
  
### 4) Learning rate를 적용하여, 기존 예측 값 업데이트하기
- 오차 값(16.8)에 원래의 예측값(71.2)을 더해주어 업데이트한다. 
- 그러나 이때 값은 88이다. 이 값은 원래의 weight와 동일하다.
- 그냥 더해주기만 하면 기존 dataset에는 완벽히 fitted하지만, new dataset에는 안맞을 가능성이 높다
- 따라서 그냥 더하지 않고 learning rate만큼 더해준다. 
- 여기서는 learning rate를 0.1로 잡아준다.
  ![](/hueman_images/python/ml/gb5.png)
- 0.1을 곱해서 더해주니 예측값은 72.9가 나왔다. 
- 처음 예측값(71.2) -> 현재 예측값(72.9) 
- 원래의 weight는 88이니 이전보다 오차가 줄어들었다. 
- 또 다시 원래의 데이터 값에 현재 예측값을 뺀 오차에 대한 트리를 만들며 일정 loop를 반복하다보면 점점 원래의 값인 88에 가까워 진다.
- 즉, High Variance를 피하면서 점진적으로 학습시키는 것이다.

### 5) 전체적인 구조
![](/hueman_images/python/ml/gb6.png)

#### *References*
- [https://www.youtube.com/watch?v=2xudPOBz-vs](https://www.youtube.com/watch?v=2xudPOBz-vs)
- [https://dailyheumsi.tistory.com/116?category=815369](https://dailyheumsi.tistory.com/116?category=815369)