---
title:  "Linux Basics 개념" 

categories:
  - System Programming
tags:
  - [linux]
toc: true
toc_sticky: true

date: 2022-10-24
last_modified_at: 2022-10-25
---


<br/> 
## LINUX/UNIX

리눅스/유닉스는 **multiuser based** OS로 많은 유저들이 동시에 접근 가능한 운영체제이다. 

둘은 매우 비슷한 운영체제이지만, 그 중에서 리눅스는 유닉스의 장점을 포함하는 운영체제로, 유닉스를 개인 컴퓨터에서 사용할 수 있도록 하기 위해 개발되었다. 또한 리눅스는 다양한 배포판과 오픈소스가 있고, 무료로 이용이 가능하기 때문에 유닉스보다 더 큰 시장 점유율을 가지고 있다. 

- **장점** : multi-user 환경, 멀티태스킹, 확장성, 유연성 등
- **UNIX variants** : MacOS, iOS, Darwin 등    
<br/> 

## UNIX Architecture

**운영체제(OS)** : 컴퓨터 하드웨어를 관리하고 프로그램을 실행하는 환경을 제공해주는 소프트웨어로, kernel과 컴퓨터에 다양한 기능들을 제공하는 소프트웨어를 모두 포함하는 개념   

UNIX 운영체제 구조 
- kernel : 운영체제의 가장 핵심적인 부분으로, 하드웨어 시스템을 통제하는 역할 
- shell : 커널과 사용자를 연결해주는 인터페이스로, 사용자의 명령어를 해석해서 커널에 전달
- applications : 여러 가지 응용 프로그램 및 프로그래밍 언어를 지원 

