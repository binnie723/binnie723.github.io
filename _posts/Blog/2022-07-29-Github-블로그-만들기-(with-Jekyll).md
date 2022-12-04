---
title:  "내 Github 블로그 만들기" 

categories:
  - Blog
tags:
  - [Github, Jekyll, minimal-mistake]

toc: true
toc_sticky: true

date: 2022-07-29
last_modified_at: 2022-07-29
---


<br/> 
이번 카테고리에서는 Jekyll 테마를 사용해서 내 Github 블로그 만드는 과정을 정리해보려고 한다. 계속 이것저것 업데이트 해보면서 블로그를 꾸며볼 예정 !!
<br/><br/> 


### Jekyll
---

Jekyll(지킬)은 Github 설립자 중 한 명이 개발한 프레임 워크로, 마크업 언어(.md)로 작성된 파일을 정적인 웹 페이지로 변환해주는 툴이다. Jekyll로 편리하게 웹 페이지를 작성해서 만들 수 있고, 사용자가 모든 페이지를 직접 커스터마이징할 수 있는 장점이 있다. 그래서 이걸 기반으로 깃허브 블로그 만들어보기로 했다.

<br/> 

### 1. 블로그용으로 사용할 새로운 Repository 생성

먼저 Github에 로그인해서 내 블로그로 사용할 새로운 Repository를 생성한다.  

💡 새로운 Repository의 이름은 **반드시!** 다음과 같이 설정   
{: .notice--primary}  

![image](https://user-images.githubusercontent.com/86834982/181670337-1f5b145f-1f10-4362-b977-202d0359022f.png){: width="520px"}

<br/> 
### 2. 생성한 Repository를 내 컴퓨터 로컬 환경으로 clone 하기

터미널에서 git clone으로 생성한 레포지토리를 컴퓨터 로컬 환경으로 복사해온다.

`git clone https://github.com/binnie723/binnie723.github.io.git`

내 블로그 Repository의 주소 :  **Code** -> **HTTPS** 아래에 적혀있는 url 참고

![image](https://user-images.githubusercontent.com/86834982/181670802-9bc91f66-18f9-4afa-a75f-66f66a0a884a.png){: width="550px"}

다음과 같이 내 컴퓨터에 블로그 Repository 폴더가 생성된다.  
-> 이제 내 컴퓨터가 Github와 원격으로 연결되어 원하는 작업 수행 가능 

![image](https://user-images.githubusercontent.com/86834982/181670860-8937ff5c-aaf9-4996-ba53-3a134851b374.png){: width="520px"}




<br/> 
### 3. Ruby, Jekyll, Bundler 설치

이제 기본적인 블로그 뼈대는 만들었고 필요한 패키지를 다운 받아야 한다.

Jekyll은 Ruby 언어로 구성되어 있기 때문에 먼저 Ruby를 설치해주어야 하고, 그리고 Ruby 관리에 필요한 bundler까지 추가로 설치해주어야 한다. 


💡 Mac OS m1 기준
{: .notice--primary} 
먼저 brew 명령어를 사용해서 **rbenv**를 설치
```
brew update
brew install rbenv ruby-build
```
<br/> 그리고 rbenv로 원하는 버전의 ruby를 다운 받고, 설치한 ruby를 global 버전으로 적용
```
rbenv install 2.6.10
rbenv global 2.6.10
```
<br/> 그리고 rbenv PATH 환경 변수를 설정, 먼저 쉘 설정 파일 들어가서
```
vim ~/.bash_profile 
```
<br/> 다음 코드를 추가하고, source 명령어로 적용해주기
```
[[ -d ~/.rbenv  ]] && \
  export PATH=${HOME}/.rbenv/bin:${PATH} && \
  eval "$(rbenv init -)"
```

```
source ~/.zshrc
```
<br/> 마지막으로 gem 명령어를 사용해서 **jekyll**과 **bundler**를 설치하면 끝
```
gem install jekyll bundler
```
<br/> 더 자세한 설명은 아래 사이트 참고하기 

🔍  **Jekyll 공식 문서 : [https://jekyllrb.com/docs/installation/macos/](https://jekyllrb.com/docs/installation/macos/)**


<br/> 
<br/> 
### 4. Jekyll 테마 파일을 다운 받아서 내 블로그 폴더에 복사해오기

이제 블로그에 적용할 테마를 아래 사이트에서 아무거나 하나 골라서 zip 파일로 다운 받으면 된다! (내가 고른건 minimal mistakes 테마)

🔍  **Jekyll 테마 사이트 : [http://themes.jekyllrc.org/](http://jekyllthemes.org/)**

![image](https://user-images.githubusercontent.com/86834982/181670910-33f009e1-d461-4d7d-be8b-709b472da56b.png){: width="580px"}

다운 받은 zip 파일을 풀고 그 안에 내용물을 **전부** 복사해서 내 블로그 폴더 [`binnie723.github.io`](http://binnie723.github.io) 에 붙여넣어주면 된다. 

![image](https://user-images.githubusercontent.com/86834982/181670954-3701248d-a1b8-4fc9-951b-9185b5d17555.png){: width="580px"}


<br/> 
### 5. Git push 해서 테마를 최종 반영하기

터미널에 아래와 같이 입력해서 변경 사항들을 전부 push하면 사이트에 글이 올라간 것을 확인할 수 있다.


`git add .` : 변경 사항을 전부 staging area에 올리기

`git commit -m “commit message”` : staging area에 올라온 모든 파일을 원격 서버로 올릴 준비 과정

`git push origin master` : 커밋한 모든 변경 사항들을 원격 Github 서버에 반영

 💁🏻‍♀️ 이제 내 블로그 사이트 :  [https://binnie723.github.io](https://binnie723.github.io) 에 들어가면 적용된 모습을 확인해볼 수 있다!
 

<br/><br/>
[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}
