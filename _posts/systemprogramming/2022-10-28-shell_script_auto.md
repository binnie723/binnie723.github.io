---
title:  "자동으로 서버에 접속하는 shell script 만들기" 

categories:
  - System Programming
tags:
  - [Linux, Unix, shell]

toc: true
toc_sticky: true

date: 2022-10-28
last_modified_at: 2022-10-28
---

<br/>
매주 실습 시간마다 학교 리눅스 서버에 접속해서 문제를 풀어야 하는데 매번 명령어 찾아서 치고 비밀번호도 따로 입력해야 하는게 귀찮아서 그냥 이걸 쉘로 자동으로 접속하게 하면 어떨까 해서 찾아봤다 

-> 찾아보다가 발견한 **sshpass !**


<br/>
### sshpass

“non-interactive ssh password provider” 

말 그대로 미리 입력 받은 암호를 통해 바로 자동으로 ssh 연결을 가능하게 하는 명령어 

서버 접속 뿐만 아니라 접속 후에 연결된 계정으로 명령어를 실행하는 것도 가능하다.
<br/>

**먼저** **sshpass 패키지 설치**

💡 Mac OS m1 기준 !
{: .notice--primary} 
`brew install ssh` 

`brew install sshpass` 로 해당 명령어 패키지를 먼저 다운 받고 

<br/>
**sshpass을 사용해서 자동 로그인** 

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
*in.sh* 실행한 결과

![image](%E1%84%8C%E1%85%A1%E1%84%83%E1%85%A9%E1%86%BC%E1%84%8B%E1%85%B3%E1%84%85%E1%85%A9%20%E1%84%89%E1%85%A5%E1%84%87%E1%85%A5%E1%84%8B%E1%85%A6%20%E1%84%8C%E1%85%A5%E1%86%B8%E1%84%89%E1%85%A9%E1%86%A8%E1%84%92%E1%85%A1%E1%84%82%E1%85%B3%E1%86%AB%20shell%20script%20%E1%84%86%E1%85%A1%E1%86%AB%E1%84%83%E1%85%B3%E1%86%AF%E1%84%80%E1%85%B5%202b52901a980e46e28fac6b614e61884a/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-10-22_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB_11.40.47.png)
<br/>
학교 서버에 들어와진다 !! 😃


<br/><br/>
[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}
