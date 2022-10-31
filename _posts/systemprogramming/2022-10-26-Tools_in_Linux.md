---
title:  "Tools in Linux : make, gdb, patch" 

categories:
  - System Programming
tags:
  - [Linux, Unix, tools, compilers]

toc: true
toc_sticky: true

date: 2022-10-26
last_modified_at: 2022-10-26
---

    
<br/> 
## Compilation tools

---

- **compiler** : 특정 프로그래밍 언어로 작성된 코드를 기계어로 변환하는 프로그램

![image](https://user-images.githubusercontent.com/86834982/198941134-9a03bb44-a033-4f7a-967f-3db34b505868.png){: width="80%" height="80%"}

- compile 과정 :
    - **Preprocessor**  *cc **-E** hello.c > hello.i (Expanded source code)*
    - **Compiler**  *cc **-S*** *hello.i > hello.s (Assembly code)*
    - **Assembler**  *cc **-c** hello.s >* *hello.o (Object code)*
    - **Linker**  **ld** *hello.o >* *a.out (Executable code)  (a.out이 default)*
    - **Loader**  *(Execution)*

  
<br/> 
### GNU Compiler Collection (GCC)

UNIX, LINUX에서 사용하는 컴파일러, GNU 컴파일러 모음 

ex. `gcc` , `cc` 로 compile하는 전체 코드

-> preprocessing, compilation, assembly, linking 수행 (실행 빼고 다 함)

![image](https://user-images.githubusercontent.com/86834982/198941134-9a03bb44-a033-4f7a-967f-3db34b505868.png){: width="80%" height="80%"}

```bash
# c code --> expanded source code
cpp hello.c hello.i
cc -v -E hello.c > hello.i

# expanded source code --> assembly code
cc -v -S hello.i

# assembly code --> object code 
cc -v -c hello.s
objdump -d hello.o

# object code --> executable code
cc -v hello.o
ld hello.o [-lc]

./a.out
```
  <br/>
**GCC** **options 정리** 

*-o* : 파일 이름 지정

-*Wall* : warning 내용을 모두 보여줌 

*-E* : preprocessing에서 멈추기

*-c* : object 코드 생성, linking 전까지

*-l* : 특정 라이브러리 포함시키기 

*-g* : debugging 정보를 포함

*-Werror* : warning을 에러로 처리 

*-s* : assembly에서 멈추기

*-O* : optimization

*-L* : 특정 라이브러리 경로 포함시키기

  
 <br/>  
### Make

multi-module 프로그램 **컴파일 과정을 간단화, 자동화**하는 툴 

- `make` : Makefile로부터 규칙과 종속관계를 읽고 컴파일하는 명령어
    
    `make -f makefile_name` : makefile 이름을 변경하고 싶은 경우
    
    → 변경사항이 있는 파일만 업데이트하기 때문에 매우 **효율적**
    
- CMake : Makefile을 알아서 생성, 다른 플랫폼 OS에서도 사용 가능

  
<br/> 
### **Makefile**

make 프로그램의 설정파일, LINUX에서 **반복적인 컴파일을 쉽게 하기 위해서** 사용

- Target, Dependency, Command로 구성
- Makefile 기본 형식
    
    ```makefile
    Target: Dependency
    [tab] Command
    ```
    
- Makefile에 저장한 명령어들을 make 프로그램으로 실행
- dependency graph를 생성해서 프로그램 전체가 컴파일 되는 것을 방지

ex. Makefile 파일 예시 

![image](https://user-images.githubusercontent.com/86834982/198941247-59a18e7f-7d52-4637-86de-7a6bd1f5c392.png){: width="80%" height="80%"}
 <br/>   
**Dependency tree**

![image](https://user-images.githubusercontent.com/86834982/198941257-3807cf1e-6d6f-4c49-96ed-9aaa950b2af8.png){: width="80%" height="80%"}

- Makefile에 의해 생성된 **파일의 종속 관계**를 나타내는 그래프 → directed acyclic graph
- **post-order traversal** : 아래쪽 leaf node에서 위쪽 root node으로 순차적 탐색
- parent와 child의 modification시간을 확인하고 parent가 빠른 경우, parent만 업데이트

  
 <br/>  
### Macro

미리 정의되어 있는 환경 변수, 계속 사용되는 변수를 저장해서 빠르게 참조 

- *MACRO_NAME* : 대문자로 이름 지정
- *${MACRO}, $(MACRO), $MACRO* : 참조할 때 앞에 $ 사용하기
- `**make** -p` : 미리 정의된 macro를 확인하는 명령어
    
    ![image](https://user-images.githubusercontent.com/86834982/198941375-690d7025-2e0c-4bf3-ac32-cffe427a49c0.png){: width="80%" height="80%"}
    

ex. macro를 사용한 Makefile 예시 

![image](https://user-images.githubusercontent.com/86834982/198941375-690d7025-2e0c-4bf3-ac32-cffe427a49c0.png){: width="80%" height="80%"}


<br/> 
### Inference Rules

Makefile 빌딩 과정에서 자주 재사용되는 것들을 기호로 지정, 쉘의 wild card와 유사한 기능

![image](https://user-images.githubusercontent.com/86834982/198941660-e90977e6-717b-4c58-acff-a37999089424.png){: width="80%" height="80%"}

**Special Macro** 자동 문자
    
    ```makefile
    eval.o : eval.c eval.h
    			${CC} -c $< -o $@
    ```
    
  - `$*` :  target의 base이름  *eval*
  - `$@` : 현재 target 파일  *eval.o*
  - `$<` : 첫번째 dependency 파일  *eval.c*
  - `$?` :  첫번째 dependency 파일  *eval.c*
  - `$^` : 모든 dependency 파일  *eval.c eval.h*
  

<br/> 
## Software Development Tools

---

이 외로 LINUX에서 사용할 수 있는 소프트웨어 개발 툴 정리

<br/> 
### gdb(1)

LINUX에서 제공하는 무료 GNU debugging 툴 

-> syntax 버그는 비교적 찾기 쉽지만 logic 버그의 경우 찾기 어려움 → debugging 필요 !

ex. *fact.c* : 초기값 0부터 펙토리얼을 계산하는 코드 (잘못된 코드)

![image](https://user-images.githubusercontent.com/86834982/198941666-fc836f01-5410-4c4f-bf6e-185fc3f1bf5b.png){: width="80%" height="80%"}

-> `**gdb**`를 사용해서 버그 찾기  

![image](https://user-images.githubusercontent.com/86834982/198941670-cdbb8881-6d67-411e-b7bb-326c2ce4714d.png){: width="80%" height="80%"}

<br/> 
### screen(1)

가상 터미널을 만들어 **하나의 콘솔창에 여러 개의 터미널 세션**을 띄워주는 툴 

-> 화면 분할 및 공동 작업 가능, connection loss 되어도 다시 재개 가능

![image](https://user-images.githubusercontent.com/86834982/198941752-a15c55f2-5353-4aa9-9e9b-5e9806cf3c95.png){: width="80%" height="80%"}

```bash
screen           # screen 세션 생성
screen -S name   # screen 세션 이름을 지정해서 생성
screen -r name   # 특정 screen 세션 다시 들어가기
screen -d name   # 특정 screen 세션 연결 해제
screen -ls       #  screen 세션 리스트 출력
exit             # screen 세션 종료하고 빠져 나가기
```

<br/> 
### diff(1)

파일 비교 명령어, 두 개의 파일을 라인 단위로 비교해서 차이를 출력 

ex. `diff` 로 *MatrixAdd.c* *MatrixAdd_yourfriend.c* 파일 비교

![image](https://user-images.githubusercontent.com/86834982/198941759-60a7821b-bf3e-4b06-a61f-7335933d9644.png){: width="80%" height="80%"}

다음과 같이 diff 결과를 patch 파일로 저장할 수 있다. (redirection 사용)

```bash
diff -u MatrixAdd.c MatrixAdd_yourfriend.c > diff.patch
```

<br/> 
### patch(1)

diff로 생성한 **패치 파일을 적용**하거나 **적용했던 패치를 제거** 

![image](https://user-images.githubusercontent.com/86834982/198941828-844d81f4-8edc-40d8-b6af-2c34d15b3d36.png){: width="80%" height="80%"}

ex. `patch` 명령어로 패치 파일을 적용하고 제거하는 코드 

```bash
# patching file 
patch -p0 < diff.patch

# patching file in reverse 
patch -p0 -R < diff.patch
```

- ***-p* option** : 패치 파일 위치 명시, 앞에서부터 무시할 파일 경로의 개수
    
    *-p0* : 현재 디렉토리에서 패치,   *-p1*: 상위 디렉토리에서 패치 
    

<br/>  
### git(1)

버전 관리 시스템, **소스코드를 효과적으로 관리**할 수 있게 해주는 공개 소프트웨어

-> 많은 사람들의 공동 작업, 작업물 병합, 레포지토리 생성 등의 다양한 기능 제공 

```bash
git clone url                   # 레포지토리 소스코드 복제
git init repositoryname         # 새로운 레포지토리 초기화 

git add .                       # 변경 사항 스테이지에 올리기
git commit -m "commit message"  # 변경 사항 커밋하기 
git pull                        # 현재 상태 불러오기 
git push                        # 변경사항 내보내기, 업로드 

git diff                        # working directory와 staging area 차이 확인 (add 안한거)
git branch branchname           # 새 브랜치 생성 
```

 <br/>
<br/> 
@Advanced Programming in the UNIX environment, Third edition 내용을 참고함
<br/>   
   <br/>
    
<br/>
