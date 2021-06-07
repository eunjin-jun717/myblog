---
title: "[R] R코딩 기초함수"
date: 2021-03-23
tags: r
---
![](/hueman_images/sample2.png)
## Visualization Practice using "counties"
### 파일 불러오기
- get working directory 라는 뜻으로 현재 위치를 알려준다.
```r
getwd()
```
- set as working directory 라는 뜻으로 위치를 새로 설정할 수 있다.
```r
setwd('R_NCS_2020/1_day/')
```
 

### dplyr 설치 방법 2가지
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
- data 폴더에 있는 counties.xlsx 파일을 가져온다. 
만약 경로 error가 뜨면 getwd()를 하여 현재 파일 위치가 잘못되어있는지 확인해본다.
  
```r 
counties <- readxl::read_xlsx("data/counties.xslx", sheet = 1)
```


### 데이터 확인
- glimpse 를 사용하여 counties에 무슨 변수들이 있는지 살펴볼 수 있다. 
```r
glimpse(counties) 
```

### Select 사용하여 원하는 변수들 추출
- counties 변수들 중에서 state, region, men, women, population 변수들의 데이터만 고른다.
select한 것을 counties_selected라는 변수 이름으로 저장한다.

- counties_selected 를 state 기준으로 내림차순 정렬한다. 
```r
counties_selected <- counties %>% 
  select(state, region, men, women, population)
  
counties_selected %>% 
  arrange(desc(state))
```


### Filter 사용하여 불필요한 것 제거하고 보기
- population이 10000명 이하인 것들만 보여준다.
```r
counties_selected %>% 
  filter(population < 10000)
```
- 조건이 두가지일 경우에는 아래와 같이 한다. 
- state가 California이거나 population이 100000명 이상인 것을 추출한다. 
```r
counties_selected %>% 
  filter(state == "California" | population > 100000)
```

### Arange 함수
- 오름차순, 내림차순으로 정렬 가능하다.
```r
counties_selected %>% 
  filter(state == "California" | population > 100000) %>%
  arrange(desc(public_work))
```

### Mutate 함수
- 새로 변수를 추가한다는 것 보다는 의미있는 데이터 발견하고자 할 때 사용한다. 
- public workers를 구해서 배열하는 코드이다. 
```r
counties_seletec <- counties %>%
  select(state, county, private_work, public_work, population)

counties_seleteced %>%
  mutate(public_workers = population * private_work /100) %>%
  arrange(public_workers) 
```

### Count 함수
- 각 행의 갯수를 셀 수 있고, 정렬까지 가능하다.
-  sort=TRUE이면 자동으로 내림차순
```r
counties_selected %>%
   count(state, sort =TRUE)
```
- 가중치를 두어 주별로 걸어서 출퇴근하는 사람의 수를 카운트할 수 있다. 
```r
counties_selected %>%
  count(state, wt= walkers_pop)
```

### 참조
- [https://github.com/dschloe/R_edu](https://github.com/dschloe/R_edu)