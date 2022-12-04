---
title: Context Free Grammar
subtitle: 
categories: Compilers
tags:
  - [syntax]
toc: true
toc_sticky: true
use_math: true
date: 2022-11-24 23:51:28 +0000
last_modified_at: 2022-11-24 23:51:28 +0000
---

<br/>
### Syntactic Analysis

Lexical Analysis를 통해 Regular Expression을 NFA, DFA로 나타내고 이를 통과했다면, 이후에는 Syntactic Analysis를 수행해야 한다. 이 단계에서는 파싱을 통해 token의 구조와 문법을 체크하고,  파싱 트리를 반환한다. 

모든 입력을 정규 표현식만으로 표현하기 어려운 경우가 존재한다. 예를 들어 ${ ( , ) }$ 를 포함하는 구문을 나타내면, $($\*$)$\* 로 표현할 수 있다. 이 정규식의 정의에 따르면 $((), ()))$, .. 등이 경우의 수에 포함되는데 여기서 open brace와 close brace가 같아야 한다는 조건을 추가한다면?  정규 표현식의 \* 기호만으로는 특정 요소의 갯수를 지정하는데 한계가 있다. 따라서 이와 같은 nesting 구조는 나타낼 수 없다.

-> 이러한 상황을 위해 도입된 것이 context-free grammar !


<br/>
### Context Free Grammar

> defines a set of rules, which will make strings, which collectively can be Context Free Language
> 

Context-free grammar는 위와 같은 상황을 해결하기 위해 도입된 문법으로, 일정한 규칙에 따라 **토큰의 구조**를 정의한다. 모든 생성 규칙이 $A -> x$ 의 형태를 가지고 있고, terminal와 non-terminal의 상관 관계로 이루어져 있다. 많은 프로그래밍 언어들이 문맥 자유 문법에 속하고 있다!

- Terminal : 문법에 종속적인 symbol, 상수와 유사한 역할
- Nonterminal : 문법에 독립적, 변수와 유사한 역할

<br/>
**Context-free grammar vs. Regular Expression**

|  | Context-free grammar | Regular Expression |
| --- | --- | --- |
| generate | context-free language | regular sets |
| recognized by | parsers | scanners |
| define | nested structure | tokens |
| recursive construct | can specify  | cannot specify  |
| expressive power | more powerful | less powerful |


<br/>
### Context Free Grammar Representation

*$CFG(V, T, P, S)$* = < Terminal, Nonterminal, Production, Start Symbol >

 $V$ : set of **variables**, 주로 대문자

 $T$ : set of **terminals**, 주로 소문자 

 $P$ : **production rules**,  substitution rules ($A -> B$)

 $S$ : **start variable** where we start ($S$)

<br/>
 ex. Context Free Grammar 표현된 예시

![image](https://user-images.githubusercontent.com/86834982/205468439-c90f8942-18ab-48ec-89de-9365b2014f9f.png){: width="500px"} 

여기서 $S$는 variable이자 start variable, $()$는 terminals, 그리고 production rule은 $S -> (S)$ 와 $S ->$ $\epsilon$ 부분을 말한다. 위의 예시에서 반복적으로 nonterminal이 없어질 때까지 위의 생성 규칙을 반복적으로 적용하다보면 다음과 같이 nesting 구조를 가진 문자열을 생성할 수 있게 된다. 

![image](https://user-images.githubusercontent.com/86834982/205468440-e8a032ac-3805-46ad-9b97-f7f4d833e855.png){: width="500px"} 
  
<br/>
ex. Context Free Grammar 표현된 예시 - **ambiguity !**

같은 문장에 대해서 두 개의 parse tree가 생성되는 경우 ambiguous grammar에 해당된다. 

![image](https://user-images.githubusercontent.com/86834982/205468438-0f346493-b194-486d-a7c2-c9eca06720a9.png){: width="500px"} 

  
<br/>
**Ambiguous to Unambiguous**

문법에서 ambiguity를 제거하기 위해서는 결합법칙과 연산 우선순위를 고려해주어야 한다.  
    <br/>
1. **Left associativity 좌측 결합**
    
    결합 법칙을 고려하지 않으면 여러 개의 파스 트리가 형성된다. 따라서 우선 순위가 없는 연산자의 경우, 좌측 결합 법칙을 적용해서 **왼쪽부터 연산**을 수행해야 한다. 다음과 같이 파스 트리의 구조가 왼쪽으로 뻗어나가도록 구성하기 위해서는 우측에 terminal을 배치해야 한다. 
    
    ![image](https://user-images.githubusercontent.com/86834982/205468442-a14ae9c6-614a-47bd-bd1f-46367783ae05.png){: width="500px"} 

    <br/>
2. **Operator precedence 연산자 우선 순위**
    
    우선 순위가 높은 연산자는 트리의 **아래쪽에서 먼저 수행**되도록 구성해야 한다. 따라서 이를 적용하기 위해서는 높은 우선 순위의 연산자일수록 생성 규칙의 하단에 배치한다. 다음과 같이 $T, F$ 등의 새로운 변수를 도입해서 해당 연산자가 맨아래에 오도록 생성 규칙을 바꿔준다. 
    
    ![image](https://user-images.githubusercontent.com/86834982/205468441-879c150a-08fb-4879-9951-d9eb18d88eb4.png){: width="500px"} 


<br/>   
<br/><br/>
[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}
