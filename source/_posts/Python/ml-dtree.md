---
title: "[ML] Decision Tree"
date: 2021-04-14
tags: python
---
# [Machine Learning] Decision Tree 
## 1. Decision Tree(결정트리) 개념
- 분류와 회귀 모두 가능한 학습 지도 모델
- 스무고개 하듯이 예/아니오 질문을 이어가며 학습
![](/hueman_images/python/ml/dt1.png)  
- 이렇게 특정 기준(질문)을 따라 데이터를 구분하는 모델
![](/hueman_images/python/ml/dt2.png) 
- 첫 질문: Root node / 맨 마지막 노드: Terminal node(Leaf node)
- 데이터를 지나치게 나누다 보면 과대적합(Overfitting) 됨
![](/hueman_images/python/ml/dt2_1.png)
- 여기서 과대적합이란, 모든 데이터 셋이 모두 잘 분류되어 불순도가 0이 될 때 까지 분기해 나간다.
- 또한, Root에서부터 하위 노드가 많이 만들어질수록 모델이 복잡해져 과대적합이 발생할 수 있다.
- 따라서, 결정트리에 하이퍼파라미터를 주고 모델링하기
- 그렇다면 오버피팅 막는 전략은 무엇이 있을까?

### 1) 가지치기(Pruning)
: 가지치기란? 하부트리를 제거하여 일반화 성능을 높이는 것.
![](/hueman_images/python/ml/dt2_2.png)
#### 첫번째, 속성을 제한 한다.
- Max_depth를 통해 트리의 깊이를 제한한다. 
#### 두번째, 한 노드에 들어있는 최소 데이터 수를 정한다.
- Min_samples_split을 조정하면 된다.
![](/hueman_images/python/ml/dt3.png) 
- 왼쪽은 default, 오른쪽은 min_samples_split=4라는 규제를 주어 훈련 시킴. 리프 노드의 최소 샘플이 4개 이상이니, 과대적합을 피하였고 좀 더 일반화가 가능해진 것을 확인 할 수 있다. 

### 2) 그리드서치(Grid Search)
: 격자탐색이라 하며, 모델 하이퍼 파라미터에 넣을 수 있는 값들을 순차적으로 입력한 뒤에 가장 높은 성능을 보이는 하이퍼 파라미터들을 찾는 탐색 방법이다.
- 즉, 쉽게 말하면 세부적인 규율인 하이퍼 파라미터를 일일히 다 적용해가면서 어떤 규율이 모델에 적합한지 판단하는 것!
- EX) grid_dtree = GridSearchCV(dtree, param_grid= parameters, cv=3, refit=True)
- parameters에는 하이퍼 파라미터 정보담기 딕셔너리
- cv는 cross validation 교차 검증이다. 
- 이 함수를 수행한 뒤 이후에 grid_dtree.best_estimator을 활용하면, 학습과정에서 하이퍼 파라미터 조정으로 가장 좋은 성능을 보인 모델이 반환된다. 

### 3) 랜덤 서치(Random Search)
: 그리드서치는 말그대로 모든 경우를 테이블로 만든 뒤 격자로 탐색하는 방식이지만 랜덤 서치는 하이퍼 파라미터 값을 랜덤하게 넣어보고 그 중 우수한 값을 모인 하이퍼 파라미터를 활용해 모델을 생성하는 것!

## 2. 하이퍼 파라미터(Hyper Parameter)
- Max_depth: 최대 깊이
- 값이 클수록 모델의 복잡도는 증가
- Min_samples_split: 분할되기 위해 노드가 가져야 하는 최소 샘플 수 (default: 2)
- Min_samples_leaf: 리프 노드가 가지고 있어야 할 최소 샘플 수(default: 1)
- Max_leaf_nodes: 리프 노드의 최대 수
- Min_samples_leaf: 가지를 칠 최소 sample 수 
- Ex) min_samples_leaf=4 이면, 4보다 자식노드 개수가 작아지면 더 이상 분류하지 않고 끝낸다.

