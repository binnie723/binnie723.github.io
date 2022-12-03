---
title: Lexical Analysis
subtitle: 
categories: Compilers
toc: true
toc_sticky: true
date: 2022-11-07 23:51:28 +0000
last_modified_at: 2022-11-07 23:51:28 +0000
---

<br/>
### Lexical Analysis

1. **tokenize** using *Regular Expression*, *Finite Automata* 
2. error message related to token (verifies)
    - exceeding length
    - unmatched string
    - illegal characters (ASCII code)
3. eliminates comment, white space (optimizes)

![image](https://user-images.githubusercontent.com/86834982/205444416-1349b3d4-a39c-4890-b4f0-e86a243f50e4.png){: width="560px"}  

컴퓨터는 사용자가 작성한 code stream을 이해할 수 없다. 따라서 컴퓨터가 이해할 수 있는 binary code로 변환시켜주는 컴파일 과정을 거친다. Hello World를 출력하는 코드를 작성했을 때 컴퓨터는 우리가 쓴 코드를 먼저 다음과 같이 받아들인다. 

![image](https://user-images.githubusercontent.com/86834982/205444417-bcb886a1-6181-4df6-970f-81eacd384ac6.png){: width="600px"}  

컴파일러가 사용자의 코드를 해석하기 위해 가장 먼저 하는 역할은 Lexical Analysis이다.  코드를 토큰 단위로 구분하고, 각 토큰이 유효한지 판별한다. 


<br/>
### Tokens

토큰은 **의미를 갖는 최소 단위의 덩어리**를 의미한다. 컴퓨터 언어에는 사전에 정의된 토큰들이 존재하는데, 종류는 크게 다음과 같다. 

- **Keywords** : specific reserved words로, identifiers 이름으로 사용될 수 없음
    ex. C언어에 정의된 32개의 keywords

    ![image](https://user-images.githubusercontent.com/86834982/205444412-26d63d8c-1068-46f0-a8e0-b6126bd3bf98.png){: width="600px"}  
    -> main : not keyword, 메인을 이름으로 한 변수 이름을 선언할 수 있기 때문  

- **Identifiers** : variable이나 function의 이름에 해당, memory space를 필요로 함  
    ex.→variable_name(x),→function_name,→sizeof, printf  등  

- Punctuators: 분리자(separators)와 연산자(operators)에 해당  
    ex.→> < + ! =  {  } (  ) [ ]  

- Constants : 상수값에 해당하는 3,→true(1),→false(0)
<br/>
- Special characters :→/,→” ” 등


<br/>
### Regular Expression

> Regular expressions are used to **represent tokens**, strings, and regular set in a regular language.
>  

문자열 코드를 token 별로 잘라주기 위해서는 특정 pattern이 필요한데, 이를 정규 표현식(regular expression)이라고 한다.   
ex.→*Yurim(string)  -  identifier(token),→ ;  -  semicolon(separator)* 


<br/>
### **Regular Expression Representation**

언어는 어떤 규칙으로 구분하고 나열되어 있는 것을 의미한다. 이것을 아래의 5가지로 적절히 조합하면 원하는 단어를 뽑아낼 수 있다. 

1. A Character
2. Empty string (₩epsilon)
3. Concatenation (RS) : R and S 둘 다 포함 
4. Alternation (R\|S) : R or S  둘 중 하나만 포함 
5. Kleen star (\*) : zero or more  있을 수도 있고 없을 수도 있고 (infinite)

    ex. regular expression으로 나타낸 예시들 
    
    ![image](https://user-images.githubusercontent.com/86834982/205445137-32997503-c8b2-4be4-aa21-d86748f90f95.png){: width="700px"}  

<br/>
### Other use of Regular Expression

Perl, Python, Ruby나 Unix에 있는 grep tool들은 이러한 regular expression을 사용해서 코드로 사용할 수 있도록 되어 있다. 유닉스의 경우를 예로 들면, 

```bash
grep -i 'linux|unix' filename  # match with word linux, unix
grep ^vivek/etc/passwd    # line starting with vivek
grep 'foo$' filename      # ending with word foo
grep '^$' filename        # search for blank lines 
```





<br/>   
<br/><br/>
[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}

