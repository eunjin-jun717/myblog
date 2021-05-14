---
title: "[ML] Catboost"
date: 2021-04-15
tags: python
---
# [Machine Learning] Catboost
## Catboost의 개념
- Catboost란 Gradient boosting를 기반으로한 부스팅이다. 
- 간단하게 Gradient boosting을 설명하자면, 첫번째 정답의 오차에 대해 학습시키고 또 두번째 정답의 오차에 대해 학습시키면서 점진적으로 weaker learner을 strong learner로 만드는 방법이다. 

## Catboost의 특징
### 1) Level-wise tree 는 Best First Tree 같이 트리를 만들어나가는 형태
![](/hueman_images/python/ml/cb1.png)
- XGB, LightBGM과 다르게 대칭 트리의 구조를 사용한다.
- 이렇게 균형잡힌 depth-wise trees의 장점은 오버피팅을 방지할 수 있음(그러나 단점으로는 leaf wise보다는 느림)
- Root node에서 leaf node까지 만들어진 조건들을 binary된 백터로 변환하여 각 index에 값을 저장함으로써 메모리에 저장할 필요가 없기 때문에 효율적인 테스트 가능!

### 2) 기존의 부스팅 모델인 Gradient boosting같은 경우에는 모든 훈련 데이터를 대상으로 잔차 계산을 했다면, Catboost는 일부만가지고 잔차를 계산한 뒤, 이것을 모델로 만든다. 
![](/hueman_images/python/ml/cb2.png)
- X1의 잔차만 계산하고, 이를 기반으로 모델을 만든다. 이 모델을 이용하여 x2의 잔차를 예측한다. 
- X1, x2 의 잔차를 가지고 모델을 만들고, 이를 기반으로 x3,x4의 잔차를 예측한다.
- 반복한다. 
- 이를 Ordered Boosting이라 한다. 

### 3) Random Permutation
![](/hueman_images/python/ml/cb3.png)
- Ordered 부스팅할 때, 순서를 섞어 주지않으면 매번 같은 순서대로 잔차를 예측하는 모델을 만들 가능성이 있음 따라서 Catboost에서는 데이터를 셔플링해서 뽑아내야한다.

### 4) Ordered Target Encoding
- 범주형 변수를 수로 인코딩 시키는 방법
  ![](/hueman_images/python/ml/cb4.png)
- Time과 feature1을 가지고 class_labels를 예측할 것이다.
- Mean encoding을 할 것인데 , 우리가 예측해야하는 값이 훈련 셋 피쳐에 들어가는 문제(Data Leakage)를 일으키지 않는 해결책으로 아래처럼 인코딩 할 것이다.
- 예를 들면, Friday에 cloudy는 (15+14)/2 =15.5로 인코딩
- Saturday에 cloudy는 (15+14+20)/3=16.3 으로 인코딩
- 즉, 현재 데이터의 target값을 사용하지 않고, 이전 데이터들의 타겟값만 사용하지 data leakage를 일으키지 않는 것!

### 5) Categorical Feature Combinations
![](/hueman_images/python/ml/cb5.png)
- Country와 hair color로 class label을 예측하려는데, 위와 같이 두 feature를 하나의 feature로 묶을 수 있는 경우가 생긴다. catboost에서는 하나로 묶어버린다.

### 6) One-hot Encoding
- Caldinality가 3이하인 범주형 변수들은 Target encoding이 아닌 one-hot encoding으로된다. Target 인코딩보다 더 효율적이다. 

## 3. Catboost의 한계
- 데이터 대부분이 수치형 변수일 경우에는 Light GBM보다 학습 속도가 느리다. 
- 따라서 범주형 변수에 더 적합하다

## 4. Hyper Parameter
- Learning_rate
- Random_strength
- L2_regulariser
- Catboost는 기본 파라미터로도 충분히 최적화가 잘 되어있기 때문에, 굳이 다른 파라미터를 더 사용하지 않아도 된다. 

#### *References*
- [https://dailyheumsi.tistory.com/136](https://dailyheumsi.tistory.com/136)
- [https://hanishrohit.medium.com/whats-so-special-about-catboost-335d64d754ae](https://hanishrohit.medium.com/whats-so-special-about-catboost-335d64d754ae)
