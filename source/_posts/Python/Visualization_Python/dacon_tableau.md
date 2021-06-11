---
title: "[Tableau] 시각화"
date: 2021-06-11
tags: Tableau, EDA
---
# [Tableau] 시각화
![](/hueman_images/kaggle/tableau.png)
Dacon 신용카드 사용자 연체 예측 AI 경진대회를 나가면서 시각화 작업을 무엇으로 할까 고민하다가 **Tableau**라는 좋은 툴을 알게되었습니다.
Python의 matplotlib, seaborn 를 사용하다가 Tableau tool을 사용하니 정말 편리하지 않을 수가 없었는데요😎\
빠르게 원하는 결과 형태를 볼 수 있다는 것이 가장 큰 장점이였습니다!
데이터를 그룹화하거나 함수를 적용하여 데이터를 변경하는 등 편리하게 작업할 수 있었습니다.\
아래의 링크는 DACON에서 참여한 대회의 데이터들을 시각화한 링크입니다.👇
> [https://public.tableau.com/app/profile/eunjin.jun/viz/credit_eda_jej/creditcard_ai_jej](https://public.tableau.com/app/profile/eunjin.jun/viz/credit_eda_jej/creditcard_ai_jej)

잠깐 보고 오셨다면 Tableau를 사용하여 어떻게 시각화를 진행하였고 어떤 데이터의 특징들이 있었는지 자세히 살펴보도록 하겠습니다!

## DACON 신용카드 사용자 연체 예측 AI 경진대회 


---
![](/hueman_images/kaggle/creditcard.png)
간단하게 [출전한 대회](https://dacon.io/competitions/official/235713/overview/description/)에 대하여 설명해보겠습니다.
- 주제: 신용카드 사용자 데이터를 보고 사용자의 연체 대금 정도를 예측하는 알고리즘 개발 
- 배경: 신용카드사는 신용카드 신청자가 제출한 개인정보와 데이터를 활용해 신용 점수를 산정합니다. 신용카드사는 이 신용 점수를 활용해 신청자의 향후 채무 불이행과 신용카드 대급 연체 가능성을 예측합니다.

즉, 간단하게 설명하자면 **신용카드 사용자들의 개인 신상정보 데이터로 사용자의 신용카드 대금 연체 정도를 예측**하는 대회입니다!

제 Github 링크를 들어가시면 출전한 대회(Project)들을 확인하실 수 있습니당🥰\
👉 [DACON, Kaggle 대회](https://github.com/eunjin-jun717/Project/tree/main/DACON%20%EC%8B%A0%EC%9A%A9%EC%B9%B4%EB%93%9C%20%EC%82%AC%EC%9A%A9%EC%9E%90%20%EC%97%B0%EC%B2%B4%20%EC%98%88%EC%B8%A1%20AI%20%EA%B2%BD%EC%A7%84%EB%8C%80%ED%9A%8C)

자 이제 본론으로 들어가서 Tableau를 사용하여 이 데이터 셋들을 시각화한 결과들을 살펴보도록 하겠습니다!!





## Tableau 시각화


---



### 1. credit - 종속변수
![](/hueman_images/kaggle/credit.png)
- credit 0,1,2가 차지하는 비율이 잘 보이지 않아 따로 text 처리를 해주었습니다.


### 2. Gender & Car
![](/hueman_images/kaggle/gender_car.png)
- Gender와 Credit 간의 관계, Car과 Credit과의 관계를 나타냈습니다. 
- Pie chart를 사용하여 나타내는 비율을 더 뚜렷하게 확인할 수 있습니다.


### 2. House type
![](/hueman_images/kaggle/housetype.png)
- House type과 Credit간의 관계를 확인하였을 때, 확실한 데이터 불균형이 나타나는 것을 확인하실 수 있습니다.
- 이 외에도 대부분의 독립변수들이 데이터 불균형을 이루고있습니다. 
- 따로 설명드리진 않겠습니다!
 
- House/Apartment에서 대부분을 차지하고 다른 주거형태에서는 적은 수의 데이터들이 분포된 것으로 보입니다. 
- 그렇다면 이런 불균형한 데이터들에 대해서는 전처리를 어떻게 해야될지 고민이겠죠?
- 방법은 있습니다! 제가 작성한 글 중에 종속변수와 독립변수들에 대해 데이터 불균형 처리한 글들을 확인해보세요!\
✔ [Imbalanced_data - response variables](https://eunjin-jun717.github.io/2021/05/13/Python/ML/imbalanced_data_response_variables/)

### 3. Family Size & ChildNum
![](/hueman_images/kaggle/familysize_childnum.png)
- Family Size가 증가할 수록 Child_num 또한 증가하는 선형관계를 확인할 수 있습니다. 

### 4. ChildNum
![](/hueman_images/kaggle/ori_childnum.png)
- 기존의 ChildNum 변수의 형태를 보면 데이터 불균형을 이루고있어서 어떻게 처리를 할까 고민하였습니다. 
- childnum이 1이상이면 자녀가 있음 / 0이면 자녀 없음 으로 범주화하였습니다. 
![](/hueman_images/kaggle/childnum.png)

### 5. Income Total
![](/hueman_images/kaggle/incometotal.png)
- 수입 역시 데이터의 불균형이 심한 것을 확인하였고, 그룹화를 진행하였습니다. 
- 불균형이 좀 더 사라지고 꼬리가 긴 분포의 형태가 확실히 줄어든 것을 확인하실 수 있습니다.


### 6. Day Birth
![](/hueman_images/kaggle/day_birth.png)
- 태어난 일자가 1일이면 -1 형태로 저장되는 데이터였습니다. **나이**변수를 변경하여 그룹화 하였습니다. 
- 아래와 같이 Generation으로 나타낼 수도 있습니다. 
![](/hueman_images/kaggle/age.png)


---
모든 변수들에 대하여 설명하진 않았습니다! 더 자세히 보고싶다면 위에 올려둔 저의 Tableau gallery를 참고하여주세요😊\
가장 기본적인 사용법만 익힌 상태에서 Tableau tool을 사용하여 시각화를 진행하였습니다. 부족한 점도 많겠지만 다음번엔 좀 더 디테일한 시각화를 보여드리도록 하겠습니다!\
감사합니다👏





