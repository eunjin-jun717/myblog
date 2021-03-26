---
title: "주택 가격: 시각화 예제"
author: "Eunjin"
output:
  html_document:
    number_sections: true
    keep_md: yes
    toc: true
    toc_depth: 3
---

### 참조
[kaggle: house prices_ project](https://www.kaggle.com/erikbruin/house-prices-lasso-xgboost-and-a-detailed-eda)


- r chunk에서 message=FALSE를 하게 되면 패키지를 불러올때의 관련 메세지가 안보임

```r
library(knitr) # 마크다운 관련 패키지
library(ggplot2) # 시각화 패키지
library(plyr) # 데이터 분석, 가공 패키지
library(dplyr) # 데이터 가공 패키지
library(corrplot) # 상관행렬과 신뢰구간의 그래프
library(caret) # 전처리, 모형 시각화
library(gridExtra) # 시각화 이미지 분할
library(scales) # ggplot2에 의해 사용된 scaling 인프라 제공
library(Rmisc) # 데이터 분석, 유틸리티 동작
library(ggrepel) # 겹치는 텍스트 레이블 제거
library(randomForest) # 분류와 회귀를 위해 Breiman의 random forest algorithm 구현
library(psych) # 기술통계량 구해주는 패키지
#library(xgboost) # 머신러닝 패키지
```

- R로 train.csv파일과 test.csv파일을 부름

```r
train <-read.csv("train.csv", stringsAsFactors = F)
test <- read.csv("test.csv", stringsAsFactors = F)
```


```r
dim(train) # train의 데이터프레임 길이
```

```
## [1] 1460   81
```

```r
str(train[,c(1:10, 81)]) # 처음 10개의 변수와 response variable
```

```
## 'data.frame':	1460 obs. of  11 variables:
##  $ Id         : int  1 2 3 4 5 6 7 8 9 10 ...
##  $ MSSubClass : int  60 20 60 70 60 50 20 60 50 190 ...
##  $ MSZoning   : chr  "RL" "RL" "RL" "RL" ...
##  $ LotFrontage: int  65 80 68 60 84 85 75 NA 51 50 ...
##  $ LotArea    : int  8450 9600 11250 9550 14260 14115 10084 10382 6120 7420 ...
##  $ Street     : chr  "Pave" "Pave" "Pave" "Pave" ...
##  $ Alley      : chr  NA NA NA NA ...
##  $ LotShape   : chr  "Reg" "Reg" "IR1" "IR1" ...
##  $ LandContour: chr  "Lvl" "Lvl" "Lvl" "Lvl" ...
##  $ Utilities  : chr  "AllPub" "AllPub" "AllPub" "AllPub" ...
##  $ SalePrice  : int  208500 181500 223500 140000 250000 143000 307000 200000 129900 118000 ...
```


```r
test_labels <- test$Id # 테스트 Id는 test_labels에 저장
test$Id <- NULL
train$Id <- NULL
```


```r
test$SalePrice <- NA # Not Available 결측치, NULL은 출력되지 않지만 NA는 출력됨
all <- rbind(train, test) # train과 test 결합
dim(all) # 결합된 것을 all이라하며 데이터 프레임 길이 구함
```

```
## [1] 2919   80
```

```r
#Id가 없으므로 (데이터프레임 = 79개의 예측변수들 + SalePrice 인 response 변수)
```


```r
ggplot(data = all[!is.na(all$SalePrice),], #SalePrice에 결측값인 NA가 포함되어있는지 확인함
       # 이때 결측값이 아닌 SalePrice 만 선택
       aes(x= SalePrice))+    # x축 : SalePrice
  geom_histogram(fill="blue", binwidth = 10000)+  # 히스토그램으로 표현, bar은 blue, bar의 두께는 10000으로 설정
  scale_x_continuous(breaks=seq(0, 800000, by=100000), # x축의 범위는 0~800000, 100000단위로 끊어줌
                     labels= comma) # 숫자 3자리마다 ',' 넣음
```

![](house_miniproj_eunjin_files/figure-html/unnamed-chunk-6-1.png)<!-- -->
- 히스토그램을 보면 좌측으로 치우쳐져 있다.
이 말은 SalePrice가 낮은 집이 잘 팔린다는 뜻이고, SalePrice가 높은 집은 사는 사람이 거의 없다는 것을 의미한다. 


```r
summary(all$SalePrice)
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
##   34900  129975  163000  180921  214000  755000    1459
```
- SalePrice의 최저가격: 34,900, 최고가격: 755,000,
1사 분위수(1st Qu): 129,975 => 오름차순으로 정렬했을 때, 하위 25%의 SalePrice
중앙값(Median): 163,000 => 오름차순으로 정렬했을 때, 중앙에 있는 값인 SalePrice
평균값(Mean): 180,921 => SalePrice의 평균
1사 분위수(3rd Qu): 214,000 => 오름차순으로 정렬했을 때, 상위 25%의 SalePrice
NA's : 결측값


```r
numericVars <- which(sapply(all, is.numeric)) # numeric 변수들의 위치를 찾는데 그 결과를 벡터 또는 행렬로 반환
numericVarNames <- names(numericVars) # 저장된 numeric 변수들인 numericVars로 변수명 변경된 것을 Name벡터인 numericVarNames로 저장
cat('There are', length(numericVars), 'numeric variables') #cat함수는 행을 바꾸지 않음
```

```
## There are 37 numeric variables
```

```r
#numericVars의 갯수를 출력함

all_numVar <- all[, numericVars] # numericVars의 데이터들을 'all_numVar'에 저장 (numericVars는 그냥 벡터일뿐, 데이터프레임이 아님)
cor_numVar <- cor(all_numVar, use="pairwise.complete.obs") # all_numVar들의 상관관계를 저장
# use="pairwise.complete.obs" => 결측치가 포함된 데이터에서 상관관계를 구하기 위해 사용

cor_sorted <- as.matrix(sort(cor_numVar[, 'SalePrice'], decreasing = TRUE)) # SalePrice와의 상관관계만을 내림차순으로 정렬한 뒤 행렬로 변환한 것을 cor_sorted 행렬에 저장
CorHigh <- names(which(apply(cor_sorted, 1, function(x) abs(x)>0.5))) # 1:행, 2:열,  function(x) { abs (x)}
# 행단위로 SalePrice의 상관관계를 절댓값 형태로 취한 뒤 0.5 이상의 값만 추출함
# 추출된 것들의 변수명을 변경하고 CorHigh에 저장 => 0.5이상의 상관관계는 "관련이 높다"라는 의미
cor_numVar <- cor_numVar[CorHigh, CorHigh] # 0.5이상의 상관관계를 가진 값들로만 cor_numVar에 다시 저장

# tl.col(text legend color은 black), tl.pos(text legend position은 left와 top)
# corrplot.mixed 함수: 시각화 방법을 혼합할 때 사용
corrplot.mixed(cor_numVar, tl.col= "black", tl.pos="lt") #tl: text legend, cl: color legend
```

![](house_miniproj_eunjin_files/figure-html/unnamed-chunk-8-1.png)<!-- -->

- 상관관계가 0.5이상인 데이터들을 봤을 때, SalePrice와 가장 높은 상관관계를 가지는 것은 "OverallQual"인 전반적인 품질이었다. => 0.791

- 독립변수들 간에 강한 상관관계가 나타나는 문제를 multicollinearity (다중공선성)라고 하는데 이것이 문제인 것으로 보인다. 

1) SalePrice와 가장 상관관계가 높은 OverallQual를 제외하고 다음으로 높은 GrLivArea 와 GarageCars 사이의 상관관계를 보았을 때 매우 높은 0.8897 이다. 즉, 독립변수들 간의 강한 상관관계를 나타낸다. 
2) 상관관계가 0.5 이상인 높은 상관관계를 가진 변수는 나머지 6개가 있다.
TotalBsmtSF, 1stFlrSF, FullBath, TotRmsAbvGrd, YearBuilt, YearRemodAdd




