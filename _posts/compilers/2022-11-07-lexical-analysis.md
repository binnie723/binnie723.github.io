---
title: Lexical Analysis
subtitle: 
categories: Compilers
tags: [lexical, analysis, regular, expression]
toc: true
toc_sticky: true
use_math: true
date: 2022-11-04 01:34:06 +0000
last_modified_at: 2022-11-04 01:34:06 +0000
---


<br/>
### Compilation Phases

1. **Lexical Analysis** : 1. **divides** into individual tokens  2. **verifies tokens**
2. **Syntactic Analysis** : checks the **structure** and **grammar**  
3. Semantic Analysis : checks the **meaning** of the code  
4. Optimization : optimizes code for better matching assembly language 
5. Assembly Code  : intermediate code, assembler를 통해 변환
6. Object Code (*.o file*)
7. Linking (*.exe file*)

<br/>
**Lexical vs. Syntactic Analysis**

|  | Lexical | Syntactic |
| --- | --- | --- |
| grammar | checks structure of lexicon(word) | checks how phrases are formed from those tokens  |
| what it does | tokenize and checks validity | checks structure and grammar  |
| returns | string of tokens (input to parser) | parse tree |
| tools | uses lexar | uses parser |

<br/>
**Syntactic vs. Semantic Analysis** 

|  | Syntax | Semantic |
| --- | --- | --- |
| dealing with | rules of statement | meaning of statement  |
| when it runs | during compilation time  | during runtime  |
| error | compile error (= syntax error) | semantic error (= logic error) |
| what it does | checks grammar of programming language  | checks whether programming language do what is intended  |


<br/>
## Lexical Analysis (Lexar, Tokenizer)

Lexical Analysis는 문자열을 스캔해서 **token으로 묶어내는 것**. 이렇게 생성해낸 토큰을 symbol table에 저장하고 syntactic analysis 단계로 보낸다.

1. **tokenize** using *Regular Expression*, *Finite Automata* 
2. error message related to token (verifies)
    - Exceeding length (*int xxxxxxxxxx…*)
    - Unmatched String
    - Illegal Characters (such as ASCII code)
3. eliminates comment, white space (optimizes)

<br/>
### **Tokens**

basic building blocks of a program, stored in **symbol tables**

- **Keywords** : specific reserved words, cannot use for identifiers
    
    ![image](2%20Programming%20Language%20Syntax%20a62b7061c92d4887b280bfef6275f4c6/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-10-25_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB_10.26.53.png)
    
    ex.→total 32 keywords in C programming language 
    
    -> *main* : not keyword,  왜냐하면 main으로 된 변수를 선언 가능
- **Identifiers** : naming of variables, functions, which needs memory space
    
    ex.→variable_name(*x*),→function_name,→*sizeof*,→*printf* 
    
- Punctuators: separators and operators
    
    ex.→>→<→+→!→=→{  }→(  )→[ ] 
    
- Constants : 3, *true,  false*
- Special characters : slash(/),  quotation marks(””)

<br/>
### Regular Expression

> Regular expressions are used to **represent tokens**, strings, and regular set in a regular language.
> 

정규 표현식은 토큰을 구체적으로 수식화(specify)해서 표현한 것으로, 입력받은 문자열로부터 토큰을 뽑아내는데 사용.  

ex. s*"Yurim"*  (string)→-→identifier (token)


<br/>
**Regular Expressions**

1. *A Character*
2. *Empty string* ($\epsilon$)
3. *Concatenation* (*RS*) : *R* and *S*  ,  둘 다 포함 
4. *Alternation* (*R\|S*) : *R* or *S*  ,  둘 중 하나만 포함 
5. *Kleen star* (\*) : zero or more , 있을 수도 있고 없을 수도 있고 (infinite)
<br/>
<br/>

- token -> regular expression 나타내기

    digit → *0 \| 1 \| 2 \| 3 \| 4 \| 5 \| 6 \| 7 \| 8 \| 9*

    integer → *digit \| digit*

    decimal → *digit (digit . \| . digit)  digit*

