---
title: "[Github] Daily Commit 시간"
date: 2021-06-09 11:00:00
tags: github
---
*references*
- [https://velog.io/@_uchanlee/](https://velog.io/@_uchanlee/Github-%ED%94%84%EB%A1%9C%ED%95%84%EC%97%90-%EB%82%98%EC%9D%98-Daliy-%EC%BD%94%EB%94%A9-%EC%8B%9C%EA%B0%84%EC%9D%84-%EC%A0%81%EC%9A%A9%ED%95%B4%EB%B3%B4%EC%9E%90)

# [Github] Daily 코딩 시간 나타내는 GIST 만드는 법
> Github Gist는 코드조각(Code Snippet), 로그, 메모 등을 남기는데 사용합니다.
- Github Gist 사이트: [https://gist.github.com/](https://gist.github.com/)


## 1. 아래의 링크에 접속하여 repository를 Fork 한다. 
👉 [https://github.com/techinpark/productive-box](https://github.com/techinpark/productive-box)
- 아래의 그림과 같이 TIMEZONE이 Asia/Seoul 인 것을 확인한다. 
![](/hueman_images/github/gist1.png)



## 2. Github Gist에 들어가서 Public Gist를 생성한다.
👉 [https://gist.github.com/](https://gist.github.com/)
- 예를 들어 아래의 내용과 같이 적고, public 으로 변경한 뒤 생성한다. 
![](/hueman_images/github/gist2.png)
- 생성 완료된 모습
![](/hueman_images/github/gist3.png)
- gist가 생성되고 그 페이지의 주소를 보면 아래와 같이 나오는데 이 주소의 뒷부분을 복사해둔다! 
> *8760d3541faa1dcb4e7e89a77163f94f*

```
https://gist.github.com/eunjin-jun717/8760d3541faa1dcb4e7e89a77163f94f
```


## 3. Github Token을 Repo와 Gist를 선택한 뒤 생성한다. 
👉 [https://github.com/settings/tokens/new](https://github.com/settings/tokens/new)
![](/hueman_images/github/gist4.png)
- 토큰이 생성되면 **무조건 복사** 해놓는다!
![](/hueman_images/github/token.png)


## 4. 이전에 fork한 자신의 repository로 가서 Settings -> Secrets 에서 "New Secret"을 눌러 환경변수를 세팅해준다. 
![](/hueman_images/github/secret.png)
- 1) Name: **GH_TOKEN** // Value: 이전에 복사해둔 TOKEN 붙여넣기 -> Add Secret 
![](/hueman_images/github/ghtoken.png)
- 2) Name: **GIST_ID** // Value: 이전에 복사해둔 Gist 주소 마지막 부분 복사한거 붙여넣기 
![](/hueman_images/github/gistid.png)
- 두개의 token이 생성된 것을 확인할 수 있다. 
![](/hueman_images/github/token2.png)


## 5. 설정은 완료하였고, Fork 했던 repository의 Action 탭에서 활성화 한다. 
![](/hueman_images/github/action.png)
- 그리고 Action tab의 Update gist를 클릭하고 Enable workflow 를 클릭해준다.
![](/hueman_images/github/enable.png) 
- 매 정각마다 업데이트 될 것이지만 결과를 확인하기 위해 fork한 project에 아무거나 push해서 업데이트를 주도록 한다. 
- 예를 들어, Readme.md 파일에 공백을 주어 업데이트한다.
![](/hueman_images/github/update.png) 
- 처음에 만들어 놓은 temp gist에 들어가보면 아래와 같이 설정해놓은 이름이 바뀌면서 정상 작동하는 것을 확인할 수 있다. 
![](/hueman_images/github/result.png) 



## 6. 마지막으로 Github 프로필에 Pin 해놓으면 완성!
- customize your pins 에 들어가서 Pin 한다. 
![](/hueman_images/github/pin1.png)
![](/hueman_images/github/pin.png)
- 마지막으로 Pin된 나의 commit 시간대를 확인한다!
![](/hueman_images/github/result2.png)

내가 어느시간대에 주로 commit을 많이하는지 한 눈에 확인할 수 있어서 좋았다. 
참고로 moring, daytime, evening, night의 시간대는 아래와 같습니다 👇

      Moring: AM 6:00 ~ PM 12:00
      Daytime: PM 12:00 ~ PM 6:00
      Evening: PM 6:00 ~ AM 12:00
      Night: AM 12:00 ~ AM 6:00
