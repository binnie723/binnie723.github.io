---
title: Finite Automata - DFA, NFA
subtitle: 
categories: Compilers
tags:
  - [automata]
toc: true
toc_sticky: true
use_math: true
date: 2022-11-09 23:51:28 +0000
last_modified_at: 2022-11-09 23:51:28 +0000
---

<br/>
### Automata

오토마타는 오토마톤의 복수형으로, 추상적인 연산을 하는 장치 또는 기계를  수학적 모델이나 그림으로 표현한 것을 말한다. 

ex. 스위치를 state diagram으로 나타낸 예시   
![image](https://user-images.githubusercontent.com/86834982/205446212-925e4b58-6030-4cff-a654-f1a8ad9bb458.png){: width="450px"}  

오토마타는 형식 언어를 규정하고 finite state 형태로 나타낼 때도 사용된다. 형식언어는 유한한 종류의 문자로 이루어진 유한한 길이의 문자열의 집합을 말한다. 

ex. 정규 언어를 state diagram으로 나타낸 예시   
![image](https://user-images.githubusercontent.com/86834982/205446216-1842011d-8144-4251-83ee-f4b118d7cab8.png){: width="450px"}  


<br/>
### Chomsky Hierarchy

촘스키는 형식 언어를 크게 4가지의 언어로 계층화해서 나타냈는데, 아래서부터 정규 언어(regular language), 문맥 자유 언어(context-free language), 문맥 의존 언어(context-sensitive language),  그리고 귀납적 가산 언어(recursively-enumerable language)로 나누어 규정한다. 

![image](https://user-images.githubusercontent.com/86834982/205446220-6eaa097a-9a0e-4a15-bfac-f71fb968d7f1.png){: width="400px"}  


<br/>   
### Finite State Automata : DFA, NFA

유한 상태 오토마타 또는 기계라고도 부른다. 유한 상태 오토마타는 한 번에 하나의 상태만을 가질 수 있다. 어떠한 사건에 의해 한 상태에서 다른 상태로 변화할 수 있는데 이를 **전이(transition)**라고 한다. 

하위 계층의 오토마타로 상위 계층의 언어를 인식할 수 없기 때문에, 유한 상태 오토마타는 context-free grammar를 인식할 수는 없지만 파싱을 효율적으로 하는 알고리즘의 도구를 만들기 위해 사용된다. 

<br/>
**Deterministic Finite Automata(DFA)**

하나의 state에 대해서 하나의 transition만 존재하기 때문에 어디로 가야하는지, 어떤 결과를 도출할 것인지 확실히 알 수 있다. 따라서 deterministic automata라고 표현하며, 주로 확실한 정답을 유추하는 상황에서 사용된다. 

ex. DFA 예시 : a (a \| b)

![image](https://user-images.githubusercontent.com/86834982/205446270-e568c9a4-a311-4198-a7f1-f642693ecb30.png){: width="400px"}  

<br/>
**Nondeterministic Finite Automata(NFA)** 

하나의 state에 대해서 여러 개의 transition을 가질 수 있다. 각 state에 대해 확신이 없는 오토마타로, 주로 확실한 하나의 결과보다는 final state에 이를 수 있는 possible state transition을 전부 찾는 상황에서 사용된다. 

ex. NFA 예시 : (0 \| 1) 1 (0 \| 1)

![image](https://user-images.githubusercontent.com/86834982/205446274-ef262822-c09d-45f6-9ed4-ae342533d00e.png){: width="400px"}  

<br/>
**Converting DFA to NFA**

NFA는 여러 개의 transition를 정의할 수 있기 때문에 정규식을 나타내는데 훨씬 쉽다. 반면에 DFA는 표현하는게 복잡한 대신 하나의 정답을 유추할 수 있기 때문에 먼저 NFA로 구현하고 DFA로 변환하는 과정이 필요하다. (-> Transition Table)

ex. NFA에서 DFA로 변환하는 예시 : (0 \| 1) 1 (0 \| 1)

![image](https://user-images.githubusercontent.com/86834982/205446278-d77127cf-dbca-435c-9551-271e2f17a130.png){: width="400px"}  

1. NFA로 나타낸 diagram을 table 형태로 나타낸다. 
    
    | NFA | 0 | 1 |
    | --- | --- | --- |
    | q0 | q0 | q0 q1 |
    | q1 | q2 | q2 |
    | q2 | - | - |
2. NFA table의 첫번째 줄을 복사해온다.
3. DFA table를 생성한다.  
    -> NFA :  최대 $n$ states,→DFA : 최대 $2^n$ states
    
    | DFA | 0 | 1 |
    | --- | --- | --- |
    | q0 | q0 | q0 q1 |
    | q0q1 | q0q2 | q0q1q2 |
    | q0q2 | q0 | q0q1 |
    | q0q1q2 | q0q2 | q0q1q2 |
4. DFA diagram으로 변환해서 나타낸다.
    
    ![image](https://user-images.githubusercontent.com/86834982/205446279-ed091266-0231-466c-b525-75d0adda2641.png){: width="400px"}  
    

전체적인 Lexical Analysis 과정은 

Lexical Specification -> Regular Expression -> NFA -> DFA -> minimal DFA -> generate Scanner 


<br/>
### Push-Down Automata

정규 언어만으로는 프로그래밍 언어를 표현하는데 한계가 있기 때문에 컴파일러는 문맥 자유 언어를 통해 생성 규칙과 nesting 구조를 정의한다. 이 경우 이전의 상태로 돌아갈 수 있어야 하는데, 이러한 계산 능력을 갖춘 기계가 푸시 다운 오토마타이다. 

푸시 다운 오토마타는 스택을 가지고 있기 때문에 다음 상태를 스텍에 push 해서 새로운 상태로 넘어갈 수 있고, pop해서 이전 상태로 돌아갈 수 있다. 

![image](https://user-images.githubusercontent.com/86834982/205446256-672a5fe7-303b-4aa4-83f7-67ed2ffdc492.png){: width="600px"}  



<br/>   
<br/><br/>
[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}
