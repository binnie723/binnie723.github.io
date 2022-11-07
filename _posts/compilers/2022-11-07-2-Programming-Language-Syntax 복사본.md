---
title: 2. Programming Language Syntax
subtitle: 
categories: Compilers
tags: [compilation, phases]
date: 2022-11-07 01:34:06 +0000
last_modified_at: 2022-11-07 01:34:06 +0000
---

강의키워드: Lexical and Syntactic Analysis
상태: 강의요약

- How to **specify** tokens → *regular expression*
- How we **specify** structural rules → *context-free grammar*
- How compiler **identifies** the structure → is following *regular expression*?

### Compilation Phases

1. **Lexical Analysis** : **divides** into individual tokens, and **verifies tokens**
2. **Syntactic Analysis** : checks the **structure** and **grammar**  
3. Semantic Analysis : checks the **meaning** of the code  
4. Optimization : optimizes code for better matching assembly language 
5. Assembly Code  = intermediate code, assembler를 통해 변환
6. Object Code (*.o file*)
7. Linking (*.exe file*)

**Difference between Lexical and Syntactic**

|  | Lexical | Syntactic |
| --- | --- | --- |
| grammar | checks structure of lexicon(word) | checks how phrases are formed from those tokens  |
| what it does | tokenize and checks validity | checks structure and grammar  |
| returns | string of tokens (input to parser) | parse tree |
| tools | uses lexar | uses parser |

**Difference between Syntactic and Semantic** 

|  | Syntax | Semantic |
| --- | --- | --- |
| dealing with | rules of statement | meaning of statement  |
| when it runs | during compilation time  | during runtime  |
| error | compile error (= syntax error) | semantic error (= logic error) |
| what it does | checks grammar of programming language  | checks whether programming language do what is intended  |

## Lexical Analysis (Lexar, Tokenizer)

---

Lexical Analysis는 문자열을 스캔해서 **token으로 묶어내는 것**. 이렇게 생성해낸 토큰을 symbol table에 저장하고 syntactic analysis 단계로 보낸다.

1. **tokenize** using *Regular Expression*, *Finite Automata* 
2. error message related to token (verifies)
    - Exceeding length (*int xxxxxxxxxx…*)
    - Unmatched String
    - Illegal Characters (such as ASCII code)
3. eliminates comment, white space (optimizes)

### **Tokens**

basic building blocks of a program, stored in **symbol tables**

- **Keywords** : specific reserved words, cannot use for identifiers
    
    ![  ex. total 32 keywords in C programming language ](2%20Programming%20Language%20Syntax%20a62b7061c92d4887b280bfef6275f4c6/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-10-25_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB_10.26.53.png)
    
      ex. total 32 keywords in C programming language 
    
    - *main* : not keyword,  왜냐하면 main으로 된 변수를 선언 가능
- **Identifiers** : naming of variables, functions, which needs memory space
    
    ex. variable_name(*x*), function_name, *sizeof*, *printf* 
    
- Punctuators: separators and operators
    
    ex. > < + ! =  {  } (  ) [ ] 
    
- Constants : 3, *true,  false*
- Special characters : slash(/),  quotation marks(””)

### Regular Expression

> Regular expressions are used to **represent tokens**, strings, and regular set in a regular language.
> 

*Yurim(string) — identifier(token),   ; — semicolon(separator)* 

**Token을 specify한 식**으로, ****토큰을 구체적으로 수식화해서 표현한 것을 말한다. 위와 같이 입력받은 문자열로부터 토큰을 뽑아내는데 사용된다. 

**Regular Expressions**

1. ***A Character***
2. ***Empty string*** ($\epsilon$)
3. ***Concatenation*** (*RS*) : *R* and *S*  둘 다 포함 
4. ***Alternation*** (*R*|*S*) : *R* or *S*  둘 중 하나만 포함 
5. ***Kleen star*** (***) : zero or more  있을 수도 있고 없을 수도 있고 (infinite)

ex. generating rules of token

*digit → 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9*

*integer → digit | digit**

*decimal → digit* (digit. | .digit)  digit**

ex. getting possible strings

*a|b** : denotes { $*\epsilon$, “a”, “b”, “bb”, “bbb”, …* }

*(a|b)** : every string only including *“a”, “b”, with empty string*

*(ab*(c|$\epsilon$))* : string starts with *a*, zero or more *b*, and possibly *c*

### Finite Automata : DFA, NFA

> Automata is a **finite representations** of infinite formal languages.
> 

오토마타는 한 마디로  state transition diagram. 여기서는 **regular expression을** **state diagram형태**로 나타낸 것. 오토마타를 사용해서 입력으로 넣은 특정 문자열이 규칙(language)을 따르는지 확인한다. 

