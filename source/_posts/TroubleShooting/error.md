---
title: "[Hexo] troubleshooting in Hexo"
date: 2021-03-29 21:23:23
tags: hexo
---
## 오류 상황
: npm을 통해서 설치를 완료하였는데도 아래와 같은 오류가 발생하였다. 
```
bash: hexo: command not found
```

## 해결 방안
- 우선 node와 npm이 제대로 설치되었는지 확인할 것
```
$ node -v
$ npm -v
```
![](/hueman_images/error/node.png)

- 정상적으로 설치되어 있다면, 다음 step을 따를 것
step 1) 자신의 blog 폴더 (ex. myblog) -> node_modules -> .bin
  경로를 복사함
![](/hueman_images/error/bin.png)
  
step 2) 시스템 속성 -> 고급 탭 -> 환경 변수
![](/hueman_images/error/systemsetting.png)

step 3) 시스템 변수에서 Path 클릭 -> 편집 클릭 -> 새로만들기 -> 경로 붙여넣기 -> 확인
![](/hueman_images/error/systemsetting2.png)

step 4) 다시 열어서 hexo server로 테스트해보기 
```
$ hexo server

INFO  Hexo is running at http://localhost:4000 . Press Ctrl+C to stop.
```
bash: hexo: command not found가 나오지 않는다면 해결!!

## 참조
- [https://www.programmersought.com/article/45443350618/](https://www.programmersought.com/article/45443350618/)