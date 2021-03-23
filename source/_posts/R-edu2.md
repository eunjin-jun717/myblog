---
title: R-edu2
date: 2021-03-23 22:53:23
tags: r
---
![](Images/sample2.png)
## counties visualization practice
### 파일 불러오기
```r
getwd()
```
get working directory 라는 뜻으로 현재 위치를 알려준다.
```r
setwd('R_NCS_2020/1_day/')
```
set as working directory 라는 뜻으로 위치를 새로 설정할 수 있다. 

###dplyr 설치 방법 2가지
- method1) tidyverse 설치 
```r
install.packages("tidyverse")
library(tidyverse)
```
- method2) dplyr만 설치 
```r
install.packages("dplyr")
library(dplyr)
```

### 데이터 가져오기
```r 
counties <- readxl::read_xlsx("data/counties.xslx", sheet = 1)
```
data 폴더에 있는 counties.xlsx 파일을 가져온다. 
만약 경로 error가 뜨면 getwd()를 하여 현재 파일 위치가 잘못되어있는지 확인해본다.

### 데이터 확인
```r
glimpse(counties) 
```

### select 사용하여 원하는 변수들 추출
```r
counties_selected <- counties %>% 
  select(state, region, men, women, population)
  
counties_selected %>% 
  arrange(desc(state))
```
counties 변수들 중에서 state, region, men, women, population 변수들의 데이터만 고른다.
select한 것을 counties_selected라는 변수 이름으로 저장한다.

counties_selected 를 state 기준으로 내림차순 정렬한다. 

### filter 사용하여 불필요한 것 제거하고 보기
```r
counties_selected %>% 
  filter(population < 10000)
```
population이 10000명 이하인 것들만 보여준다.
조건이 두가지일 경우에는 아래와 같이 한다. 
```r
counties_selected %>% 
  filter(state == "California" | population > 100000)
```
state가 California이거나 population이 100000명 이상인 것을 추출한다. 

### 참조
- [https://github.com/dschloe/R_edu](https://github.com/dschloe/R_edu)