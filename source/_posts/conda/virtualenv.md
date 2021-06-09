---
title: "[virtual enviroments] Anaconda 가상환경"
date: 2021-06-09 10:00:00
tags: conda
---
*references*
- [https://yganalyst.github.io/pythonic/anaconda_env_1/](https://yganalyst.github.io/pythonic/anaconda_env_1/)
- [https://chancoding.tistory.com/85](https://chancoding.tistory.com/85#:~:text=%EB%98%90%ED%95%9C%2C%20%EB%8B%A4%EB%A5%B8%20%EC%BB%B4%ED%93%A8%ED%84%B0%20%ED%98%B9%EC%9D%80%20%EB%8B%A4%EB%A5%B8,%EB%A5%BC%20%EB%B0%A9%EC%A7%80%ED%95%A0%20%EC%88%98%20%EC%9E%88%EC%8A%B5%EB%8B%88%EB%8B%A4.)

# [Virtual Environments] 가상환경


## 1. 가상환경이란?
> 사용자가 컴퓨팅 환경과 다른 사용자의 작업 모두와 상호작용할 수 있는 네트워크 응용 프로그램이다. 

> 즉, 독립적인 환경에서 패키지 및 버전관리를 하기 위한 가상의 환경이다. 


## 2. 가상환경이 필요한 이유는?

- 예를 들어, 하나의 환경인 Base를 기준으로 필요한 library들을 설치하다보면, 경고문구나 각 라이브러리들의 의존성 문제에 의해 여러 오류들을 접할 수 있다. 
- 따라서 하나의 프로젝트를 할 때 마다 다른 가상환경을 만들어 충돌을 줄이자는 의도에서 시작한 것으로 생각된다.

## 3. 아나콘다 가상환경
- 가상환경은 ```virtualenv```와 ```conda```를 사용한다. 
- 참고로 windows 10 기준으로 설명할 것이다. 




### 3-1. 가상환경 만들기 
- Anaconda에서의 가상환경을 만들기 위해 아래와 같이 입력할 수 있다.
- 특정 python 버전도 설치 가능하다.  
```
conda create -n 가상환경이름
conda create -n 가상환경 이름 python=(특정 버전)
```
- 가상환경을 활성화 한다. 
```
conda activate 가상환경이름
activate 가상환경이름
```

### 3-2. 가상환경 리스트 
- 아래의 명령어를 사용하여 가상환경을 나타낼 수 있다. 
```
conda env list
```
![](/hueman_images/conda/conda1.png)
- 가상환경을 비활성화 시킬 수도 있다.
```
conda deactivate 가상환경이름
deactivate 가상환경이름
deactivate
```
- 가상환경 삭제도 가능하다. 
```
conda env remove -n 가상환경이름
```

### 3-3. 가상환경에 라이브러리 설치
- 필요한 라이브러리를 가상환경에 설치할 수 있다. 
![](/hueman_images/conda/conda2.png)
- 설치된 라이브러리를 확인한다. 
```
conda list
```
![](/hueman_images/conda/conda3.png)

### 3-4. 라이브러리(패키지) 관리
```
pip freeze > requirements.txt
```
- 가상환경을 활성화 시킨 상태에서 위의 명령어를 실행한다. 
- requirements.txt에 패키지의 목록과 버전 정보들이 존재한다. 

- 만약, 새로운 가상환경에 동일한 라이브러리를 구성해야된다고 가정해보면 아래의 명령을 사용해 새로운 가상환경에서도 requirements.txt의 내용대로 패키지를 설치할 수 있다. 
```
pip install -r requirements.txt
```
- 삭제도 가능하다.
```
pip uninstall -r requirements.txt
```

### 3-5. 가상환경 복사하기 
- 기존 가상환경과 똑같은 환경으로 복사할 수도 있다. 
```
conda create -n 복사된것 --clone 복사할것
```
