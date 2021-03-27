---
title: "apply함수와 for문의 속도 비교"
output: 
  html_document:
    keep_md: yes

---


# 개념 정리
## apply 함수
- apply(X, MARGIN, FUN, ...)
- X: 배열, 매트릭스
- Margin: 행(1), 열(2)
- Fun: 함수
![apply()](/hueman_images/for_apply/apply.png)

```r
my.matrx <- matrix(c(1:10, 11:20, 21:30), nrow = 10, ncol=3) # 행이  10개, 열이 3개인 매트릭스 생성
my.matrx
```

```
##       [,1] [,2] [,3]
##  [1,]    1   11   21
##  [2,]    2   12   22
##  [3,]    3   13   23
##  [4,]    4   14   24
##  [5,]    5   15   25
##  [6,]    6   16   26
##  [7,]    7   17   27
##  [8,]    8   18   28
##  [9,]    9   19   29
## [10,]   10   20   30
```

```r
apply(my.matrx, 1, sum) # 매트릭스를 행단위 합 계산
```

```
##  [1] 33 36 39 42 45 48 51 54 57 60
```

```r
apply(my.matrx, 2, sum) # 열단위 합 계산
```

```
## [1]  55 155 255
```

```r
apply(my.matrx, 2, function(x) length(x)) # 직접 함수를 정의해서 사용 가능
```

```
## [1] 10 10 10
```

## lapply 함수
- lapply(X,FUN, ...)
- X: 벡터, 리스트
- 반환값: 리스트
![lapply()](/hueman_images/for_apply/lapply.png)
```r
vec <- c(1:10)
vec
```

```
##  [1]  1  2  3  4  5  6  7  8  9 10
```

```r
lapply(vec, sum) #list 형태로 나옴
```

```
## [[1]]
## [1] 1
## 
## [[2]]
## [1] 2
## 
## [[3]]
## [1] 3
## 
## [[4]]
## [1] 4
## 
## [[5]]
## [1] 5
## 
## [[6]]
## [1] 6
## 
## [[7]]
## [1] 7
## 
## [[8]]
## [1] 8
## 
## [[9]]
## [1] 9
## 
## [[10]]
## [1] 10
```

```r
A <- c(1:9)
B <- c(1:12)
C <- c(1:15)
my.lst <- list(A,B,C)
my.lst
```

```
## [[1]]
## [1] 1 2 3 4 5 6 7 8 9
## 
## [[2]]
##  [1]  1  2  3  4  5  6  7  8  9 10 11 12
## 
## [[3]]
##  [1]  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15
```

```r
lapply(my.lst, sum) # 각 list 안의 값들의 합
```

```
## [[1]]
## [1] 45
## 
## [[2]]
## [1] 78
## 
## [[3]]
## [1] 120
```


## sapply 함수
- sapply(X,FUN,...,simplify=TRUE, USE.NAMES=TRUE) 
- lapply와 같은 동작을 하지만, 가능하면 출력을 단순화 시키는 함수
- simplify=TRUE : 출력을 단순화시킴, simplify=FALSE : 단순화 시키지 않음
- USE.NAMES=TRUE : 이름 속성도 반환, USE.NAMES=FALSE : 이름 속성 없이 반환

```r
vec
```

```
##  [1]  1  2  3  4  5  6  7  8  9 10
```

```r
sapply(vec, sum, simplify=FALSE) # 출력을 단순화 하지 않으므로 리스트 형태로 출력
```

```
## [[1]]
## [1] 1
## 
## [[2]]
## [1] 2
## 
## [[3]]
## [1] 3
## 
## [[4]]
## [1] 4
## 
## [[5]]
## [1] 5
## 
## [[6]]
## [1] 6
## 
## [[7]]
## [1] 7
## 
## [[8]]
## [1] 8
## 
## [[9]]
## [1] 9
## 
## [[10]]
## [1] 10
```

```r
sapply(vec, sum, simplify=TRUE) # 출력을 단순화시킴 
```

```
##  [1]  1  2  3  4  5  6  7  8  9 10
```

```r
my.lst
```

```
## [[1]]
## [1] 1 2 3 4 5 6 7 8 9
## 
## [[2]]
##  [1]  1  2  3  4  5  6  7  8  9 10 11 12
## 
## [[3]]
##  [1]  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15
```

```r
sapply(my.lst, sum) # 출력을 단순화시킴 # simplify=TRUE 생략된 모습
```