- regular expression -> 가능한 문자열 

    *a* \| *b* → { $\epsilon$, “a”, “b”, “bb”, “bbb”, … }

    (*a* \| *b*)\* → *a*, *b* 로만 이루어진 문자열,  empty string 포함

    (*ab*\* (*c* \| *$\epsilon$*)) → *a* 로 시작하는 문자열,   *b* (zero or more),  *c* (zero or one)

<br/>
### Finite Automata : DFA, NFA

> Automata is a **finite representations** of infinite formal languages.
> 

오토마타는 한 마디로  state transition diagram. 여기서는 **regular expression을** **state diagram형태**로 나타낸 것. 오토마타를 사용해서 입력으로 넣은 특정 문자열이 규칙(language)을 따르는지 확인한다. 

<br/>
**Deterministic Finite Automata (DFA)**

![스크린샷 2022-10-25 오후 4.05.19.png](2%20Programming%20Language%20Syntax%20a62b7061c92d4887b280bfef6275f4c6/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-10-25_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_4.05.19.png)

next state could be **only one** state (= deterministic)

<br/>
**Regular Expression Representation**

 $Q$ — Set of finite state 

 $\sum$ — Input symbols→*ex.* {*0, 1*}, {*a*, *b*}

 $q_0$ — Starting state

 $F$ — Set of final states  

 $\delta$ — transition function  *main rule*

<br/>
ex. design a Deterministic Finite Automata  

$\sum$ - {*a, b*} ,→Language : string starting with  *a*

regular expression : *a* (*a* \| *b*)*

![image](2%20Programming%20Language%20Syntax%20a62b7061c92d4887b280bfef6275f4c6/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-10-25_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_4.28.19.png)

<br/>
**Non Deterministic Finite Automata**

![스크린샷 2022-10-25 오후 4.11.38.png](2%20Programming%20Language%20Syntax%20a62b7061c92d4887b280bfef6275f4c6/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-10-25_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_4.11.38.png)

next state could go to **multiple states**

<br/>
ex. design a Non Deterministic Finite Automata

$\sum$ - {*0,1*} ,→Language : binary strings that 2nd last bit is 1

regular expression : (*0* \| *1*)* *1* (*0* \| *1*)

![스크린샷 2022-10-25 오후 7.03.29.png](2%20Programming%20Language%20Syntax%20a62b7061c92d4887b280bfef6275f4c6/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-10-25_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_7.03.29.png)

<br/>
**DFA vs. NFA**

| Deterministic Finite Automata | Non-deterministic Finite Automata |
| --- | --- |
| one next state for one input | multiple next state for one input |
| no dead configuration  trap state | dead configuration defined |
| no null move  | null move possible  |
| relatively hard to design  | easy to design |

<br/>
**Converting NFA to DFA**

**Transition Table**을 사용해서 NFA -> DFA 변환하는 방법

ex. regular expression : (*0* \| *1*)* *1* (*0* \| *1*)

![image](2%20Programming%20Language%20Syntax%20a62b7061c92d4887b280bfef6275f4c6/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-10-25_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_7.34.30.png)


1. Make NFA table 

    | NFA | 0 | 1 |
    | --- | --- | --- |
    | q0 | q0 | q0 q1 |
    | q1 | q2 | q2 |
    | q2 | - | - |

1. copy first row from NFA table
2. Make DFA table : expand elements with multiple states (*q0q1, q0q2, q0q1q2*)
    
    -> NFA :  $n$ states,→DFA : $2^n$ states maximum 
    

    | DFA | 0 | 1 |
    | --- | --- | --- |
    | q0 | q0 | q0 q1 |
    | q0q1 | q0q2 | q0q1q2 |
    | q0q2 | q0 | q0q1 |
    | q0q1q2 | q0q2 | q0q1q2 |

3. convert to DFA diagram
    
    -> *q0* : starting state,→*q2* : final state  (*q0q2*, *q0q1q2*)
    
    ![image](2%20Programming%20Language%20Syntax%20a62b7061c92d4887b280bfef6275f4c6/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-10-25_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_7.34.38.png)
    

<br/>
* **Lexical Analysis** 전체적인 과정을 정리하면,  

    Lexical Language Specification -> Regular Expression -> NFA -> (minimal) DFA -> Generate Scanner 


<br/>   
<br/><br/>
[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}


