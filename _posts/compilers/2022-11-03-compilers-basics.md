---
title: Compiler 개념 및 과정
subtitle: 
categories: Compilers
tags: [compiler, interpreter, linking]
toc: true
toc_sticky: true
date: 2022-11-03 23:51:28 +0000
last_modified_at: 2022-11-03 23:51:28 +0000
---

<br/>
### What is Compiler

- **Compiler** : Source code - (Assembly language) -> Machine code (0, 1)
- **Assembler** : Assembly language -> Machine code (0,1)

컴파일러는 사람이 작성한 코드를 CPU가 이해할 수 있는 기계어로 변환해주는 프로그램. 

어셈블러는 어셈블리어를 기계어로 변환, 어셈블리어는 기계어를 그나마 사람이 읽을 만한 언어로 바꾼 것. 굳이 있는 이유는 컴파일 과정에서 어셈블리어를 만들 필요는 크게 없지만, 컴파일의 결과가 최적화되었는지 디버깅하는 과정에서는 사용된다. 

<br/>
### Compilation vs. Interpretation

둘 다 translator의 종류, 다른 방법으로 translate
- **Compiler** : translates entire code, returns object file (.exe file)
- **Interpreter** : translates section of code, returns each result

컴파일러는 전체 파일을 **한 번에 번역**해서 오브젝트 파일을 만든다. 이후 링크를 통해 최종 실행 파일을 생성한다.

인터프리터는 코드를 **한 줄씩 번역**해서 바로 실행한다. 한 줄씩 실행하기 때문에 속도가 매우 느린데, 이를 극복하기 위해 최근에는 컴파일러와 함께 제공되기도 한다. 

-> *인터프리터도 일부 컴파일 과정을 수반하는데, 컴파일러에서 번역하는 방식과 다를까?*

**Difference between Compiler and Interpreter**

|  | Compiler | Interpreter |
| --- | --- | --- |
| returned reults | executable file (safer) | result of execution |
| run frequency | once (faster) | line by line  |
| flexibility | cannot change, recompile | can change run again (flexible)  |
| debugging | hard to find  | easier to find  |

<br/>
### **Mixing Compilation + Interpretation (ex. Java)**

- ***.java* file** - (javac) ->  ***.class* file** - (JVM) -> **machine code**
    
    ![image](https://user-images.githubusercontent.com/86834982/200181795-656cacc0-3411-461e-af7f-e8e06c517c59.png){: width="80%" height="80%"}
    
    ( javac : compiler,  JVM : interpreter )
    
- Java Virtual Machine (JVM) : 자바 가상 머신 , 컴파일러가 생성한 byte 코드를 기계어로 변환
- Java Runtime Environment (JRE) = JVM + Libraries   **→ platform 종속적 !**
- Java Development Kit (JDK) = JRE + Development tools (Debugger, Compiler, JavaDoc)
    
    중간 과정으로 byte 코드로 변환하는 이유 : **less efficient, but better portability !**  JVM을 거쳐서 특정 하드웨어에 대한 의존도를 줄일 수 있고, 배포하는 과정에서 코드를 숨길 수 있기 때문이다. 
    
<br/>
**New JRE : Just in Time Compiler**

![image](Introduction%20to%20Compilers%20e77595e373164e238719be7c264d6fb8/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-10-25_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB_12.53.26%201.png){: width="80%" height="80%"}

- **use compiler** instead of JRE interpreter → much faster !
- compiles code at a runtime(= just in time)

굳이 중간에 byte 코드로 변환하는 이유 > **less efficient, but better portability**   JVM을 거쳐서 특정 하드웨어에 대한 의존도를 줄일 수 있고, 배포하는 과정에서 코드를 1차로 숨길 수 있기 때문이다. 

<br/>
### Compilation Phases

1. **Lexical Analysis** : **divides** into individual tokens, and **verifies tokens**
2. **Syntactic Analysis** : checks the **structure** and **grammar**  
3. Semantic Analysis : checks the **meaning** of the code  
4. Optimization : optimizes code for better matching assembly language 
5. Assembly Code  = intermediate code, assembler를 통해 변환
6. Object Code (*.o file*)
7. Linking (*.exe file*)


<br/>
### Post Compilation : Linking

![image](https://user-images.githubusercontent.com/86834982/200181850-2aa39527-e113-436a-bce3-f6064d5b3e96.png){: width="80%" height="80%"}

1. Preprocessor : remove comments, expand macro
2. Compiler : generate assembly language (for debugging!)
3. Assembler : generate machine code 
4. **Linker** : link all files, give final executable file

<br/>
**Linking : relocation of object files,  libraries** 

- Symbol Resolution : 각 symbol(함수, 변수) reference를 하나의 definition에 매칭하는 것.

- Relocation : 재배치, symbol의 정의를 메모리에 올리고, reference가 이를 가르키도록 수정. 

    ![image](https://user-images.githubusercontent.com/86834982/200181852-6a5a2a0c-1437-4cf7-9a8b-faa51c6e2bbd.png){: width="80%" height="80%"}

    ex. relocation entries 

    링커가 relocation entry를 발견하면, 그 symbol을 resolve하여 (빨간색 부분을) 채워넣는다.  

    ![image](https://user-images.githubusercontent.com/86834982/200181881-4284def0-7f61-4924-ac61-214eee51200b.png){: width="80%" height="80%"}

    ex. relocated text section : 주소로 relocation(재배치)한 결과

    ![image](https://user-images.githubusercontent.com/86834982/200181882-7c8ca937-45c6-4bce-a54d-4d7e34790db3.png){: width="80%" height="80%"}

<br/>
**Static Linking vs. Dynamic Linking** 

- **Static Linking** : 프로그램에서 사용하는 **라이브러리 모듈 내용을 복사**하는 방식
    
    -> increases the size of exe file, hard to modify (수정마다 다시 링크 작업)
    
    ![image](https://user-images.githubusercontent.com/86834982/200181911-52031cd9-e5cd-4b5a-8dea-6601bfeeb09e.png){: width="80%" height="80%"}
    

- **Dynamic Linking** : 프로그램에서 사용하는 **라이브러리의 주소**를 가지고 있다가, 런타임에 **해당 주소로 가서 로드**해오는 방식
    
    -> does not increase the size of file, 메모리와 디스크의 공간을 아낄 수 있다 
    
    ![image](https://user-images.githubusercontent.com/86834982/200181914-52873492-1084-4cea-a571-9eff5232b667.png){: width="80%" height="80%"}

<br/>   
<br/><br/>
[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}