```
## [1]  45  78 120
```

```r
lapply(my.lst, sum) # 리스트를 반환
```

```
## [[1]]
## [1] 45
## 
## [[2]]
## [1] 78
## 
## [[3]]
## [1] 120
```

## vapply 함수
- vapply(X,FUN,FUN.VALUE,...,USE.NAMES=TRUE)
- sapply함수와 비슷함.
- 차이점: value로 예상되는 데이터 유형을 지정해야함
- FUN.VALUE: 자료형 지정


```r
vec
```

```
##  [1]  1  2  3  4  5  6  7  8  9 10
```

```r
vapply(vec, sum, numeric(1)) # 1개의 숫자데이터로 나오게함
```

```
##  [1]  1  2  3  4  5  6  7  8  9 10
```

```r
my.lst
```

```
## [[1]]
## [1] 1 2 3 4 5 6 7 8 9
## 
## [[2]]
##  [1]  1  2  3  4  5  6  7  8  9 10 11 12
## 
## [[3]]
##  [1]  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15
```

```r
vapply(my.lst, sum, numeric(1)) # 1개의 숫자데이터로 나오게함
```

```
## [1]  45  78 120
```

```r
#vapply(my.lst, function(x) x, numeric(1)) # error 뜸
#1개의 데이터만 들어갈 수 있는데 함수의 결과는 9개가 나오므로 error뜸
```

## tapply 함수
- tapply(X, INDEX, FUN=NULL, ..., DEFAULT=NA,SIMPLIFY=TRUE)
- 그룹별로 처리함
- factor형으로 인자를 줌

```r
tdata <- as.data.frame(cbind(c(1,1,1,1,1,2,2,2,2,2), my.matrx)) # 열단위로 결합시킴
tdata
```

```
##    V1 V2 V3 V4
## 1   1  1 11 21
## 2   1  2 12 22
## 3   1  3 13 23
## 4   1  4 14 24
## 5   1  5 15 25
## 6   2  6 16 26
## 7   2  7 17 27
## 8   2  8 18 28
## 9   2  9 19 29
## 10  2 10 20 30
```

```r
colnames(tdata) # tdata의 열 이름
```

```
## [1] "V1" "V2" "V3" "V4"
```

```r
tapply(tdata$V2, tdata$V1, sum) # V1: index이며, V2가 함수의 인자로 전달됨 
```

```
##  1  2 
## 15 40
```

```r
# index가 1인 그룹으로 sum되고, index가 2인 그룹을 sum함
```

## mapply 함수
- mapply(FUN,...,MOREARGS=NULL, SIMPLIFY=TRUE, USE.NAMES=TRUE)
- sapply()와 유사하지만 mapply()는 다수의 인자를 함수에 넘길 수 있음


```r
mapply(rep, 1:10, 10:1) # 반복함수, 반복할 숫자, 반복되는 갯수
```

```
## [[1]]
##  [1] 1 1 1 1 1 1 1 1 1 1
## 
## [[2]]
## [1] 2 2 2 2 2 2 2 2 2
## 
## [[3]]
## [1] 3 3 3 3 3 3 3 3
## 
## [[4]]
## [1] 4 4 4 4 4 4 4
## 
## [[5]]
## [1] 5 5 5 5 5 5
## 
## [[6]]
## [1] 6 6 6 6 6
## 
## [[7]]
## [1] 7 7 7 7
## 
## [[8]]
## [1] 8 8 8
## 
## [[9]]
## [1] 9 9
## 
## [[10]]
## [1] 10
```

```r
tdata
```

```
##    V1 V2 V3 V4
## 1   1  1 11 21
## 2   1  2 12 22
## 3   1  3 13 23
## 4   1  4 14 24
## 5   1  5 15 25
## 6   2  6 16 26
## 7   2  7 17 27
## 8   2  8 18 28
## 9   2  9 19 29
## 10  2 10 20 30
```

```r
tdata$V5 <- mapply(function(x,y) x*y, tdata$V1, tdata$V2) # V1과 V2의 값들에 대한 곱
tdata
```

```
##    V1 V2 V3 V4 V5
## 1   1  1 11 21  1
## 2   1  2 12 22  2
## 3   1  3 13 23  3
## 4   1  4 14 24  4
## 5   1  5 15 25  5
## 6   2  6 16 26 12
## 7   2  7 17 27 14
## 8   2  8 18 28 16
## 9   2  9 19 29 18
## 10  2 10 20 30 20
```

