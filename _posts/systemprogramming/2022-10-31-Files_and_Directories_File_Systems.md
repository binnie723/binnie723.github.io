---
title:  "[5] Files랑 Directories, File Systems" 

categories:
  - System Programming
tags:
  - [Linux, Unix, file, directory]

toc: true
toc_sticky: true

date: 2022-10-31
last_modified_at: 2022-10-31
---
<br/>   
### Contents

- **Unix File Types**에는 어떤 것들이 있는지
- **File Metadata**  (Metadata: 데이터에 대한 데이터, 어떤 데이터를 설명하는 데이터)
- **File Systems**  Linux에서의 File System은 어떻게 구성되는지

<br/>   
### Definition of File

| User Definition  | OS Definition |
| --- | --- |
| sequence of bytes (= Array)로 나타내는 것  | nonvolatile(비휘발적) storage device에 저장된 것  |
| - file types 은 user가 결정 (모든 확장자 가능)
- rebooting, power failures에도 정보 유지 | - bytes : collection of blocks on physical storage   |

<br/>   
### File Types

1. **Regular File(-)** 
<br/>
    \- user, app (arbitrary) data를 포함하는 파일
<br/>
    \- text files : ASCII and Unicode 로 구성된 것  -  *‘\n’* (*0xa, LF* =10) 로 끝남
<br/>
    \- binary files : 그 외의 나머지  (object files, images)
        
    -> kernel (OS)은 두 파일을 구분 짓지 않고 “파일” 자체로 인식 
<br/>        
2. **Directory File(d)** 
<br/>
    \- **파일의 이름**과 **주소**를 포함하는 파일
<br/>
    \- 새로운 파일 생성시 default로 포함 : `.` (현재 디렉토리), `..` (부모 디렉토리)
