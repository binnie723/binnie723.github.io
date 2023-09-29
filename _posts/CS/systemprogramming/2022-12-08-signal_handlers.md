---
title:  "Signal Handlers" 

categories:
  - System Programming
tags:
  - [signal]

toc: true
toc_sticky: true
use_math: true

date: 2022-12-08
last_modified_at: 2022-12-08
---

<br/>
Signal handler는 전달받은 시그널에 대해서 기본적인 default 동작이 아닌, 새로 등록한 함수의 동작을 수행하도록 바꿔준다. 

<br/>
### Installing Signal Handler : signal(3)

시그널 핸들러는 signal 함수를 통해 등록할 수 있다. 사용자가 직접 정의한 함수를 인자로 넣어서 대신 수행하도록 해야 한다. 

```c
#include <signal.h>

sig_t signal(int sig, sig_t func);
void (*signal(int sig, void (*func)(int)))(int);
// returns: previous signal handler, SIG_ERR otherwise
```

- SIG_IGN : 함수 대신 전달한 경우, 해당 신호를 무시한다
- SIG_DFL : 함수 대신 전달한 경우, 기존의 default handler를 수행한다

ex1. SIGINT를 받고, 3초 뒤에 종료시키는 핸들러

![image](https://user-images.githubusercontent.com/86834982/206746381-bd4e89ce-0189-4fed-8c3b-203e1adaa81e.png){: width="530px"}

ex2. sleep(3)을 alarm 핸들러와 pause 함수로 구현한 코드

-> “Hello” 출력하고 3초 슬립(alarm + pause)한 뒤에 “I am Seungbin!” 출력

```c
void alarm_handler(int signo){
		// do nothing !
}

int main(num){
	signal(SIGALRM, alarm_handler);

	printf("Hello\n");
	alarm(3);
	pause(); // sleep(3)
	printf("I am Seungbin!\n");
	return 0;
}
```

<br/><br/>
### Blocking/Unblocking Signals : sigprocmask(2)

sigprocmask를 사용해서 특정 시그널을 완전히 blocking 시킬 수 있다. 

```c
#include <signal.h>

int sigprocmask(int how, const sigset_t *restrict set, sigset_t *restrict oset);
// returns 0 if ok, -1 otherwise
```

- how flags : 어떻게 mask를 동작시킬 것인지
    
    *SIG_BLOCK* :  해당 set을 mask하도록 설정 
    
    *SIG_UNBLOCK* : 해당 set을 unmask하도록 설정 
    
    *SIG_SETMASK* : 해당 set으로 mask를 초기화
    
    ex. `sigprocmask(SIG_SETMASK, &prev_mask, NULL);` : 이전 마스크로 초기화
    
<br/>
**Signal sets** : 쉽게 mask를 조절하도록 도와주는 함수로, 자유롭게 set에 signal을 추가하고 삭제할 수 있다.

```c
#include <signal.h>

int sigemptyset(sigset_t *set); // 빈 set 생성
int sigfullset(sigset_t *set); // 전부 채운 set 생성

int sigaddset(sigset_t set, int signo); // 특정 sig 추가 
int sigdelset(sigset_t set, int signo); // 특정 sig 삭제 

int sigismember(const sigset_t *set, int signo);
// returns 1 if true, 0 if false (포함되어 있는지 확인)
```
<br/><br/>
### **Nested Handlers**

하나의 signal handler가 수행되는 동안 다른 핸들러가 등록되어 중첩되어 실행될 수 있다. 핸들러도 프로그램 코드 안에서 실행되기 때문에 전역변수가 공유된다. 핸들러가 중첩되어 동시에 전역변수를 접근하게 되면 문제가 발생할 수 있다. 

![image](https://user-images.githubusercontent.com/86834982/206746387-1e399e2f-dca7-40ad-8c0d-7b3bc1c576eb.png){: width="530px"}

**Race condition** : 어떤 프로세스가 먼저 실행되는지에 따라 실행 결과가 달라지는 것 

ex1. 두 개의 thread가 전역변수를 공유하면서 발생하는 문제 

![image](https://user-images.githubusercontent.com/86834982/206746391-f17059e1-6e8e-421e-8ebb-5e69a5208d5e.png){: width="550px"}

ex2. if문이 실행된 직후 interrupt signal이 발생하는 경우

-> iFlag 값이 0이 아닌데도 if문이 실행되는 문제점이 발생할 수 있음 

![image](https://user-images.githubusercontent.com/86834982/206746393-161d4c40-1845-4bd5-9028-4af358881694.png){: width="490px"}

<br/>
### **Synchronization : sigsuspend(2)**

pause 함수의 경우 특정 시그널을 기다릴 수 없고, sleep함수는 시그널이 들어왔을 때 즉각적인 처리가 어려운데,  sigsuspend 함수는 이 두 가지를 보완하는데, 시그널 블록을 설정함과 동시에 시그널이 도착할 때까지 대기한다. 

```c
#include <signal.h>
int sigsuspend(const sigset_t *sigmask);

// sigsuspend는 다음을 atomic operation으로 구현한 것
sigprocmask(SIG_BLOCK, &mask, &prev);
pause();
sigprocmask(SIG_SETMASK, &prev, NULL);
```
<br/>
ex. sigsuspend로 signal을 기다리는 예시

![image](https://user-images.githubusercontent.com/86834982/206746395-8ac75be7-1c3f-482d-95af-305a1d01bad8.png){: width="490px"}


<br/>
### Nonlocal Jump : sigsetjmp(3), siglongjmp(3)

비지역 점프(nonlocal jump)는 사용자가 의도적으로 만들어낸 exceptional control flow로, 강제로 코드를 다른 곳으로 보내고 돌아오게 한다. 

-> C 에서는 longjmp와 setjmp 함수를 통해 비지역 점프를 수행한다. 

<br/>
ex1.  setjmp와 longjmp을 사용한 예시 

한계점 : stack에 저장한 env가 남아있어야만 longjmp 수행이 가능하다. 

![image](https://user-images.githubusercontent.com/86834982/206825228-5e67189e-16e8-4796-b0e4-c93c9ff2b40e.png){: width="550px"}

ex2. signal에 대해서 setjmp와 longjmp를 사용한 예시 -> **문제 발생 !**

signal handler는 핸들러가 수행되는 동안에는 해당 시그널을 block 처리하도록 구현되어 있다. 따라서 핸들러 안에서 longjmp가 실행된 경우, 핸들러가 정상적인 종료를 하지 못하고, 계속 block된 상태로 main 함수로 돌아가기 때문에 문제가 발생한다. 

![image](https://user-images.githubusercontent.com/86834982/206825224-ef71f17b-28df-4b29-accf-e92683a3fb31.png){: width="550px"}

<br/>
 **sigsetjmp, siglongjmp** 

위의 상황을 방지해서 setjmp, longjmp를 수행함과 동시에 blocking까지 풀어주는 함수

```c
#include <setjmp.h>

int sigsetjmp(sigjmp_buf env, int savemask);
int siglongjmp(sigjmp_buf env, int val);
```
<br/>
ex. sigsetjmp, siglongjmp를 통해 보완한 예시 

![image](https://user-images.githubusercontent.com/86834982/206825229-5b310655-ed19-4e90-aa09-b57795a9d879.png){: width="550px"}




<br/><br/>
@Advanced Programming in the UNIX environment, Third edition 내용을 참고함
<br/><br/>
[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}