**Deterministic Finite Automata (DFA)**

![스크린샷 2022-10-25 오후 4.05.19.png](2%20Programming%20Language%20Syntax%20a62b7061c92d4887b280bfef6275f4c6/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-10-25_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_4.05.19.png)

next state could be **only one** state (= deterministic)

**Regular Expression Representation**

$Q$ — Set of finite state 

$\sum$ — Input symbols ,  *ex.* {*0, 1*}, {*a*, *b*}

$q_0$ — Starting state

$F$ — Set of final states  

$\delta$ — transition function  *main rule*

ex. Design a Deterministic Finite Automata to represent a language.  

$\sum$ — {*a, b*} ,  Language : “string starting with  *a*” 

- regular expression : *a* (*a*|*b*)*

![스크린샷 2022-10-25 오후 4.28.19.png](2%20Programming%20Language%20Syntax%20a62b7061c92d4887b280bfef6275f4c6/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-10-25_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_4.28.19.png)

**Non Deterministic Finite Automata**

![스크린샷 2022-10-25 오후 4.11.38.png](2%20Programming%20Language%20Syntax%20a62b7061c92d4887b280bfef6275f4c6/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-10-25_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_4.11.38.png)

next state could go to **multiple states**

ex. Design a Non Deterministic Finite Automata to represent a language.  

$\sum$ — {*0,1*} ,  Language : “binary strings that 2nd last bit is 1” 

- regular expression : (*0*|*1*)* *1* (*0*|*1*)

![스크린샷 2022-10-25 오후 7.03.29.png](2%20Programming%20Language%20Syntax%20a62b7061c92d4887b280bfef6275f4c6/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-10-25_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_7.03.29.png)

**DFA vs. NFA**

| Deterministic Finite Automata | Non-deterministic Finite Automata |
| --- | --- |
| one next state for one input | multiple next state for one input |
| no dead configuration  trap state | dead configuration defined |
| no null move  | null move possible  |
| relatively hard to design  | easy to design |

**Converting NFA to DFA**

NFA → DFA 변환하는 방법 : **Transition Table** 

ex. regular expression : (*0*|*1*)* *1* (*0*|*1*)

![스크린샷 2022-10-25 오후 7.34.30.png](2%20Programming%20Language%20Syntax%20a62b7061c92d4887b280bfef6275f4c6/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-10-25_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_7.34.30.png)

1. Make NFA table 

| NFA | 0 | 1 |
| --- | --- | --- |
| q0 | q0 | q0 q1 |
| q1 | q2 | q2 |
| q2 | - | - |
1. copy first row from NFA table
2. Make DFA table : expand elements with multiple states (*q0q1, q0q2, q0q1q2*)
    
    → NFA :  $n$ states,  DFA ****: $2^n$ states maximum ****
    

| DFA | 0 | 1 |
| --- | --- | --- |
| q0 | q0 | q0 q1 |
| q0q1 | q0q2 | q0q1q2 |
| q0q2 | q0 | q0q1 |
| q0q1q2 | q0q2 | q0q1q2 |
1. convert to DFA diagram
    
    → *q0* : starting state,   *states with* *q2* : final state  (*q0q2*, *q0q1q2*)
    
    ![스크린샷 2022-10-25 오후 7.34.38.png](2%20Programming%20Language%20Syntax%20a62b7061c92d4887b280bfef6275f4c6/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-10-25_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_7.34.38.png)
    

**Lexical Analysis** 전체적인 과정 : 

Lexical Language Specification → Regular Expression → NFA → (minimal) DFA → Generate Scanner 

## Syntactic Analysis

---

checks the structure and grammar of tokens by parser. 

### Context Free Grammar

> defines a set of rules, which will make strings, which collectively can be Context Free Language
> 

beyond regular expression, represents **structure of tokens**

영어에서의 문법과 같이, 프로그래밍 언어에서 정의된 문법 

**Context-free grammar vs. Regular Expression**

|  | Context-free grammar | Regular Expression |
| --- | --- | --- |
| generate | context-free language | regular sets |
| recognized by | parsers | scanners |
| defines | nested structure | tokens |
| recursive construct | can specify  | cannot specify  |
| expressive power | more powerful | less powerful |

**Context-free grammar representation**

CFG(*V*, *T*, *P*, *S*)

V : set of **variables**, denoted with capital letter

T : set of **terminals**, denote with small letter

P : **production rules**,  substitution rules (*A* → *B*)

S : **start variable** where we start (*S*)

*ex1*.  **Possible strings**

