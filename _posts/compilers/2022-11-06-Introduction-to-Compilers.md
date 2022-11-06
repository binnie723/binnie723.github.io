---
title: Introduction to Compilers
subtitle: 
categories: Compilers
tags: compiler
date: 2022-11-03 23:51:28 +0000
last_modified_at: 2022-11-03 23:51:28 +0000
---



이 수업은 Compiler가 어떻게 동작하는지의 과정을 깊게 다뤄볼 예정. 

### Computer Language classification

- **Imperative** : explicit solution, **어떻게** 할 것인가에 초점
    
    **Procedural**(Fortran, C), Object-Oriented (C++, Java) → code is easy, but huge 
    
- **Declarative** : implicit solution, **무엇을** 할 것인가에 초점
    
    **Functional**(Javascript, React), Logic → reduce complexity, but hard to learn
    

레스토랑에 가서 자리에 앉을 때, “저 창가 앞에 두번째 자리에 앉을게요. 메뉴 주세요.”는 imperative, “2명 자리 주세요”는 declarative . 그러나 결국 선언형도 내부적으로 어떻게 할 것인가에 대한 추상화가 필요.

- Why so many programming lanuguages?
    
    Evolution, Different Purposes, Personal Preference, Ease of learning and implementation, Open source, Good compilers and tools
    

### What is Compiler

- **Compiler** : Source code —(Assembly language)→ Machine code (0, 1)
- **Assembler** : Assembly language → Machine code (0,1)

컴파일러는 사람이 작성한 코드를 CPU가 이해할 수 있는 기계어로 변환해주는 프로그램. 

어셈블러는 어셈블리어를 기계어로 변환, 어셈블리어는 기계어를 그나마 사람이 읽을 만한 언어로 바꾼 것. 굳이 있는 이유는 > 컴파일 과정에서 어셈블리어를 만들 필요는 크게 없지만, 컴파일의 결과가 최적화되었는지 디버깅하는 과정에서는 사용된다. 

### Compilation and Interpretation

**Two Kinds of Translator** 

- **Compiler** : translates entire code, returns object file (.exe file)
- **Interpreter** : translates section of code, returns each result

컴파일러는 전체 파일을 **한 번에 번역**해서 오브젝트 파일을 만든다. 이후 링크를 통해 최종 실행 파일을 생성한다.

인터프리터는 코드를 **한 줄씩 번역**해서 바로 실행한다. 한 줄씩 실행하기 때문에 속도가 매우 느린데, 이를 극복하기 위해 최근에는 컴파일러와 함께 제공되기도 한다. 

→ 인터프리터도 일부 컴파일 과정을 수반하는데, 컴파일러에서 번역하는 방식과 다를까?

**Difference between Compiler and Interpreter**

|  | Compiler | Interpreter |
| --- | --- | --- |
| returned reults | executable file (safer) | result of execution |
| run frequency | once (faster) | line by line  |
| flexibility | cannot change, recompile | can change run again (flexible)  |
| debugging | hard to find  | easier to find  |

### **Mixing Compilation and Interpretation : Java**

- ***.java* file**  —javac—>  ***.class* file**  —JVM—>  **machine code**
    
    ![( javac : compiler,  JVM : interpreter )](Introduction%20to%20Compilers%20e77595e373164e238719be7c264d6fb8/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-10-25_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB_12.53.26.png)
    
    ( javac : compiler,  JVM : interpreter )
    
- Java Virtual Machine (JVM) : 자바 가상 머신 , 컴파일러가 생성한 byte 코드를 기계어로 변환
- Java Runtime Environment (JRE) = JVM + Libraries   **→ platform 종속적 !**
- Java Development Kit (JDK) = JRE + Development tools (Debugger, Compiler, JavaDoc)
    
    중간 과정으로 byte 코드로 변환하는 이유 : **less efficient, but better portability !**  JVM을 거쳐서 특정 하드웨어에 대한 의존도를 줄일 수 있고, 배포하는 과정에서 코드를 숨길 수 있기 때문이다. 
    

**New JRE : Just in Time Compiler**

![스크린샷 2022-10-25 오전 12.53.26.png](Introduction%20to%20Compilers%20e77595e373164e238719be7c264d6fb8/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-10-25_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB_12.53.26%201.png)

- **use compiler** instead of JRE interpreter → much faster !
- compiles code at a runtime(= just in time)

굳이 중간에 byte 코드로 변환하는 이유 > **less efficient, but better portability**   JVM을 거쳐서 특정 하드웨어에 대한 의존도를 줄일 수 있고, 배포하는 과정에서 코드를 1차로 숨길 수 있기 때문이다. 

### Post Compilation : Linking

![스크린샷 2022-10-25 오전 1.32.29.png](Introduction%20to%20Compilers%20e77595e373164e238719be7c264d6fb8/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-10-25_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB_1.32.29.png)

1. Preprocessor : remove comments, expand macro
2. Compiler : generate assembly language (for debugging!)
3. Assembler : generate machine code 
4. **Linker** : link all files, give final executable file

**Linking : relocation of object files,  libraries** 

Symbol Resolution : 각 symbol(함수, 변수) reference를 하나의 definition에 매칭하는 것.

Relocation : 재배치, symbol의 정의를 메모리에 올리고, reference가 이를 가르키도록 수정. 

![스크린샷 2022-10-25 오전 1.39.47.png](Introduction%20to%20Compilers%20e77595e373164e238719be7c264d6fb8/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-10-25_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB_1.39.47.png)

ex. relocation entries 

링커가 relocation entry를 발견하면, 그 symbol을 resolve하여 (빨간색 부분을) 채워넣는다.  

![스크린샷 2022-10-25 오전 1.44.52.png](Introduction%20to%20Compilers%20e77595e373164e238719be7c264d6fb8/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-10-25_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB_1.44.52.png)

ex. relocated text section

주소로 relocation(재배치)한 결과

![스크린샷 2022-10-25 오전 1.45.29.png](Introduction%20to%20Compilers%20e77595e373164e238719be7c264d6fb8/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-10-25_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB_1.45.29.png)

**Static Linking vs. Dynamic Linking** 

- **Static Linking** : 프로그램에서 사용하는 **라이브러리 모듈 내용을 복사**하는 방식
    
    → increases the size of exe file, hard to modify (수정마다 다시 링크 작업)
    
    ![스크린샷 2022-10-25 오전 8.51.43.png](Introduction%20to%20Compilers%20e77595e373164e238719be7c264d6fb8/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-10-25_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB_8.51.43.png)
    

- **Dynamic Linking** : 프로그램에서 사용하는 **라이브러리의 주소**를 가지고 있다가, 런타임에 **해당 주소로 가서 로드**해오는 방식
    
    → does not increase the size of file, 메모리와 디스크의 공간을 아낄 수 있다 
    
    ![스크린샷 2022-10-25 오전 9.39.49.png](Introduction%20to%20Compilers%20e77595e373164e238719be7c264d6fb8/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-10-25_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB_9.39.49.png)
