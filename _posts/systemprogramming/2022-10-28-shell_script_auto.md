---
title:  "[추가] 자동으로 서버에 접속하는 shell script 만들기" 

categories:
  - System Programming
tags:
  - [shell]

toc: true
toc_sticky: true

date: 2022-10-28
last_modified_at: 2022-10-28
---

<br/>
매주 실습 시간마다 학교 리눅스 서버에 접속해서 문제를 풀어야 하는데 매번 명령어 찾고 비밀번호 입력해서 들어가는게 귀찮아서 뭐가 없을까 찾아보다가 발견한 **sshpass !**

<br/>
### sshpass

“non-interactive ssh password provider” 

말 그대로 미리 입력 받은 암호를 통해 바로 자동으로 ssh 연결을 가능하게 하는 명령어 

서버 접속 뿐만 아니라 접속 후에 연결된 계정으로 명령어를 실행하는 것도 가능하다.
<br/>

<br/>
### 1. 먼저 sshpass 패키지 설치

💡 Mac OS m1 기준 !
{: .notice--primary} 
`brew install ssh` 

`brew install sshpass` 로 해당 명령어 패키지를 먼저 다운 받고 
<br/>

<br/>
### 2. sshpass을 사용해서 자동 로그인

password에 비밀번호를 미리 입력하고 기존 ssh로 접속하는 명령어 쓰면 끝 

```bash
sshpass -p ‘password’ ssh ‘아이디’@’아이피’
```

<br/>
*in.sh* 우리 학교 서버로 적용해본 코드

```bash
#!/bin/bash
sshpass -p password ssh -Y s20205108@cse.gist.ac.kr
```
<br/>
<br/>
### 실행 결과
<br/>
![image](https://user-images.githubusercontent.com/86834982/199553895-03b8e887-22da-48c8-88cf-8aa5d69f68bb.png){: width="550px"}
<br/>
학교 서버에 들어와진다 !! 😃


<br/><br/>
[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}
