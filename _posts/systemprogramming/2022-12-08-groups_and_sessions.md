---
title:  "Process Groups, Sessions" 

categories:
  - System Programming
tags:
  - [process]

toc: true
toc_sticky: true

date: 2022-12-06
last_modified_at: 2022-12-06
---




<br/>
### Linux Process Hierarchy

프로세스는 전체적으로 tree 구조 (hierarchy)를 가진다. 

pid 0인 프로세스는 컴퓨터 부팅과 동시에 실행되며 모든 부트 시스템과 부트 로더를 실행한다. 그리고 pid가 1인 init 프로세스를 fork하는데, 이는 다른 모든 프로세스의 root 프로세스가 된다. 

![image](https://user-images.githubusercontent.com/86834982/206691743-e456d67f-e0a4-4d9d-820a-a86ba6f04c3b.png){: width="550px"}

`ps` : 현재 수행 중인 프로세스가 무엇인지 확인하는 명령어

`pstree` : 수행 중인 프로세스의 **트리 구조를 확인**하는 명령어

ex.  *pstree* 명령어를 실행한 결과

![image](https://user-images.githubusercontent.com/86834982/206691842-94360a91-ae12-42e5-bb1b-d7bd829712c5.png){: width="550px"}

-> 프로세스가 각자의 hierarchy를 가지고 실행되고 있음을 확인 가능   

<br/>
## Process Groups and Sessions

하나의 프로그램을 실행시키는데 프로세스들은 굉장히 많은데, 각각의 pid만으로 모든 프로세스들을 관리하는데는 한계가 있다. -> **group**과 **session**을 통해 관리

보통 로그인 쉘을 이용해서 하나의 세션에 들어왔다고 한다. 현재 실행되는 모든 프로세스들은 하나의 세션에서 실행되고, 이 세션 안에서는 같은 목적을 가진 프로세스들끼리 grouping 되어 각각 실행된다. 

![image](https://user-images.githubusercontent.com/86834982/206691939-8ef889c1-a8da-42e9-9b41-17c0a578cc59.png){: width="550px"}

<br/>
### **Process** **Groups**

같은 일(job)을 수행하고 있는 여러 개의 프로세스를 묶어놓은 것 

ex. pipe를 통해 여러 커맨드 수행하는 경우 

 `proc1 | proc2 &` : 프로세스 2개가 **하나의 목적**을 가지고 실행되는 것

-> 여기서 &는 background에서 프로세스를 실행하겠다는 명령어

**getpgrp(2), getpgid(2)**

각 프로세스 그룹은 고유한 group id(=pgid)를 부여받는데,

getpgrp는 현재 프로세스의 그룹 아이디를, getpgid는 입력한 pid의 그룹 아이디를 가져오는 함수이다. 

```c
#include <unistd.h>
pid_t getpgrp(void);  // 현재 실행하고 있는 프로세스의 pgid 리턴
pid_t getpgid(pid_t pid); // pid에 해당하는 프로세스의 pgid 리턴

// returns group-ID
// returns -1 on error (only getpgid(2)만)
```

- ***pid ==pgid*인 경우** : 그룹 내에서 leader 프로세스
    - 나머지 child 프로세스들을 모두 호출한 가장  parent 프로세스
    - leader 프로세스만 다른 child 프로세스나 새로운 프로세스 group 생성 가능

**setpgid(2)**

setpgid는 프로세스가 속한 그룹의 pgid를 바꿔주는 함수이다. 

```c
#include <unistd.h>
int setpgid(pid_t pid, pid_t, pgid);

// pid가 속한 group의 id를 pgid로 설정 
// returns 0 if OK, -1 on error 
```

- *pid == pgid* :  해당 프로세스를 leader 프로세스로 만들겠다
- *pid == 0* : pid를 정해주지 않은 경우, 함수를 호출한 프로세스의 pgid를 바꾸겠다
- *pgid == 0* : pgid를 정해주지 않은 경우, pid == pgid, 리더 프로세스로 만들겠다
- 프로세스는 자신과 child 프로세스의 pgid를 지정할 수 있지만, exec 함수를 사용해서 다른 프로그램을 돌리는 child의 pgid는 바꿀 수 없다 -> 한계!

<br/>
### **Sessions**

프로세스 group들의 집합. 프로세스 그룹이 일종의 작업이면, 세션은 **작업 공간**이다. 

setsid는 새로운 세션 아이디를 주는 명령어로, 새로운 sid와 pgid가 할당되어 실행된다. 기존 그룹에 속해있던 자식 프로세스가 새로운 세션을 만들어서 독립하기 위한 느낌으로 사용된다. 

```c
#include <unistd.h>
pid_t getsid(pid_t pid);
pid_t setsid(void);

// 새로운 세션을 만드는 함수, group leader가 호출할 수 없다
// returns process group-ID, -1 otherwise 
```

호출한 프로세스가 그룹의 leader가 아닌 경우, **새로운 세션을 생성**한다.

- 프로세스는 새로운 session leader가 된다
- 프로세스는 새로운 그룹의 group leader가 된다
- 프로세스는 controlling terminal이 없다 (독립된 deamon 프로세스가 됨)

ex. ps 명령어로 pgid, sid 확인하기 

`ps -o pid,ppid,pgid,sid,comm`  (추가적인 oflags를 부여)

![image](https://user-images.githubusercontent.com/86834982/206692018-bdfd176c-4f14-4475-b1f1-f874269113ec.png){: width="550px"}

<br/>
### Controlling Terminal

한 세션은 controlling terminal(ctty)을 포함할 수 있다. 현재 OS에서 어떤 프로그램들이 실행되고 있고, 어떤 프로세스가 background인지 foreground인지 컨트롤하는 역할을 한다. 

| Background Process (&) | Foreground Process |
| --- | --- |
| 눈에 보이지 않고 뒤에서 돌아가는 작업 | 실제로 눈에 보이는 실행 중인 작업 |
| 사용자 뒤쪽에서 실행되고 있음 | 사용자와 직접 상호 작용  |
| 프로세스가 끝날 때까지 기다리지 않음, 즉 돌아가는 동안 다른 프로세스 실행 가능  | 프로세스가 끝날 때까지 기다린 후 다음 프로세스 실행  |
| ex. 바이러스 백신, 방화벽 프로그램  | ex. 크롬창 실행, 유저와 직접 상호작용  |

![image](https://user-images.githubusercontent.com/86834982/206692115-76251996-f251-4697-9ff7-4925a924897c.png){: width="550px"}


<br/><br/>
@Advanced Programming in the UNIX environment, Third edition 내용을 참고함
<br/><br/>
[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}

