---
title: visualization
date: 2021-03-22 22:09:02
tags:
---

## 간단하게 시각화 하는 과정
- step1) 패키지 설치
```r
install.packages("ggplot2")
```
- step2) 패키지 불러오기
```r
library(ggplot2)
```

- step3) 데이터 불러오기
```r
data("iris")
```

- step4) 데이터 확인하기
```r
str(iris)
```  

- step5) 가공되지 않은 Raw data 가공하기
  
- step6) 시각화하기
```r
ggplot(data=iris, 
       mapping = aes(x = Petal.Length, 
                     y = Petal.Width,
                     colour = Species)) +
  geom_point(size=3)
```
*tap 누르면 자동입력기능 있음.
*help에서 ggplot sample 확인 가능.

![](Images/iris_data.png)<!-- -->
