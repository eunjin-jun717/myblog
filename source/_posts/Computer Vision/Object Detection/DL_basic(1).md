---
title: "[Deep Learning] Computer Vision 용어 정리 (1)"
date: 2021-06-06
tags: DL
---
*references*
> 글을 쓰는데 도움을 준 자료들 입니다. 감사합니다.
- [https://velog.io/@hewas1230/ObjectDetection-Architecture](https://velog.io/@hewas1230/ObjectDetection-Architecture)
- [https://eehoeskrap.tistory.com/186](https://eehoeskrap.tistory.com/186)
- [https://eehoeskrap.tistory.com/300](https://eehoeskrap.tistory.com/300)


# [DL] Computer Vision 기초 용어 정리 (1)
YOLOv4 논문 리뷰를 하는데 필요한 용어들을 정리해보았습니다. 이것저것 용어들을 찾아보며 공부를 하다보니 두서없이 정리되었네요😂

## 1. 딥러닝 네트워크 판단


### 1-1. mAP
- Mean Average Precision -> AP의 평균이며 CNN을 평가할 때 사용하는 지표


### 1-2. FPS
- Frame Per Second -> 1초당 몇개의 이미지 Frame이 보여지는지 
- 즉, 1초당 보여지는 frame 수가 많을 수록 영상은 끊김없이 부드럽게 보여진다. 
- 참고로 인간의 눈은 보통 25-30 FPS를 보면 영상이 끊기지 않는다고 판단을 한다고 한다. -> 즉, 실시간성이 있다라고 판단
- 그러나 예를 들어 우리가 40 FPS의 영상에 익숙해져 있다가 25-30 FPS를 보면 연속적이지 않은데? 라는 생각을 가질 수도 있다.

## 2. Object Detection
- Object Detection이란 객체 감지를 뜻하는 용어이며, 과정은 아래와 같다. 
> object regognition(인식) -> object classification(분류) -> object Localization(좌표찍기) 
- 이때, Object classification + object localization 을 "object detection"이라 한다.
- object detection은 한 이미지 내에서 진행되는 것이다. 
- 예를 들어 아래의 사진을 보게되면 car, truck, umbrella, person 등 여러 개의 object가 있는 것을 확인할 수 있다. 
![](https://media.vlpt.us/images/hewas1230/post/7adcbe72-01fa-43a5-b45d-b01c962244ea/image.png)
- 이때 한 이미지 내에서 서로 다른 객체들을 분류하고 있는데 이것이 바로 object detection 이다. 
- object classification이라 생각할 수도 있지만 그것과는 조금 다른 개념이다. 
- object detection의 구조인 backbone-neck-head 구조를 보게 되면 알게 될 것이다.
- 알아보기 전에 pre-training과 fine tuning의 개념을 알아보겠다. 

## 2-1. Pre-training
- 사전훈련, 선행학습, 전처리과정 이라고도 한다. 
- Multi layered perceptron(MLP)에서 weight와 bias를 잘 초기화 시키는 방법이다.
- 이러한 pre-training을 통해서 효과적으로 layer를 쌓아서 여러 개의 hidden layer도 효율적으로 훈련할 수 있다
- 비지도학습이 가능하기 때문에(물론 이러한 가중치, 편향 초기화가 끝나면 label된 데이터로 지도학습해야한다->fine tuning) 레이블되지 않은 큰 데이터를 넣어 훈련시킬 수 있다는 장점이 있다. 
- 또한, Drop-out, mini-batch 방식을 사용해서 pre-training 생략하기도 한다. 


## 2-2. Fine Tuning
- 기존에 학습되어져 있는 모델을 기반으로 아키텍처를 새로운 목적(나의 이미지 데이터에 맞게) 변형을 하고 이미 학습된 모델 weights로부터 학습을 업데이트하는 방법을 말한다.
- 딥러닝에서는 이미 존재하는 모델에 추가 데이터를 투입하여 파라미터를 업데이트하는 것
- 예를 들어, cat과 dog 분류기를 만드는데 다른 데이터로 학습된 모델(VGG16, ResNet)을 가져다 쓰는 경우가 있다. VGG16의 모델 경우 1000개의 카테고리를 학습시켰기 때문에 고양이, 개, 2개의 카테고리만 필요한 우리 문제를 해결하는데 모든 레이어를 그대로 쓸 수는 없다. 따라서 가장 쉽게 이용하려면 내 데이터를 해당모델로 예측하여 보틀텍 피쳐만 뽑아내고, 이를 이용하여 fully-connected-layer만 학습시켜서 사용하는 방법을 취하게 한다. 하지만 이 경우에는 fine-tuning이라고 부르지 않는다. Feature를 추출해내는 layer의 파라미터를 업데이트하지 않기 때문이다. 
- 따라서 fine-tuning을 했다고 말하려면 기존에 학습된 레이어에 내 데이터를 추가로 학습시켜 파라미터를 업데이트 했다고 해야한다. 
- 주의할 점은 완전히 랜덤한 초기 파라미터를 쓴다거나 가장 아래쪽의 레이어(일반적으로 피쳐를 학습한 덜 추상화된 레이어)의 파라미터를 학습해버리면 과적합이 일어나거나 전체 파라미터가 망가지는 문제가 생긴다.

## 2-3. object detection architecture
![](https://media.vlpt.us/images/hewas1230/post/b8355298-e489-4838-adb6-67f04d89c51a/image.png)
1. Backbone
- https://media.vlpt.us/images/hewas1230/post/b8355298-e489-4838-adb6-67f04d89c51a/image.png
- 이것들은 ImageNet과 같은 ‘image classification datasets’에 pre-training된 모델이다. 
- Detection dataset에 fine-tune된다. 
- 깊어질수록 higher semantics와 함께 다양한 level의 feature를 생성하는 구조는 OD 네트워크의 후반부에 유용하다.
- Backbone의 가장 밑에 부분은 input layer이고, 가운데 두개(실제로는 적거나 더 많음)는 conv layer이고, 가장 위의 부분은 feature map이다.
- 결론적으로 backbone 파트는 일반적인 CNN cell을 거쳐 평범하게 feature map을 생성하는 파트가 된 것이다. 
- 게다가 image를 다루게 되니 이러한 상황에 최적화된 CNN 구조인 ImageNet을 주로, ResNet, VGG를 사용하는 경우가 많은 것이다. 

2. Neck
- 이름 그대로 Backbone과 Head를 이어주는 연결부이다.
- Neck에서는 backbone의 다른 stages에서 서로 다른 feature maps를 추출하게 되는데 이때 FPN, PANet, BI-FPN등이 사용될 수 있다. 
- 이때 FPN이란 Feature Pyramid Network인데 위의 설명을 다시 봅시다.

3. Head
- Bounding box들의 classification이나 regression 과 같은 검출이 이루어지는 실질적인 부분이다. 
- Output은 x,y,h,w와 k classes +1 의 확률(1은 배경을 위한 것) 형태이다. 




## 2-4. FPN(Feature Pyramid Network)
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F16xz2%2FbtqubeXA8WS%2FmQUOaaqCKPwUL5cVYDMl8k%2Fimg.png)
- Top-down 방식과 측면 연결을 통해서 기존의 convolutional network를 증강시킨다. 
- 이는 network가 single resolution input image로부터 풍부하고 다양한 scale의 feature pyramid를 구성할 수 있도록 한다. 
- 각 추출된 결과들인 low-resolution및 high-resolution들을 묶는 방식이다. 
- 각 레벨에서 독립적으로 특징을 추출하여 객체를 탐지하는데 상위 레벨의 이미 계산된 특징을 재사용하므로 멀티 스케일 특징들을 효율적으로 사용할 수 있다. 
- CNN 자체가 레이어를 거치면서 피라미드를 만들고 forward를 거치면서 더 많은 의미(semantic)을 가지게 된다. 
- 각 레이어마다 예측과정을 넣어서 scale변화에 더 강한 모델이 되는 것이다. 
- FPN은 skip connection, top-down, cnn forward에서 생성되는 피라미드 구조를 합친 형태이다. 
- forward에서 추출된 의미정보들은 top-down 과정에서 up sampling하여 해상도를 올리고 forward에서 손실된 지역적인 정보들은 skip connection으로 보충해서 스케일 변화에 강인하게 되는 것이다. 
- FPN 모델을 입력으로 임의의 크기의 단일 스케일 영상을 다루며, 전체적으로 convolutional한 방식으로 비례된 크기의 feature map을 다중 레벨로 출력하게 된다. 이 프로세스는 backbone convolutional architecture와는 독립적이며 ResNet을 사용하여 결과를 보여주게 된다. 
> 결국 피라미드 형태를 통해 검출 객체에 다양한 스케일 변화를 준다 라는 아이디어이다. 


## 2-5. NMS(Non-max Suppressed)
- 같은 물체를 한 번이 아닌 여러 번 검출 될 수 있는데 이때 NMS 알고리즘은 각 물체를 한번만 감지하게 한다.
> NMS란 확률의 최댓값을 도출하고 최댓값 아닌 것을 억제하는 것이다. 

## 2-6. Letter Box Image
- 보통 이미지는 4:3 16:9 사이즈이다. 그러나 네트워크 input은 608*608 사각형이다.
- 그러므로 원본을 그대로 넣으면 찌그러질 것이다. 따라서 여분을 매꿔주는 letter box image 기법을 사용한다. 
