---
title:  "블로그 포스팅하는 방법 (with Notion)" 

categories:
  - Blog
tags:
  - [Github, Notion, minimal-mistake]

toc: true
toc_sticky: true

date: 2022-08-01
last_modified_at: 2022-08-01
---

<br/> 
이번에는 블로그에 포스팅하는 방법과 사용되는 간단한 마크다운 문법들에 대해 정리해보려고 한다. 사실 내가 적어두고 내가 참고하려고 만들어두는 글 😗 
<br/><br/> 

## 블로그에 글을 포스팅하는 방법
---

Github 블로그에 글을 작성하는 방법은 간단하다. 
<br/><br/> 
### 1. 내 블로그 repository에 `_posts` 폴더 생성

만약 이미 폴더가 있다면 이 과정은 생략하면 된다. 이 폴더는 앞으로 새로운 블로그 포스팅 글들을 모아두는 역할을 할 예정이다. 

![image](https://user-images.githubusercontent.com/86834982/182108024-8acd25e1-228b-4ce7-8585-d3490b25f0a1.png){: width="80%" height="80%"}
<br/><br/> 
### 2. **yyyy-mm-dd-title.md 형식**의 마크다운 파일 생성

작성하고자 하는 포스트 내용을 마크다운 형식으로 작성해서 저장해야 한다. 마크다운 파일 만드는 방법은 아래 더 자세하게 설명해두었다. 

ex. 2022-07-29-Github-블로그-만들기-(with-Jekyll).md

💡 마크다운 파일 맨 위쪽에 다음과 같은 블로그 포스트 정보를 추가해주어야 한다.
{: .notice--primary} 

```markdown
---
title:  "블로그 포스팅하는 방법 (with Notion)"
excerpt: "여기는 소제목 작성하는 공간"

categories:
  - Blog
tags:
  - [Blog, Notion, Github, Git]

toc: true          # Table of Content 목차를 추가해줄 것인지 
toc_sticky: true   # 스크롤바에 따라 내려가는 목차 
 
date: 2022-07-29
last_modified_at: 2022-07-29
---
```

  
<br/>
### 3. 파일을 내 블로그 repository에 있는 `_post` 폴더에 옮기기

아까 생성한 `_posts` 폴더에 작성한 마크다운 파일을 옮겨주면 된다. 애초에 먼저 파일을 여기에 만든 경우에는 이 단계를 생략하면 된다.   
<br/>
### 4. Github Pages 서버에 push 해서 글을 최종 업로드 하기

다음과 같이 터미널에 명령어를 입력해서 변경 사항들을 push하면 글이 최종 업로드 된다. 

`git add .` : 변경 사항을 전부 staging area에 업로드

`git commit -m “commit message”` : staging area에 모든 파일을 원격 서버로 올릴 준비 및 확정

`git push origin master` : 커밋한 모든 변경 사항들을 내 원격 Github 서버에 반영

 💁🏻‍♀️ 이제 내 블로그 사이트인  [https://binnie723.github.io](https://binnie723.github.io)에 접속하면 작성한 글이 업로드 된 것을 확인할 수 있다.   

  
<br/><br/> 
## Markdown 파일 간단하게 생성하는 방법 (with Notion)
---

마크다운 파일을 생성하는 방법에는 여러 가지가 있다. 주로 Visual Studio code나 다른 마크다운 에디터를 많이 사용하는 것 같지만 나는 예전부터 계속 사용하던 Notion으로 글을 작성한 후, markdown 형식으로 export 해서 사용하고 있다.    
<br/> 
### 1. 오른쪽 메뉴(···)에 있는 내보내기를 클릭한다

![image](https://user-images.githubusercontent.com/86834982/182108474-b503b526-2ab0-4cdb-abc7-8a438901b84c.png){: width="80%" height="80%"}

  
<br/> 
### 2. 형식을 markdown으로 지정하고 내보내기를 누른다.

![image](https://user-images.githubusercontent.com/86834982/182108243-0aff1dbf-dd0a-443e-ac88-fe7644c93dba.png){: width="80%" height="80%"}

   
<br/> 
### 3. 로컬 폴더에 자동적으로 변환된 마크다운 파일이 다운로드 된다.

![image](https://user-images.githubusercontent.com/86834982/182108352-ed1195de-7180-4307-b824-860766dab793.png){: width="80%" height="80%"}
<br/>
💡 단, Notion에서 이미지 파일이 변환 형식이 맞지 않는데, 이미지 부분만 따로 바꾸어 설정해주어야 한다.
{: .notice--primary} 

  
<br/> 
이렇게 Notion을 사용하면 비교적으로 더 쉽게 마크다운 파일로 생성해서 업로드할 수 있다. Notion을 사용하지 않고 다른 에디터를 사용하려고 하는 경우에는 Visual Studio Code 나 Atom을 사용한 마크다운 파일 작성법을 찾아보면 좋을 것 같다.

<br/> <br/> 