*S* → *0S1* | $\epsilon$ 

start symbol : S,  variable : S,  terminal : 0, 1, $\epsilon$

production rule : *S* → *0**S**1,  S* → $\epsilon$    (can **replace *S*** with production recursively)

![  → replace nested construct, and stop with terminal ](2%20Programming%20Language%20Syntax%20a62b7061c92d4887b280bfef6275f4c6/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-10-25_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_8.23.51.png)

  → replace nested construct, and stop with terminal 

*ex2*. **Context-free Grammar to Language** 

*expr* → *id* | *number* | *- expr* | *( expr )* | *expr op expr*

*op* → + | - | * | /

start symbol : *expr* ,  variable : *expr, op ,*  terminal *: id, number, (  , ) , + , - , * , /, -*

![스크린샷 2022-10-25 오후 8.24.19.png](2%20Programming%20Language%20Syntax%20a62b7061c92d4887b280bfef6275f4c6/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-10-25_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_8.24.19.png)

→ **but ambiguous grammar :** need to change to unambiguous for parsing by

1. Left associativity
2. Operator Precedence 

![스크린샷 2022-10-25 오후 8.34.47.png](2%20Programming%20Language%20Syntax%20a62b7061c92d4887b280bfef6275f4c6/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-10-25_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_8.34.47.png)

### Parser : LL, LR

language recognizer로, 여기서는 입력으로 받은 token이 주어진 context-free grammar를 잘 따르고 있는지 분석해서 **parse tree** 반환 

![스크린샷 2022-10-25 오후 8.46.58.png](2%20Programming%20Language%20Syntax%20a62b7061c92d4887b280bfef6275f4c6/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-10-25_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_8.46.58.png)

**LL Top down Parser** 

predictive parser, 미리 어떤 rule을 쓸 것인지 보고 미리 결정 → not efficient 

![스크린샷 2022-10-25 오후 9.17.03.png](2%20Programming%20Language%20Syntax%20a62b7061c92d4887b280bfef6275f4c6/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-10-25_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_9.17.03.png)

**LR Bottom up Parser**

no prediction, LL parser 단점을 보완하고 오른쪽 아래서부터 shift-reduce 매커니즘 사용 

![스크린샷 2022-10-25 오후 10.49.04.png](2%20Programming%20Language%20Syntax%20a62b7061c92d4887b280bfef6275f4c6/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-10-25_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_10.49.04.png)

**LL vs. LR**

|  | Direction of scanning | Derivation discovered | Parse tree construction | Algortithm used |
| --- | --- | --- | --- | --- |
| LL parser | left → right | left-most | top-down | predictive  |
| LR parser | left → right | right-most | bottom-up | shift to stack and reduce(pop) |

### LL Parsing

Parsing 전에 충족시켜주어야 하는 조건 :

- grammar should be **unambiguous**
- grammar should **Left → Right Recursive**
- grammar should be **deterministic**

**LL-parsing 과정** : 

1. calculate **First** for every production rule, **Follow** for every variable
2. make **Parsing Table**
3. check if grammar is LL(1)

**Finding First()**

- First of terminal - terminal
- First of $\epsilon$ - $\epsilon$
- **First of variable** : 각 production rule의 첫 terminal
    
    ex. finding first with epsilon 
    
    ![스크린샷 2022-10-25 오후 11.32.06.png](2%20Programming%20Language%20Syntax%20a62b7061c92d4887b280bfef6275f4c6/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-10-25_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_11.32.06.png)
    

**Finding Follow()**

- **Follow of start symbol** - $
- Follow never returns   
$\epsilon$
- **Follow of variable** : First of next variable (RHS)
    
    → if is last element : Follow of LHS
    
    ex. finding follow with epsilon 
    
    ![스크린샷 2022-10-25 오후 11.39.02.png](2%20Programming%20Language%20Syntax%20a62b7061c92d4887b280bfef6275f4c6/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-10-25_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_11.39.02.png)
    
    ex2. finding both first and follow 
    
    ![스크린샷 2022-10-25 오후 11.39.40.png](2%20Programming%20Language%20Syntax%20a62b7061c92d4887b280bfef6275f4c6/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-10-25_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_11.39.40.png)
    

**Finding LL-Parsing Table**

1. Write **number** for each Production Rule
2. Calculate **First** of each Production Rule 
3. Put number in First values(terminals)
4. If **no clash** (only one entry) : is **LL(1) grammar** 

|  | terminals |  | $ |
| --- | --- | --- | --- |
| variables | 1 | 2, 4 (clash) |  |
|  |  |  |  |

**Making Parse Tree from LL-Parsing Table**