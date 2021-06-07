---
title: "[Deep Learning] CNN(Convolution Neural Network)"
date: 2021-06-05
tags: DL
---

*references*
- 아래의 자료를 참고하였습니다. 감사합니다. 
- [https://22-22.tistory.com/26](https://22-22.tistory.com/26)


# [DL] CNN(Convolution Neural Network)
- CNN이란 합성곱(convolution)을 이용해 Feature를 추출하는 layer이다.
- Conv 특성상 근처의 데이터끼리만 묶어서 처리한다는 특징이 있다. 

## 1. 합성곱 계층
- 이미지의 경우(w,h,color)형상으로 되어있으므로, 3차원 입력 받아 3차원을 출력한다. 
- 합성곱 계층의 입출력 데이터를 feature map이라한다. 
- 입력 데이터는 input feature map, 출력 데이터는 output feature map이라 한다.
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcyAizb%2FbtqEnJeB7eM%2F9H9DGofIN8WhLJqQpJ0EPK%2Fimg.png)

### 1-1. Conv 연산
- 필터의 윈도우를 이동해가며, 각각의 범위 내 데이터만 적용된다.
- 단일 곱셈-누산(FMA): 입력과 필터에 각각 대응하는 원소끼리 곱한 후 총합을 구한다
- 위 과정을 모든 장소에서 수행
- 필터를 적용한 원소에 고정값(bias)를 더한다. 
> input feature map이 filter와 패턴이 일치할수록 output feature map의 값 또한 크다. 
- 즉, 입력값이 filter와 얼마나 일치하는지 나타낸다. 
- 이미지나 음성 등에서 특정 패턴을 추출할 때 많이 사용한다. 
- 계층이 얕으면 CNN은 둥근 모양, 날카로운 모양 등 저수준의 패턴 정보를 추출한다.
- 계층이 깊으면 추상적인 정보를 추출한다. 


### 1-2. Filter
- 특징을 가진 값으로 입력 데이터와 “동일 차원”을 가진다. = kernel이라고 한다. 
- 예를 들면, 필터는 이미지에 있는 특징 등을 나타낸다고 볼 수 있다. 
- Filter를 여러 개 사용하여 한 번에 여러 feature를 추출할 수 있다. 

## 2. Padding
- 패딩이란, 입력 데이터 주위에 0을 채우는 것이다. CNN의 출력 크기를 조절하기 위해 사용
- Padding이 없으면 CNN layer를 통과할 때 마다 가로,세로 크기가 감소하지만, padding을 적용하면 conv을 수행해도 데이터의 크기가 동일하다. 

## 3. Stride
- 필터를 적용하는 위치의 간격을 의미한다.
- 예를 들어 필터가 오른쪽, 아래쪽으로 한 칸 씩 움직이면 stride는 1이다. 
- Padding, stride의 출력크기는 아래와 같다.
- ![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbnoVrS%2FbtqEm2yWMQT%2FYBq5ycKUc8nJEvpe8FTb91%2Fimg.png)


## 4. 3차원 데이터
- 이미지 데이터는 3차원 즉, h,w,channel이 3개의 차원으로 나눠서 생각할 수 있다. 
- h,w크기의 데이터에 대해서 각 channel 별로 각각 다른 filter를 이용해 conv를 수행한다.

## 5. Pooling
- 데이터의 크기를 줄이는 연산을 의미한다.
> 즉, 데이터의 여러 값을 묶어서 하나의 데이터로 줄이는 과정
- pooling에는 max,avg,global avg pooling 등이 있다. 
> 1) max pooling: 영역 내 최댓값을 취해 pooling하는 방법\
> 2) Average pooling: 영역 내 값을 평균내서 pooling하는 것\
> 3) global avg pooling: feature map 전체에 대해서 avg pooling을 취하는 방법

- 즉, 영역 크기가 H*W로 feature map의 크기와 동일하다. 
- 즉, H*W*C 크기를 가지는 feature map을 1*1*C 크기의 하나의 뉴런으로 mapping하는 것이다. 

## 6. Epoch - 에폭
- 인공신경망에서 전체 데이터 셋에 대해 한 번 학습을 완료한 상태를 의미
- 순방향과 역방향을 모두 완료하면 한 번의 epoch이 완료된다. 
- 1 epoch은 학습에서 훈련 데이터를 모두 소진했을 때의 횟수이다.
- 예를 들어, 훈련 데이터 10000개를 100개의 미니배치로 학습할 경우, 최적의 가중치를 구하기 위해 SGD를 100번 반복해야 모든 훈련 데이터를 학습에 활용하기 때문에 100회가 1 epoch이 된다. 
- 손실함수는 오차제곱합(Sum of Squares for error), 교차 엔트로피 오차(Cross Entropy Error)가 있으며, 오차제곱합은 회귀에 주로 사용, 교차 엔트로피오차는 분류에 주로 사용함

## 7. CNN layer의 응용


### 7-1. FC(Fully Connected)
- Conv와 max pooling을 반복하면 반복할수록 추상적인 특징이 남는다. 제일 마지막 feature map을 fully connected layer에 연결하는 경우가 많다.

### 7-2. One-by-One(1*1) conv
- Channel 수를 조절하기 위한 conv이다. 높은 차원을 낮은 차원으로 축소하는 dimension reduction에 사용한다. 
- Dimension reduction은 아래와 같은 특징이 있다.
- 차원 수를 줄여 불필요한 feature를 줄임/ 파라미터를 줄여 연산량 줄임/ 비선형성 증가
