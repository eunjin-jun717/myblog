---
title: "[Hexo] 블로그 꾸미기"
date: 2021-06-08
tags: hexo
---
# [HEXO] 블로그 꾸미기
- 이전에 HEXO로 블로그 생성하는 법과 "Hueman" 테마 적용하는 글을 올렸었습니다. 아래의 링크 참고 부탁드립니다 🙏\
👉 [hexo blog 시작하기](https://eunjin-jun717.github.io/2021/03/22/R/firstday/)

## 1. 썸네일(Thumbnail)과 Theme color 변경
![](/hueman_images/hexo/thumbnail1.png)
step 1) themes -> hueman -> source -> css -> images 폴더로 들어가서 올리고 싶은 이미지 파일을 업로드 합니다. 
![](/hueman_images/hexo/blog1.png)

step 2) themes -> hueman -> _config.yml 파일에 들어가서 기존에 Hueman 이미지 파일의 이름을 지우고 "eunjin.png(자신이 원하는 파일)"을 적어줍니다.
![](/hueman_images/hexo/blog2.png)
![](/hueman_images/hexo/blog3.png)



## 2. Theme color 변경
- themes -> hueman -> _config.yml 파일에서의 Customize의 "theme_color"를 찾습니다. 
![](/hueman_images/hexo/thumbnail3.png)
- 원하는 색상을 찾아서 넣어주면 됩니다.
- 참고로, 구글에 color picker 이라고 검색하면 아래와 같이 원하는 색상표를 뽑아낼 수 있습니다!
![](/hueman_images/hexo/colorpicker.png)

## 3. 사이드 썸네일
![](/hueman_images/hexo/thumbnail2.png)
step 1) themes -> hueman -> layout -> common -> post -> header.ejs 파일을 찾습니다.  
![](/hueman_images/hexo/side.png) 

step 2) 아래의 그림과 같이 원하는 text를 넣고 그 외에는 그림과 똑같이 만들기 위해 다른 text는 삭제합니다.  
![](/hueman_images/hexo/side2.png) 

## 4. Category 설정 방법
![](/hueman_images/hexo/thumbnail4.png)
step 1) themes -> hueman -> languages -> en.yml 파일을 찾습니다. 
![](/hueman_images/hexo/cat.png)

step 2) 아래와 같이 원하는 category를 추가해줍니다.\
저의 경우 Python, DL 등을 추가하였습니다. 
![](/hueman_images/hexo/cat2.png)

step 3) themes -> hueman -> _config.yml 파일에 들어가서 Menu를 찾은 뒤 그 안에 원하는 카테고리를 입력합니다. 하위 카테고리를 만들고 싶으면 상위 카테고리 밑에 tab을 누르고 입력하면 됩니다. 
![](/hueman_images/hexo/blog2.png)
![](/hueman_images/hexo/cat3.png)