## 3. Criterion(분할 품질을 측정하는 기능)
### 1) 불순도- Gini 계수(결정트리의 기본 criterion)
- 우선, 불순도가 무엇인지 간단하게 설명해보자.
![](/hueman_images/python/ml/dt4.png)
- 위쪽: 순도가 높다
- 아래쪽: 불순도가 높음, 즉 순도가 낮다.
- 불순도 최대: 한 범주에 두 데이터가 딱 반반
- 그렇다면 불순도는 어떻게 구해지는가?
- 아래의 식을 Gini라고 한다. 6장의 예제를 보면서 간단하게 설명해보자.
![](/hueman_images/python/ml/dt5.png)
- gini = 1- (i번째 노드에 들어있는 k의 비율)
![](/hueman_images/python/ml/dt6.png)
- Versicolor의 Gini(불순도)를 위의 gini 식에 대입하면 아래와 같다. 
- 1- ((0/54)^2 +(49/54)^2 +(5/54)^2 ) = 0.168
- Gini 계수의 값이 0에 가까울수록 좋다.

### 2) 불순도- 엔트로피(Entropy)
![](/hueman_images/python/ml/dt7.png)
- (Pi는 한 영역에 존재하는 데이터 가운데 범주 i에 속하는 데이터 비율)
![](/hueman_images/python/ml/dt8.png)
- 위의 그림을 보며 엔트로피를 설명해보자.
- 전체 16개 중 빨간색(범주=1)는 10개, 파란색(범주=0)은 6개이다. 
- 이때의 엔트로피는 다음과 같다.
- 주황색 영역의 엔트로피는 0.95이다. 
- 그렇다면, 그림에서 빨간 선을 그은 뒤 두개의 영역으로 나눴을 때의 엔트로피는?
![](/hueman_images/python/ml/dt9.png)
- 엔트로피가 0.95에서 0.75로 내려갔다.
- 즉, 불확실성 감소 -> 순도 증가 -> 정보 획득
- 이런식으로 엔트로피가 낮아지는 방향으로 분할하면 된다.  

### 3) 정보 획득량(information gain)
- 정보획득량이란? 엔트로피(불확실성)의 감소량
![](/hueman_images/python/ml/dt10.png)
- 각 특성들이 훈련 예제들을 얼만큼 잘 분류할 수 있는가를 측정
- 트리 구축과정에서 테스트할 후보 특성의 순서를 결정할 때 사용
- Information gain = (분류 전) 처음 엔트로피 – (분류 후) 나중 엔트로피

## 4. DecisionTreeRegressor
![](/hueman_images/python/ml/dt11.png)
: 결정트리 문제는 회귀 문제에도 사용 가능하다. 
- DecisionTreeClassifer 와 비슷하지만 DecisionTreeRegressor은 클래스를 예측하는것이 아니라 어떤 값을 예측한다.
- 예를 들면, x1=0.6인 샘플의 target값을 예측한다 하자
- 루트 노드부터 순회하면 결국 value=0.111인 리프 노드에 도달한다.
- 이 리프노드에 있는 110개 훈련 샘플의 평균 target값이 예측된다. 
- 이 예측값을 사용해 110개 sample에 대한 MSE를 계산하면 0.015가 나온다. 
![](/hueman_images/python/ml/dt12.png)
- 즉, 각 영역의 예측값 = 그 영역에 있는 target 값의 평균 
- 알고리즘은 예측값과 가능한 많은 샘플이 가까이 있도록 영역을 분할한다.

#### *References*
- [https://wikidocs.net/43627](https://wikidocs.net/43627)
- [https://ysyblog.tistory.com/76](https://ysyblog.tistory.com/76)
- [https://scikit-learn.org/stable/modules/generated/sklearn.tree.DecisionTreeClassifier.html](https://scikit-learn.org/stable/modules/generated/sklearn.tree.DecisionTreeClassifier.html)
- [https://huidea.tistory.com/32](https://huidea.tistory.com/32)
- [핸즈온 머신러닝]()