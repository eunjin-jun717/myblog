---
title: "[Deep Learning] Computer Vision 용어 정리 (2)"
date: 2021-06-07
tags: DL
---

*references*
> 아래의 자료들을 참고하였습니다. 감사합니다.
- [https://eehoeskrap.tistory.com/430](https://eehoeskrap.tistory.com/430)
- [https://deep-learning-study.tistory.com/635](https://deep-learning-study.tistory.com/635)
- [https://arxiv.org/abs/2002.05712](https://arxiv.org/abs/2002.05712)

# [DL] Computer Vision 기초 용어 정리 (2)
computer vision 용어 정리 1편에 이어서 YOLOv4 논문 리뷰를 하는데 필요한 용어들을 공부하였습니다.

## 1. Batch
![](/hueman_images/dl/batch2.png)
- 만약 데이터가 10만개 있을 경우, 데이터를 1개씩 입력 받아 총 10만 번의 연산을 진행하는 것 보다 한번에 큰 묶음으로 데이터를 입력 받아(Batch) n번의 연산을 진행하는 것이 빠르다. 
- 즉, 느린 I/O를 통해 데이터를 읽는 횟수를 줄이고 CPU나 GPU로 순수 계산을 하는 비율을 높여 속도를 빠르게 할 수 있다는 뜻


### 1-1. mini-batch
- 우선 데이터를 하나씩 학습시키는 방법/ 전체를 학습시키는 방법의 장단점을 소개한다. 
- 하나씩 학습: 장점- 신경망을 한번 학습시키는데 소요시간이 매우 짧다. but 데이터를 하나씩 입력받으면 계산 속도가 사실상 늦어짐
>  GPU의 병렬처리를 사용하지 않으니 그만큼 자원낭비됨 -> loss function에서 최적의 파라미터를 설정하는데 힘들다. 
- 전체데이터 입력: 한번에 여러개의 데이터에 대해 신경망을 학습시킬 수 있으니 오차를 줄일 수 있는 cost funtion의 최적 param를 빠르게 알아낼 수 있음
> but 신경망 한 번 학습시키는데 소요시간 매우 길다.
- 이 모든 장점들을 섞은 것이 mini-batch이다. 
- mini-batch는 SGD(Stochastic Gradient Descent:확률적 경사 하강법)와 Batch를 섞은 것으로 전체 데이터를 N등분해서 각각의 학습 데이터를 배치 방식으로 학습시킨다. 
- 따라서 최대한 신경망을 한 번 학습시키는데(iteration) 걸리는 시간을 줄이면서 전체 데이터를 반영할 수 있게 되며 효율적으로 CPU와 GPU를 활용함

### 1-2. small batch, large batch 비교
![](/hueman_images/dl/batch.png)
- Batch size가 작을수록 generalization performance 측면에서 긍정적 영향을 끼친다. 
- Batch size가 클수록 gradient는 정확해지지만 한 반복에 대해서는 계산량이 늘어나게 된다. 
- 그러나 한 반복에서 각 example에 대한 gradient는 병렬계산이 가능하므로, 큰 batch를 사용하면 멀티 GPU 등 병렬계산의 활용도를 높여 전체 학습시간을 단축할 수 있다.
> small batch size의 장점
> - Generalization performance가 좋아짐
> - Training stability가 좋아짐. 즉, 보다 넓은 범위의 learning rate로 학습이 가능하다. 

### 1-3. SGD(확률적 경사 하강법)
- SGD는 신경망을 학습할 때 마다 가중치를 갱신함.
- 학습할 때마다 가중치를 갱신하기 때문에 학습할 때마다 신경망의 성능이 들쑥날쑥하 변하면서 정답에 가까워지기 때문에 '무작위적'으로 보이기 때문
![](https://d1zx6djv3kb1v7.cloudfront.net/wp-content/media/2019/09/Neural-network-18-i2tutorials.png)


### 1-4. Normalization(정규화)
- 정규화하는 이유: 학습을 더 빨리 하기위해서 or local optimum 문제에 빠지는 가능성을 줄이기 위해 사용 
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbJfAq1%2FbtqEcFYhFpH%2F1tOyKt3nRLbcCMoFWmgZU1%2Fimg.png)
- 위의 그림을 보면 왼쪽에서 global optimum을 찾지 못하고 local optimum에 머물러있는 문제가 생긴다. 
- 해결하기 위해 정규화 하면 local optimum에 빠질 수 있는 가능성을 낮춘다. 

### 1-5. Internal Covariance Shift
- 학습에서 불안정화가 일어나는 이유라한다. 
- 네트워크의 각 레이어나 activation마다 입력값의 분산이 달라지는 현상을 뜻한다. 
- covariate shift: 이전 레이어의 파라미터 변화로 인해 현재 레이어의 입력분포가 바뀌는 현상
- internal covariate shift: 레이어를 통과할 때 마다 covariate shift가 일어나면서 입력의 분포가 약간씩 변하는 현상
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FPYpzO%2FbtqEbvPCvsc%2F3x9sukTLAwdqNWOkpwgTAk%2Fimg.png)


### 1-6. Batch Normalization
- 배치 정규화는 평균과 분산을 조정하는 과정이 별도의 과정으로 떼어진 것이 아니라, 신경망 안에 포함되어 학습 시 평균과 분산을 조정하는 과정 역시 같이 조절됨 
- 즉, 각 레이어마다 정규화하는 레이어를 두어, 변형된 분포가 나오지 않도록 조절하게 하는 것
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcFYkLE%2FbtqEcUnlXKy%2FZbGZNjObjo2gL2xss8zYzk%2Fimg.png)
- 즉, 간단하게 말하면 미니배치의 평균과 분산을 이용해 정규화 한 뒤, scale 및 shift를 감마값, 베타값을 통해 실행한다. 
- 이때, 감마,베타는 학습 가능한 변수
> 역전파를 통해 학습


### 1-7. CNN구조에서의 배치 정규화
- conv 레이어에서 활성화 함수가 입력되기 전에 wX+b 로 가중치가 적용되었을 때, b의 역할은 베타가 완전히 대신할 수 있기 때문에 b를 삭제함 
- 결국 배치 정규화를 사용 시 장점은
- 기존 방법에서 learning rate를 높게 잡으면 gradient가 vanish/explode거나 local minima에 빠지는 경향이 있는데 이는 scale 때문이었으며 배치 정규화를 사용할 경우 propagation시 파라미터의 scale에 영향을 받지 않게 되기 때문에 learning rate를 높게 설정할 수 있다. 
- regularization 효과가 있기 때문에 dropout 등의 기법 쓰지 않아도됨(효과는 같기 때문)

## 2. CBN(Cross-iteration Batch Normalization)
- CBN은 small batch size에서 발생하는 BN의 문제점을 개선하기 위해 이전 iteration에서 사용한 sample 데이터로 평균과 분산을 계산한다. 
- CBN은 테일러 시리즈를 사용해 이전 가중치와 현재 가중치의 차이만큼 compensation하여 근사화 한다. 
- 매 반복마다 변화하는 가중치 값이 매우 작다고 가정하기 때문에 테일러 시리즈 사용 가능
- BN의 문제점은 미니배치 사이즈가 작은 경우에 발생, 적은 sample로 평균, 분산을 추정하므로 추정된 값은 전체 training set과 동일하지 않다. 따라서 많은 노이즈를 포함하게된다. 
- 그렇다면 large batch size를 사용하면 BN은 문제가 될까? 
> 되지는 않지만, 많은 연산량과 메모리 점유율이 필요한 OD, segmentation task에서는 GPU한계 때문에 mini-batch-size를 사용할 수 밖에 없다. 
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcPiGZ7%2Fbtq4Y4KMwa1%2FWOPp1G7dR26IfSk9BWFFq0%2Fimg.png)
- 배치 사이즈가 4,2,1 인경우에 모델 정확도가 급격히 떨어짐

