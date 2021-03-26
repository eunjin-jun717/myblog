---
title: R코딩 기초1
date: 2021-03-23 22:06:56
tags: r
---
![](Images/sample.png)

# R 코딩 기초
## 실행방법
- Windows: Ctrl + Enter
- Mac: Command + Enter

## 변수 저장 시 
```r
a <- 3
```
Alt + - key 누르면 된다. 

## 변수 호출 시 
```r
print(x) 
#혹은
x 
```

# 변수 종류
## 변수 type
- 숫자형 변수 
```r 
my_numeric <- 17
```
- 문자형 변수 
```r 
my_character <- "eunjin"
```
- 논리형 변수 
```r 
my_logical <- FALSE 
my_logical <- TRUE 
```
## 벡터 생성
```r 
number_vector <- c(1,2,3)
number_vertor <- c(1,"2",3)
```
- 첫번째 라인은 숫자로만 이루어져있기 때문에 변수 type을 알아보면 "numeric" 으로 나온다. 
- 두번째 라인은 숫자, 문자가 섞여져 있다.
이때 컴퓨터는 문자형 -> 숫자형 -> 논리형 으로 저장된다. 
따라서 두번째 라인의 변수 type은 문자형이 나온다. => "character"

```r 
class(number_vector)
```
- 변수 type을 알아볼때는 class()를 사용한다.

## 범주형 변수의 순서
- Levels: 낮음 높음
```r
height_vector <- c("낮음", "높음", "낮음")
factor_height_vector <- factor(height_vector)
factor_height_vector
```
- Levels 순서 바꾸기 => 높음 낮음
```r
levels(factor_sex_vector) <- c("남성", "여성")
factor_sex_vector
```
- Levels 이름 변경 => Levels: 매우높음 매우낮음
```r
factor_height_vector <- factor(factor_height_vector,
                               levels = c("매우높음",
                                          "매우낮음"))
factor_height_vector
```

### 참조
- [https://github.com/dschloe/R_edu](https://github.com/dschloe/R_edu)