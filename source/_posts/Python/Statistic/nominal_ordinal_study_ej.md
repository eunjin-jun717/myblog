---
title: "[Statistic] Basic Data Types"
date: 2021-05-05 15:08:52
tags: statistic
---
# Basic Data Types
- Nominal Data(명목 자료)
- Ordinal Data(순서 자료)
- Interval Data(구간 자료)
- Ratio Data(비율 자료)
- Discrete and Continuous (이산형, 연속형)
![](/hueman_images/statistics/datatypes.png)

## 1. Nominal Data
- nominal(Named)이란 여러 catergories 들 중 하나의 이름에 데이터를 분류할 수 있을 때 사용된다. 
      ex) 청팀, 홍팀, 백팀
- 순서를 매길 수 없고, 셀 수만 있다. 
- 수학적인 계산을 하는 것은 의미가 없다. 그러나 percentage로는 표현해도 된다. 
      ex) 청팀(33%), 홍팀(40%), 백팀(27%)
- nominal data가 두개의 범주 중 하나에 속하는 경우에는 Dichotomous data(이분 자료)라고도 한다. 
      ex) gender(female, male)

## 2. Ordinal Data
- 데이터가 속하는 categories에 **순서가 있는 경우** ordinal data라 한다. 
- 즉, 순서가 있는 명목형 자료
      ex) 만족도: 1(낮음)/ 2(보통)/ 3(높음)
- nominal data 처럼 세는 것은 가능하고, percentage로 표현도 가능하다. 
- 단, 평균에 대해서는 신중해야한다. 위의 예시와 같이 만족도의 1,2,3에 대한 숫자에 수학적/과학적 의미가 있지 않기 때문이다. 

## 3. Interval Data
- 데이터의 연속된 측정 구간 사이의 간격이 동일한 경우 Interval data라고 부른다.
      ex) 온도, 시간
- Interval data는 Numeric value를 가지므로 다양한 연산이 수행 가능하다.
      평균, 중앙값 등을 이용하여 척도의 중심 경향 계산 가능
- 단점: 미리 결정된 시작점이나 절대적 원점(zero point)이 없는 것
      00:00 이라는 자료의 값이 측정한 시간의 값이 없다는 것이 아니라 그냥 자정에 시간을 측정했다는 것!
- Ordinal data의 속성들을 모두 포함한다. 



## 4. Ratio Data
- 비율척도라 한다. 
      ex) 나이, 돈, 몸무게
- 현재 시각이 10시인데 시계를 보고 10시부터 10시 30분까지 계산해서 30분 기다렸네 하면 이것이 ratio data이다. 
- Interval data와 다르게 절대적 원점(zero point)가 존재한다. 
      00:00 이라는 값은 기다린 시간이 0초라는 것이다. 

## 5. Discrete, Continuous
- Interval, ratio 자료는 discrete이나 continuous 둘 중 하나의 속성을 가진다. 
- discrete: 측정값이 정수로 떨어지는 것
      ex) 사탕 갯수(1개,2개,3개,...)
- continuous: 측정값이 연속된 무수히 많은 값으로 이루어진 것
      ex) 몸무게(72.6kg)
- '나이'라는 데이터는 본질적으로는 Ratio data이지만 Ordinal data로도 수집될 수 있다.
      ex) 나이가 속한 그룹: 21~29, 31~39 등
- 반면, nominal이나 ordinal(둘다 categorical data)은 interval이나 ratio data로 수집될 수 없다. 
      ex) 청팀, 홍팀, 백팀을 interval, ratio로 수집할 수 없음
      - 보편적으로 얘기하면, 데이터 측정은 주어진 데이터의 본질적 속성보다 더 낮은 수준으로 내려갈수는 있어도 더 정교한 수준으로 올라갈 수는 없다. 
      - 다시 말해, Interval/ratio -> nominal/ordinal (O)
      -            nominal/ordinal -> Interval/ratio (X)

## 6. Summary of Data types
![](/hueman_images/statistics/summary_datatypes.png)

*references*
- [http://blog.heartcount.io/dd](http://blog.heartcount.io/dd)
- [nominal, ordinal, interval, ration data types](https://www.questionpro.com/blog/nominal-ordinal-interval-ratio/#:~:text=Nominal%2C%20Ordinal%2C%20Interval%2C%20and,being%20a%20multiple%20choice%20question.&text=Nominal%20scale%20is%20a%20naming,labeled%2C%20with%20no%20specific%20order.)

