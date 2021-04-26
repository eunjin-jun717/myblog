---
title: "[R] Encoding error in R"
date: 2021-03-30 21:06:12
tags: hexo
---

## 오류 상황
![encoding error](/hueman_images/error/encoding_error.png)
```
Not all characters in C:/Users/study/Desktop/analysis/logistic_hd.R could be decoded using CP949. 
To try a different encoding, choose "File | Reopen with Encoding..." from the main menu.
```
- R 파일을 여는데 위와 같은 오류문구가 발생하면서 글자가 거의 모두 깨져있었다. 

## 해결방안
- File -> Reopen with encoding -> UTF-8로 설정
![encoding error](/hueman_images/error/encoding_set.png)
- 설정을 완료하면 아래와 같이 해결된 것을 볼 수 있다.
![encoding error](/hueman_images/error/encoding_set2.png)

## 오류가 난 이유
- Mac OS나 Linux에서의 UTF-8로 인코딩되어 있는 상태에서 자료가 Windows로 넘어올 때 글자가 깨지는 현상이 발생할 수도 있다.


## 참조
- [https://nittaku.tistory.com/341](https://nittaku.tistory.com/341)