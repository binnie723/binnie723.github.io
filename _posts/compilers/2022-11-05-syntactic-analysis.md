---
title: Syntactic Analysis
subtitle: 
categories: Compilers
tags: [compilation, syntactic, analysis]
date: 2022-11-05 01:34:06 +0000
last_modified_at: 2022-11-05 01:34:06 +0000
---


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
