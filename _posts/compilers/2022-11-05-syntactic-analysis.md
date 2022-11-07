---
title: Syntactic Analysis
subtitle: 
categories: Compilers
use_math: true
toc: true
toc_sticky: true
tags: [compilation, syntactic, analysis]
date: 2022-11-05 01:34:06 +0000
last_modified_at: 2022-11-05 01:34:06 +0000
---


<br/>
### Compilation Phases

1. **Lexical Analysis** : 1. **divides** into individual tokens  2. **verifies tokens**
2. <span style="color:skyblue">**Syntactic Analysis**</span> : checks the **structure** and **grammar**
3. Semantic Analysis : checks the **meaning** of the code  
4. Optimization : optimizes code for better matching assembly language 
5. Assembly Code  : assembler를 통해 변환
6. Object Code (*.o file*)
7. Linking (*.exe file*)


<br/>
## Syntactic Analysis

Lexical Analysis를 통해 token의 정규식 Regular Expression을 NFA-> DFA로 나타내고 이를 통과시켰다면, 이후에는 Syntactic Analysis를 수행해야 한다. Parser를 통해 각 token의 structure와 grammar를 체크한다. 

<br/>
### Context Free Grammar

> defines a set of rules, which will make strings, which collectively can be Context Free Language
> 

언어를 regular expression으로만 표현하기 어려운 경우가 존재한다. 예를 들어 { ( , ) } 를 포함하는 구문을 regular expression으로 나타낸다고 하면, (\*)\* 로 표현할 수 있다. 이 정의에 따르면 ((), (((), .. 등이 경우의 수에 포함되는데 여기서 open brace와 close brace의 갯수가 같아야 한다는 조건을 추가한다면? regular expression의 \* 기호로는 특정 요소의 갯수를 파악하는데 한계가 있어 위와 같은 nesting 구조를 표현할 수 없다.

-> Context-free grammar는 이런 상황을 해결하기 위해 도입된 다른 형식의 문법으로, 일정한 생성 규칙에 따라 토큰의 구조를 정의한다. 많은 프로그래밍 언어가 문맥 자유 문법에 속하고 있다!

(참고) Chomsky Hierarchy : 세상의 언어를 표현하는 4단계의 grammar 정의
![image](https://user-images.githubusercontent.com/86834982/200325821-7deff989-dbe3-422c-9af2-92d4ace573f7.png){: width="90%" height="90%"}  
-> 맨 아래에 regular expression, 그리고 이것을 포괄하는 것이 context-free grammar
 


<br/>
**Context-free grammar vs. Regular Expression**

|  | Context-free grammar | Regular Expression |
| --- | --- | --- |
| generate | context-free language | regular sets |
| recognized by | parsers | scanners |
| defines | nested structure | tokens |
| recursive construct | can specify  | cannot specify  |
| expressive power | more powerful | less powerful |


<br/>
**Context-free grammar representation**  
문법을 설명하기 위해서는 Terminal , Nonterminal Symbol에 대한 개념 필요

>CFG ( *V*, *T*, *P*, *S* )
>

  V : set of **variables**, nonterminals (주로 대문자로 표현)

  T : set of **terminals** (주로 소문자로 표현)

  P : **production rules**,  substitution rules (*A* -> *B*)

  S : **start variable** (*S*)<br/><br/>
  
ex 1. **Possible strings** 찾는 예제

>*S*→->→*0S1→\|→$\epsilon$*
>

start symbol : S,→variable : S,→terminal : 0, 1, $\epsilon$

production rule : *S* -> *0**S**1,→S* -> $\epsilon$   
![image](https://user-images.githubusercontent.com/86834982/200315088-3fbd9a27-1eb3-48a7-8b2b-b30aa8c72235.png){: width="80%" height="80%"}

-> production을 사용해서 variable을 계속해서 replace 할 수 있고, terminal을 사용해서 멈출 수 있음  
-> nested model을 도출하는 것이 가능


<br/>
### Parser : LL, LR

language recognizer로, 어떤 문장을 분석하거나 문법적인 관계를 해석해주는 것을 말한다. 프로그램을 compile하는 과정에서 각 문자열을 의미있는 token으로 분해하고, 그 token이 주어진 context-free grammar를 잘 지켜서 작성했는지 검사하는 역할을 하며 **parse tree**를 최종적으로 반환한다. 


![image](https://user-images.githubusercontent.com/86834982/200318034-f2b002f8-740c-40c5-af2c-dc3f3563a4af.png){: width="80%" height="80%"}

<br/>
**LL - Top down Parser** 

predictive parser, 미리 어떤 rule을 쓸 것인지 보고 미리 결정 -> not efficient 

![image](https://user-images.githubusercontent.com/86834982/200318046-03859608-1619-4a20-8ad7-5665223f10f7.png){: width="80%" height="80%"}

<br/>
**LR - Bottom up Parser**

no prediction, LL parser 단점을 보완하고 오른쪽 아래서부터 shift-reduce 매커니즘 사용 

![image](https://user-images.githubusercontent.com/86834982/200318173-788e5a83-74b1-468f-bfa2-d2a6b5036943.png){: width="80%" height="80%"}

<br/>
**LL vs. LR parser 비교**

|  | Direction of scanning | Derivation discovered | Parse tree construction | Algortithm used |
| --- | --- | --- | --- | --- |
| LL parser | left -> right | left-most | top-down | predictive  |
| LR parser | left -> right | right-most | bottom-up | shift to stack and reduce(pop) |

<br/>
### LL Parsing

Parsing 전에 충족시켜주어야 하는 조건 :

- grammar should be **unambiguous**
- grammar should **Left -> Right Recursive**
- grammar should be **deterministic**

뒷 부분에서 조건들을 충족시키는 변환 방법에 대해 정리할 예정 !  
<br/>
**LL-parsing 과정** : 먼저 주어진 Production Rule에서 First와 Follow를 각각 구한다. 그리고 이를 바탕으로 **Parsing Table**을 생성하고 LL(1) grammar에 해당하는지 체크한다. 문법에 해당되는 것이 확인되면 생성한 parsing table을 사용해서 주어진 문자열이 LL(1) 문법에 해당하는지 확인하는 LL(1) parsing을 수행한다.   
<br/>
1. **Finding First()**
<br/>\- First of terminal - terminal
<br/>\- First of $\epsilon$ - $\epsilon$
<br/>\- **First of variable** : 각 production rule의 첫번째 요소
    
    ex. finding first 구하는 방법 (with epsilon)
    
    ![image](https://user-images.githubusercontent.com/86834982/200318229-70220a6f-0780-433a-b460-284902f002f2.png){: width="80%" height="80%"}
<br/><br/>
2. **Finding Follow()**
<br/> \- **Follow of start symbol** - $
<br/> \- Follow never returns epsilon
<br/> \- **Follow of variable** : First of next variable (RHS) !  
        -> 만약에 last element인 경우 : Follow of LHS
    
    ex. finding follow 구하는 방법 (with epsilon)
    
    ![image](https://user-images.githubusercontent.com/86834982/200318350-422c8cf1-9805-4017-9ee2-d517989fd0b1.png){: width="80%" height="80%"}
    
    ex2. first and follow 구하는 연습하기
    
    ![image](https://user-images.githubusercontent.com/86834982/200318358-fd869d5e-3739-47a2-8338-20c5c091f11b.png){: width="80%" height="80%"}
    <br/><br/>
3. **Making LL-Parsing Table**
<br/> \- 각 Production Rule에 number 작성하기
<br/> \- 각 Production Rule의 First, Follow 구하기
<br/> \- Put number in First values(terminals)
<br/> \- **no clash** 인 경우 (only one entry) -> **LL(1) grammar** !



<br/>   
<br/><br/>
[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}

