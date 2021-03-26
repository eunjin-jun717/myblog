---
title: How to create a blog with Hexo
date: 2021-03-22 21:38:36
tags: hexo
---
## github에 2개의 Repository 생성
- 포스트 버전관리: user_name 
- 포스트 배포용 관리: user_name.github.io

## Blog 초기화 
$ hexo init myblog

## Hexo 모듈 설치
$ npm install -g hexo-cli

## myblog로 들어간 뒤 아래와 같이 입력
$ cd myblog
$ npm install
$ npm install hexo-server --save
$ npm install hexo-deployer-git --save
- 참고로, 복사+붙여넣기 하면 오류를 줄일 수 있음

## Local server로 테스트
$ hexo server

## Pycharm
1) pycharm을 열어서 myblog 폴더를 연다.
2) config.yml 파일을 열어 title, author을 수정한다.
title: Eunjin's Blog
author: Eunjin Jun
3) URL도 아래와 같이 수정한다.
- url: https://user_name.github.io
4) Deployment의 deploy를 아래와 같이 입력한다.
deploy:
  type: git
  repo: https://github.com/user_name/user_name.github.io.git
  branch: main
   
## Hexo generate & deploy
- 활성화시킨 뒤 배포한다. 
$ hexo generate 
$ hexo deploy
- 한꺼번에 명령을 할 수도 있다.
$ hexo deploy --generate
  
## Themes 설치
ex) icarus 설치
1) icarus 설치
$ npm install hexo-theme-icarus
   
2) config.yml의 Extensions의 theme을 icarus로 변경
theme: icarus
   
3) $ hexo server
이때, Error에서 뜨는 설명대로 그대로 복사한 뒤 붙여넣기하여 설치하기
$ npm install --save bulma-stylus@0.8.0 hexo-renderer-inferno@^0.1.3

4) 다시 local server 테스트
$ hexo server
$ hexo deploy --generate

![](/hueman_images/blog_image.png)

### 참조
- [https://dschloe.github.io/settings/hexo_blog/](https://dschloe.github.io/settings/hexo_blog/)