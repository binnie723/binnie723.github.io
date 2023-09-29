---
title: Compilation and Interpretation
subtitle: 
categories: Compilers
tags:
  - [compiler, interpreter]
toc: true
toc_sticky: true
date: 2022-11-03 23:51:28 +0000
last_modified_at: 2022-11-03 23:51:28 +0000
---


<br/>
### What is Compiler

- **Compiler** : Source code —(Assembly language)-> Machine code (0, 1)
- **Assembler** : Assembly language -> Machine code (0,1)  

![image](https://user-images.githubusercontent.com/86834982/205443277-97c15d02-6949-43c8-a732-6d354a3602b0.png){: width="600px"}  

**컴파일러**는 넓은 의미로는 하나의 언어를 다른 언어로 번역해주는 프로그램, 좁은 의미로는 사람이 작성한 코드를 **CPU가 이해할 수 있는 기계어로 변환**해주는 프로그램.

**어셈블러**는 **어셈블리어를 기계어로 변환**해주는 프로그램, 여기서 어셈블리어는 기계어와 일대일 대응이 되는 로우 레벨 언어로, 쉽게 말해 기계어를 그나마 사람이 읽을 만한 언어로 바꾼 것이다. 그렇다면 굳이 어셈블리어가 있는 이유는? 컴파일 과정에서는 굳이 어셈블리어를 만들 필요는 없지만, 컴파일의 결과가 최적화되었는지 디버깅하는 과정에서는 사용된다.  


<br/>
### What is Interpreter

- **Compiler** : translates **entire code**, returns **object file (.exe file)**
- **Interpreter** : translates **section of code**, returns **each result**

![image](https://user-images.githubusercontent.com/86834982/205443599-6ed83111-fc53-4e65-887f-f72d10b9b1dd.png){: width="600px"}  

컴파일러는 프로그램 전체 파일을 스캔해서 **한 번에 번역**하여 오브젝트 파일(Object code)을 만든다. 이후 링크를 통해 최종 실행 파일을 생성한다. 대표적인 언어로 C, C++. JAVA 등이 있다. 

인터프리터는 코드를 **한 줄씩 번역**해서 바로 실행한다. 한 줄씩 실행하기 때문에 속도가 매우 느리지만 오브젝트 파일을 생성하지 않고, 링킹하지 않기 때문에 메모리 효율이 좋다. 대표적인 언어로는 Python, Ruby, Javascript가 있다.  
<br/>
**Difference between Compiler and Interpreter**

|  | Compiler | Interpreter |
| --- | --- | --- |
| returned reults | executable file (safer) | result of execution |
| run frequency | once (faster) | line by line  |
| flexibility | cannot change, recompile | can change run again (flexible)  |
| debugging | hard to find  | easier to find  |

  
<br/>
### **Mixing Compilation and Interpretation : Java**

- ***.java* file** - (javac) ->  ***.class* file** - (JVM) -> **machine code**

    ( javac : compiler,  JVM : interpreter )  
![image](https://user-images.githubusercontent.com/86834982/205443644-a682c1e6-622a-4212-8726-1f015ea790d9.png){: width="600px"}  
    
- 중간 과정으로 byte 코드로 변환하는 이유 : **less efficient, but better portability !**
    
    JVM을 거쳐서 특정 하드웨어에 대한 의존도를 줄일 수 있고, 배포하는 과정에서 코드를 숨길 수 있기 때문이다. 
    
- **Java Virtual Machine (JVM)** : 자바 가상 머신으로, 컴파일러가 생성한 byte 코드를 기계어로 변환한다.
- **Java Runtime Environment (JRE)** = JVM + Libraries   **-> platform 종속적 !**
- **Java Development Kit (JDK)** = JRE + Development tools (Debugger, Compiler, JavaDoc)  

<br/>

**New JRE : Just in Time Compiler**

- **use JIT compiler** instead of JRE interpreter -> much faster !
- compiles code at a runtime(= just in time)

JIT 컴파일러는 인터프리터와 컴파일러를 혼합한 방식이다. 최근의 **자바 가상 머신에서 JIT 컴파일**을 지원하는데, 자바 컴파일러(javac)가 자바 프로그램 코드를 바이트 코드로 변환한 후, 실제 바이트 코드를 실행하는 시점에서 자바 가상 머신(java virtual machine)이 JIT 컴파일을 통해 기계어로 변환한다.

![image](https://user-images.githubusercontent.com/86834982/205443736-ad4b918a-9c16-49ed-bcf3-742c59f23965.png){: width="600px"}  




<br/>   
<br/><br/>
[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}