# For문, apply함수 속도 비교(1)
## For문 사용 시 속도

```r
# 랜덤한 10000개의 숫자를 x1, x2에 저장
N <- 10000
x1 <- runif(N) # runif() : 랜덤숫자 발생함수
x2 <- runif(N)

# x1과 x2를 열단위로 묶어서 d에 data frame 형태로 저장
d <- as.data.frame(cbind(x1, x2))

# for loop으로 시간 체크
system.time(for(i in c(1:length(d[,1]))){ # 1부터 'd의 1열 길이'만큼 for 문이 반복함
  d$mean2[i] <- mean(c(d[i,1], d[i,2])) # x1, x2 각각 더해 평균을 구한것을 d의 데이터 프레임에 넣음
})
```

```
##    user  system elapsed 
##    0.80    0.05    0.84
```
- For문은 1부터 10000까지 한 명이 순차적으로 일을 처리한다.

## apply 함수사용 시 속도

```r
# apply 함수를 사용하여 같은 데이터를 처리해보자.
system.time(d$mean1 <- apply(d, 1, mean)) # d의 행에 대한 평균
```

```
##    user  system elapsed 
##    0.06    0.00    0.06
```

# For문, apply함수 속도 비교(2)

```r
install.packages('nycflights13', repos="http://cran.us.r-project.org")
```

```
## package 'nycflights13' successfully unpacked and MD5 sums checked
## 
## The downloaded binary packages are in
## 	C:\Users\study\AppData\Local\Temp\Rtmp08X2ug\downloaded_packages
```

```r
install.packages('dplyr', repos="http://cran.us.r-project.org")
```

```
## package 'dplyr' successfully unpacked and MD5 sums checked
## 
## The downloaded binary packages are in
## 	C:\Users\study\AppData\Local\Temp\Rtmp08X2ug\downloaded_packages
```

```r
library(dplyr)
library(nycflights13)
```

## apply 함수사용 시 속도

```r
head(flights)
```

```
## # A tibble: 6 x 19
##    year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
##   <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
## 1  2013     1     1      517            515         2      830            819
## 2  2013     1     1      533            529         4      850            830
## 3  2013     1     1      542            540         2      923            850
## 4  2013     1     1      544            545        -1     1004           1022
## 5  2013     1     1      554            600        -6      812            837
## 6  2013     1     1      554            558        -4      740            728
## # ... with 11 more variables: arr_delay <dbl>, carrier <chr>, flight <int>,
## #   tailnum <chr>, origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>,
## #   hour <dbl>, minute <dbl>, time_hour <dttm>
```

```r
flights_selected <- select(flights, flight, distance) # flight number와 distance를 뽑음
data_flight <- tapply(flights_selected$distance, flights_selected$flight, sum) 
# 데이터 양이 많아 우선적으로 각각의 flight마다 간 거리를 합함.
df <- data.frame(flight_num= c(1:length(data_flight)), flight_distance= data_flight)
# data frame 형태로 만듦 (인덱스 길이= flight 갯수)

# flight_num와 distance의 평균을 구함, 의미는 없으며 속도차이를 보기 위함!

## apply문 사용 시 속도 
# 행단위로 평균을 구함
system.time(df$mean3 <- apply(df, 1, mean)) 
```

```
##    user  system elapsed 
##    0.03    0.00    0.03
```

```r
## For문 사용 시 속도
system.time(for(i in c(1:length(data_flight))){
  df$mean2[i] <- mean(c(df[i,1], df[i,2]))
})
```

```
##    user  system elapsed 
##    0.28    0.02    0.29
```

# apply함수 사용 시 장점
- For문과 비교해봤을 때, apply함수 사용 시 속도가 빠음
- 대용량 데이터에 대해 짧은 코드로 반복 연산 처리 가능

## 참조
- 1. [https://ademos.people.uic.edu/Chapter4.html#:~:text=Apply%20functions%20are%20a%20family,and%20often%20require%20less%20code.](https://ademos.people.uic.edu/Chapter4.html#:~:text=Apply%20functions%20are%20a%20family,and%20often%20require%20less%20code.)
-2. [http://rstudio-pubs-static.s3.amazonaws.com/5526_83e42f97a07141e88b75f642dbae8b1b.html](http://rstudio-pubs-static.s3.amazonaws.com/5526_83e42f97a07141e88b75f642dbae8b1b.html)