<br/><br/>
    \- **Directory Hierarchy** : root 디렉토리(/)를 중심으로 hierarchy 형성
        
    ![image](https://user-images.githubusercontent.com/86834982/199545979-0a1896e2-d194-478b-bb98-43442c2475ee.png){: width="80%" height="80%"}
<br/><br/>
3. **Character special(c), Block special(b) files** 
    - Character special : 입력이 들어온 즉시 바로 수행하는 하드웨어 (terminal)
    - Block special ****: 입력을 block마다 수행하는 하드웨어 (disk)
<br/><br/>
4. **FIFO(p = pipe)** 
<br/>
    \- 프로세스 간에 inter-process communication 할 때 사용
<br/><br/>
5. **Socket(s)** 
<br/>
    \- 네트워크 간에 프로세스 통신할 때 사용되는 소켓
<br/><br/>
6. **Symbolic Link(l)** 
<br/>
    \- softlink, 어떤 파일을 가르키는 포인터 파일
<br/><br/>  
**File Type Macros**
    
    ![image](https://user-images.githubusercontent.com/86834982/199546000-25d32bbc-0260-4c3b-bbbf-88fc5a3faa04.png){: width="80%" height="80%"}
    

<br/>   
### File Metadata

데이터에 대한 데이터, 즉 파일을 설명하는 데이터를 말한다

- **Name** : 유일하게 사람이 읽을 수 있는 형태의 정보
- **Location** : 디바이스의 위치를 가르키는 pointer
- **Identifier** : 어떤 파일인지 나타내는 unique number 태그
- **Size** : 현재 파일의 크기
- **Protection** : controls permissions for reading, writing, executing
- **Time, date, and user identification** : 파일 생성 날짜, 수정 날짜, 유저 정보
    
    -> 이러한 메타데이터들을 저장하는 장소 :  Unix/Linux의 i-node table
    

    ![image](https://user-images.githubusercontent.com/86834982/199546193-eb1d2cc1-dba9-492e-b71a-8e4beca29806.png){: width="80%" height="80%"}

    -> kernel이 가진 정보들이기 때문에 접근을 위해서는 system call 함수를 사용해야 함 

<br/>   
### stat(2)

파일의 type과 metadata를 보여주는 함수 

```c
#include <sys/stat.h>

int stat(const char *path, struct stat *sb);
int lstat(const char *path, struct stat *sb); //symbolic link 정보

int fstat(int fd, struct stat *sb);
int fstatat(int fd, const char *path, struct stat *sb); //fd 위치 기준 상대경로
```

- `ls -l file` : 파일의 속성과 정보까지 리스트로 출력하는 명령어
    
    -> 터미널에 실행한 화면 
    

    ![  image](https://user-images.githubusercontent.com/86834982/199546222-d2baf26f-f68f-4b51-abee-c0c7af4139c6.png){: width="80%" height="80%"}

- `stat -t file` : 더 detail한 정보까지 포함해서 출력하는 명령어
    
    -> 터미널에 실행한 화면
    
    ![image](https://user-images.githubusercontent.com/86834982/199546272-4a351182-8c3d-413f-8d16-dce540d34aa7.png){: width="80%" height="80%"}
    
    -> stat mode, hardlink의 갯수, 해당 파일 소유자의  uid, gid, time 등의 정보들을 모두 포함!

<br/><br/>
- **st_mode** : 파일의 형식이나 접근 권한에 대한 정보를 저장
    
    `mode_t st_mode;` : stat 구조체에 선언되어 있는 inode 정보 중 하나 
    
    다음과 같은 구조로 정의되어 있음 
    

![image](https://user-images.githubusercontent.com/86834982/199546950-49836508-4e10-48f0-acb2-27e207c0bcef.png){: width="80%" height="80%"}

ex. simple-ls 변형 : file의 status까지 확인해볼 수 있는 코드 

![image](https://user-images.githubusercontent.com/86834982/199546513-bfb4d226-ba9a-420b-82a1-3f60a7e33139.png){: width="80%" height="80%"}

---
<br/>   
### UIDs and GIDs

Unix/Linux 시스템은 multi-user based 체계의 운영체제이기 때문에 사용자들 구분이 필요.

-> 사용자를 구분하기 위해 각자 다른 ID 숫자를 부여해서 관리함

1. **real user/group ID** : 실제 사용자, 프로그램을 실행한 사용자
2. **effective user/group ID** : 유효 사용자, OS가 권한 확인 대상 
    
    -> 일반적으로는 real user id = effective user id 
    
    -> 그러나 다른 소유자 파일에 접근할 때, 임시로 권한이 필요할 때 사용
    
3. **saved set-user/group-ID** : saved by exec functions (얜 머지?)

<br/>   
### SetUIDs and SetGIDs

`setuid` : 파일 소유자(*st_uid*)로 effective user ID 설정
    <br/> -> 다른 사람의 파일을 실행할 때, 소유자 권한이 필요한 경우 사용

`setgid` : 파일 그룹(*st_gid*)으로 effective group ID 설정 
    <br/> -> 다른 사람의 파일을 실행할 때, 그룹 권한이 필요한 경우 사용


    
Unix/Linux 시스템에는 특수권한이라고 불리는 **“SetUID, SetGID, StickyBit”**이 있다. 이 3가지는 모두 실행 권한(x) 관련된 기능으로 사용되어 특수한 기호가 실행 자리에 생긴다. 예를 들어,
    
- 파일에 **SetUID**를 설정하면, user의 실행 권한에 **x 대신 “s”**가 들어가고 (4000)
- 파일에 **SetGID**를 설정하면, group의 실행 권한에 **x 대신 “s”**가 들어가고 (2000)
- 파일에 **StickyBit**을 설정하면, other의 실행 권한에 **x 대신 “t”**가 들어간다. (1000)
    - 기존 파일 `drwxrwxrwx` ***777***
    - 특수 권한 파일 `drwsrwsrwt` **7777** (SetUID + SetGID + StickyBit = 7000을 추가)
        
    ![image](https://user-images.githubusercontent.com/86834982/199546578-cc3d5911-b898-4620-b099-ec03a74e2afc.png){: width="80%" height="80%"}
        
이렇게 원래 권한은 **“특수권한, UID, GID, Other”**순으로 4자리 형태이고, 여태까지 `chmod 777`은 특수권한을 생략한 것임. 따라서 원래는 `chmod 0777`이 더 정확한 표현이다.   
        
-> 그러면 대문자 **S, S, T**는 언제 적용되는지? : 기존에 실행 권한(x)이 있었으면 소문자, 없었으면 대문자

        

<br/> **Standard I/O**를 사용해서 변경하는 방법

```c
#include <unistd.h>
    
int setuid(uid_t uid);
int seteuid(uid_t uid);  // return 0 OK, -1 error
    
uid_t getuid(void);
uid_t geteuid(void);     // get uid_t, never errors 
```
<br/>
**Linux command**로 터미널에서 변경하는 방법
    
    `chmod u+s filename` (+4000)
    
    `chmod g+s filename` (+2000)



<br/>   <br/> 
### access(2)

파일의 권한을 확인할 때 사용하는 함수

-> 파일 open하기 전에 먼저 권한을 확인하고 싶을 때 주로 사용 

```c
#include <unistd.h>

int access(const char *path, int mode);
int faccessat(int fd, const char *path, int mode, int flags); // can use relative path
```

- options
    
    *R_OK* : 읽기 권한 있는지 확인 
    
    *W_OK* : 쓰기 권한 있는지 확인 
    
    *X_OK* : 실행 권한 있는지 확인 
    
    *F_OK* : 파일이 존재하는지 확인 
    

<br/>  <br/>  
 
@Advanced Programming in the UNIX environment, Third edition 내용을 참고함


<br/><br/>
[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right} 
