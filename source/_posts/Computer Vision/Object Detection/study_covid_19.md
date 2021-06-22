---
title: "[DL] Kaggle COVID-19 Detection 대회 Study"
date: 2021-06-15
tags: DL
categories:
  - DL
  - Study
---

*references*
- [https://www.kaggle.com/c/siim-covid19-detection/discussion/240878](https://www.kaggle.com/c/siim-covid19-detection/discussion/240878)

# [Deep Learning] Kaggle COVID-19 Detection 대회 Study

Object Detection에 관심을 가지게 되면서 YOLOv4의 논문을 리뷰해본 적이 있습니다.([YOLOv4 논문 리뷰](https://eunjin-jun717.github.io/2021/06/02/Computer%20Vision/Object%20Detection/YOLOv4/))

어떤 이론에 대해 조금 공부하게 되면 더 깊은 내용의 이론들을 공부하고 싶어지더라구요! 새로운 기법이나 알고리즘들이 가장 빠르게 들어오고 공부할 수 있는 곳은 Kaggle이라고 생각해서 딥러닝을 공부하기 쉬운 경로인 Kaggle 대회를 나가기로 하였습니다🎈

현재 제가 나가려는 [딥러닝 대회](https://www.kaggle.com/c/siim-covid19-detection/rules)를 잠깐 보시면 아시겠지만 의학쪽에 있는 지식이 바탕이 되어있어야 훨씬 대회를 이해하기가 쉬웠습니다. 저는 지식이 없는터라 Discussion을 보면서 dataset에 대한 이해와 여러 의문점들을 해결할 수 있었습니다! 

앞으로 몇 개의 글들은 대회를 본격적으로 나가기 전에 공부를 하기 위한 글들이라고 보시면 되겠습니다😊 

## Understanding Studies
- Kaggle 대회인 siim-covid19-detection의 코드들을 살펴보기 전에 데이터셋에 대한 이해가 필요하다.
- dataset은 아래와 같이 구성되어있다. 
> 1. test -> 약 1184 이상의 dcm 파일들
> 2. train -> 약 6024 이상의 dcm 파일들
> 3. sample_submission 
> 4. train_image_level
>> id/ boxes/ label/ StudyInstanceUID 
> 5. train_study_level
>> id/ Negative/ Typical/ Indetermine/ Atypical
- image level label들과 study level label들의 상호작용이 어떻게 되는지
- 어떤 이미지들은 bbox를 가지고 있고 가지고 있지 않은지 
- Image level dataset: opacity 위치를 찍기 위해서 bbox를 예측
- Study level dataset: 분류를 위해(Negative/ Typical/ Indetermine/ Atypical)
  - Negative는 No-covid
  - 그 외 3가지는 covid19에 감염되어 있는 환자들 상태 나타냄



### Understanding at a high level
- 한 개의 이미지 혹은 여러개의 이미지(9개까지)를 포함한다. 
- 연구안에 모든 이미지들은 또한 image level dataset에서 찾았다. 
- 만약에 study가 여러개의 이미지들을 포함한다면, 하나의 이미지에서 maximum 그리고 zero image들에서의 minimum은 bbox를 가질 것이다. 비록 모든이미지들이 image level dataset에 존재하더라도!
- 여러개의 이미지들이 study안에 존재한다고 생각하는 2개의 typical 시나리오
> a. image들은 거의 identical하다.(비교적 흔함) 차이점은 블러링/뾰족함/밝기/위치변화 등이 있을 것이다. 예를 들어 example a&c 를 보아라.\
> b. 이미지들은 각자 다르다(비교적 흔하진 않음) example b를 보아라
- 이미지들은 가끔 잘못되어있다. defect는 보통 아래의 category들 중 하나이다. 
> poor quality, blurry, too-sharp, perspective transform, cropped poorly, exposure/brightness, skew, etc.

### 몇몇의 관찰된 defects/weirdness
- 모든 관찰은 이 understanding of studies의 저자분의 노트북에서 볼 수 있고, 여러 이미지들을 포함한 studies 안에서 이미지를 언급하기 위함

### 1) Example A - 작은 차이가 있는 거의 동일한 이미지들
![](https://i.ibb.co/3mVP2Md/Screen-Shot-2021-05-21-at-6-32-22-PM.png)


### 2) Example B - 이미지가 다름 / 같은 환자의 사진인데 angles, times, positions들이 다른듯
![](https://i.ibb.co/qFjd8c3/Screen-Shot-2021-05-21-at-6-35-14-PM.png)

### 3) Example C - horizontal flip 을 보여주는 거의 동일한 이미지들
![](https://i.ibb.co/dGzTX34/Screen-Shot-2021-05-21-at-6-34-57-PM.png)

