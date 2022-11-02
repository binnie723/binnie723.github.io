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


블로그에 글을 포스팅하려면 우선 올릴 내용을 markdown 파일로 만들어주고, 또 형식을 이것저것 몇개 추가해주어야 한다. 

<br/><br/> 
### 0. 노션으로 마크다운 파일 생성하기

블로그에 포스팅할 때는 마크다운 파일로 만들어서 업로드해야 하는데, 노션에는 정리한 글을 markdown으로 자동 변환해주는 기능이 있다. 

<br/> 오른쪽 메뉴(...)를 보면 내보내기가 있는데

![image](https://user-images.githubusercontent.com/86834982/182108474-b503b526-2ab0-4cdb-abc7-8a438901b84c.png){: width="80%" height="80%"}

<br/> Mark & CSV로 내보내기해서 저장하면 끝 

![image](https://user-images.githubusercontent.com/86834982/182108243-0aff1dbf-dd0a-443e-ac88-fe7644c93dba.png){: width="80%" height="80%"}  

💡 Notion에서 이미지 파일이 변환 형식만 맞지 않는데, 따로 바꿔서 넣어주어야 한다. (다음 글에 정리할 예정)
{: .notice--primary} 


<br/><br/> 
### 1. 내 블로그 폴더에 _posts 폴더 생성하기

이제 만든 마크다운 파일을 블로그 폴더에 옮겨주어야 한다. 편의를 위해 포스팅들을 모아둘 _post 폴더를 따로 만들어준다. (이미 있으면 생략하기)

![image](https://user-images.githubusercontent.com/86834982/182108024-8acd25e1-228b-4ce7-8585-d3490b25f0a1.png){: width="80%" height="80%"}

<br/><br/> 
### 2. **yyyy-mm-dd-title.md**로 파일 이름 변경 

파일 이름은 아래와 같이 날짜 - 제목으로 바꿔서 설정해준다.

ex. 2022-07-29-Github-블로그-만들기-(with-Jekyll).md

<br/><br/> 
### 3. 마크다운 파일 상단에 포스트 정보 추가하기
파일에 따로 추가해줘야 하는 내용이 있는데 아래 내용을 잘 바꿔서 복붙하면 된다. 

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
### 4. 파일을 _post 폴더에 넣기

아까 생성한 `_posts` 폴더에 작성한 마크다운 파일을 옮겨주면 된다.

<br/>
### 5. Git push 해서 포스팅 내용 서버에 반영하기

터미널에 아래와 같이 입력해서 변경 사항들을 전부 push하면 사이트에 글이 올라간 모습을 확인할 수 있다.


`git add .` : 변경 사항을 전부 staging area에 올리기

`git commit -m “commit message”` : staging area에 올라온 모든 파일을 원격 서버로 올릴 준비, 확정 짓는 과정

`git push origin master` : 커밋한 모든 변경 사항들을 원격 Github 서버에 반영

 💁🏻‍♀️ 이제 내 블로그 사이트 :  [https://binnie723.github.io](https://binnie723.github.io) 에 들어가면 적용된 모습을 확인해볼 수 있다!
   
<br/> <br/> 


<br/><br/>
[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}