![image](https://user-images.githubusercontent.com/86834982/198583431-2ec1bc6d-0a56-48f9-b906-6a6ed31373ce.png){: width="530px"}
  
<br/> 

## Logging in to UNIX system

UNIX는 로그인할 때 **username**, **password** 입력 받는데, 시스템은 사용자들에 대한 정보를 보관하는 /etc/passwd(패스워드) 파일을 열어서 해당 사용자 이름이 있는지 확인한다. 이를 통해 사용자들의 접근을 제한하고 보안을 유지한다. 

ex. 학교 서버의 /etc/passwd 파일   

![image](https://user-images.githubusercontent.com/86834982/198583452-ab328d2a-6a84-4fe8-a389-687b1a09bbb6.png){: width="530px"}  
    username:password:userid:groupid::homedirectory:shellprogram  


- **User ID** : 로그인한 유저를 나타내는 아이디(숫자)로, 시스템 관리자에 의해 등록된다. uid가 0인 경우, superuser(root 권한자)를 나타낸다.
    
    
- **Group ID** : 로그인한 유저가 속한 그룹을 나타내는 아이디(숫자)로, 특정 파일 공유 및 접근이 가능하다.  
    
    -> 유저와 그룹 아이디는 특정 파일에 대한 **접근 권한을 확인**하는데 사용된다 !
    

<br/> 
## Pipes

프로세스가 나열되는 것, 어떤 프로세스의 표준 출력이 다른 프로세스의 표준 입력으로 들어가는 것을 말한다. 일종의 interprocess communication의 한 방법으로, **파이프**를 통해 프로그램을 연속으로 수행 가능하다. 

![image](https://user-images.githubusercontent.com/86834982/198583923-640be25a-df7c-4f08-8dea-fd49763ab528.png){: width="530px"}  


ex. *reverse.c* : 파이프로 문자열을 거꾸로 출력하는 코드

![image](https://user-images.githubusercontent.com/86834982/198583944-1b3034cf-5174-44f3-b72c-da917cc976f9.png){: width="530px"}

<br/>     

## Redirection

프로세스 스트림의 방향을 지정해주는 역할 

- cmd  >  output : 명령어를 output으로 저장
- cmd  <  input : 파일로 부터 입력 받기
- cmd  > >  append : 명령어를 output 마지막에 추가   
<br/>   

## Files and Directories
![image](https://user-images.githubusercontent.com/86834982/198584018-95b06d06-8133-48ab-9e9d-5958aa810161.png){: width="550px"}


- 루트 디렉토리를 중심으로 **tree 구조**로 구성

- 모든 장치 및 디렉토리를 **파일(=읽고 쓸 수 있는 것)로 취급**
    
    Regular files(일반 파일), Directory files(디렉토리), Special file(I/O device 장치)
    

- 기본적으로 제공되는 **디렉토리**
    - `.` : current directory (디렉토리 생성시 자동 포함)
    - `..` : parent directory (디렉토리 생성시 자동 포함)
    - `/` : root directory
    - `~` : home directory  
<br/>
- 디렉토리 경로 : **절대 경로, 상대 경로**
    - absolute path : 루트 디렉토리(`/`) 기준 →ex. */home/binnie/a.txt*
    - relative path : 현재 디렉토리(`.`) 기준  →ex. *usr/bin/xv*  
<br/>
- **디렉토리 파일 목록 보기 : `ls -la`**
    
    ![image](https://user-images.githubusercontent.com/86834982/198584033-970e305b-37fb-4d82-a4be-361f4c9163ab.png){: width="500px"}
    <br/>type+permission / links / owner / group / filesize / date / time
    
    
<br/>
ex. ***simple-ls.c*** : `ls` 명령어를 구현한 코드

![image](https://user-images.githubusercontent.com/86834982/198584173-13a92c17-2aac-4942-9a3f-505ea72bb8aa.png){: width="570px"}  

<br/>
ex. listing files in ascending order : `ls`를 오름차순으로 구현

![image](https://user-images.githubusercontent.com/86834982/198584474-ec9290bf-4dd4-490c-81a8-14b4984749c4.png){: width="570px"}  

<br/>
ex. simple-shell with multiple arguments

![image](https://user-images.githubusercontent.com/86834982/198584459-047e3020-c7cb-443d-a0d9-2e6c80999994.png){: width="500px"}
<br/>     
<br/>
## Standard I/O

모든 UNIX 프로그램은 3가지의 표준 스트림과 표준 파일 디스크립터를 제공한다. 

**Standard Stream** : 입출력 통로, 리눅스 프로그램을 실행하면 3개의 표준 스트림이 자동으로 열린다.
    
- Standard Input (***stdin***, file descriptor 0) : 터미널에서 받아오는 입력
- Standard output (***stdout***, file descriptor 1) : 정상적인 실행 결과를 터미널에 출력
- Standard error (***stderr***, file descriptor 2) :  비정상 종료시 터미널로 반환  

<br/>
**File Descriptors** : 프로세스가 특정 파일에 접근할 때, **파일을 분류**하기 위해 제공되는 고유 숫자
    
- **unbuffered I/O** : buffer 지정 필요한 입출력 함수
: `open(2)`, `read(2)`, `write(2)`, `lseek(2)`, `close(2)`
    
- **buffered I/O** : buffer 따로 지정 없이 사용 가능한 입출력 함수
: `getc(2)`, `putc(2)`, `fopen(2)`, `fread(2)`, `fwrite(2)`
    
<br/>
ex. ***cat.c*** : `cat`명령어를 구현한 코드  
![image](https://user-images.githubusercontent.com/86834982/198584192-73fe1a62-6a16-434d-973b-3dc6ddaf1cba.png){: width="530px"}

<br/>   

## UNIX Time Values

**Process** : 현재 실행되고 있는 컴퓨터 프로그램, 프로그램이 메모리에 올라간 상태   

**Process time** : 프로세스 관련한 시간 정보로, clock ticks(clock_t)에 의해 측정

-> clock time(총), user CPU time(user), system CPU time(kernel)

<br/>
`time ls` : 명령어의 running time을 확인

![image](https://user-images.githubusercontent.com/86834982/198584459-047e3020-c7cb-443d-a0d9-2e6c80999994.png){: width="530px"}
    
<br/>
`ps` : 현재 돌아가는 process 확인  
`pstree` : process tree 출력하는 명령어  

![image](https://user-images.githubusercontent.com/86834982/198584436-eae6a354-0c88-42df-aa70-0ff149449c48.png){: width="510px"}
<br/>   

<br/> <br/> 
@Advanced Programming in the UNIX environment, Third edition 내용을 참고함  
<br/><br/>
[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}
