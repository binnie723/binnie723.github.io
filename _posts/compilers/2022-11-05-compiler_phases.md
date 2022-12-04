---
title: Compiler Phases
subtitle: 
categories: Compilers
tags:
  - [compiler]
toc: true
toc_sticky: true
date: 2022-11-03 23:51:28 +0000
last_modified_at: 2022-11-03 23:51:28 +0000
---


<br/>
### Compilation Phases

1. **Lexical Analysis (Lexar)**
2. **Syntactic Analysis (Parser)**
3. **Semantic Analysis** 
4. Optimization
5. Code Generation (Assembly, Object code, Linking)

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
### Post Compilation : Linking

![image](https://user-images.githubusercontent.com/86834982/205443977-f3b6df88-2d9f-4d9e-90dd-7c3cd4623624.png){: width="500px"}

1. Preprocessor : remove comments, expand macro
2. Compiler : generate assembly language (for debugging!)
3. Assembler : generate machine code 
4. **Linker** : relocation of object files and libraries, give final executable file

<br/>
 
Symbol Resolution : 각 symbol(함수, 변수) reference를 하나의 definition에 매칭하는 것.

Relocation : 재배치, symbol의 정의를 메모리에 올리고, reference가 이를 가르키도록 수정. 

![image](https://user-images.githubusercontent.com/86834982/205443978-f9e8b219-9545-4aac-9505-af7421f00961.png){: width="600px"}
  
<br/>
ex. relocation entries 

링커가 relocation entry를 발견하면, 그 symbol을 resolve하여 (빨간색 부분을) 채워넣는다.  
![image](https://user-images.githubusercontent.com/86834982/205443979-1f86693f-0b9f-43ee-8c10-9dbb3be04d8b.png){: width="600px"}
  
<br/> 
ex. relocated text section

주소로 relocation(재배치)한 결과

![image](https://user-images.githubusercontent.com/86834982/205443980-57a43685-6940-4e47-bb67-32e1cd3acc24.png){: width="600px"}
<br/><br/> 
### Static Linking vs. Dynamic Linking<br/> 
  
  
- **Static Linking** : 프로그램에서 사용하는 **라이브러리 모듈 내용을 복사**하는 방식
    
    -> exe 파일의 크기를 증가시키고, 수정할 때마다 다시 링크 작업을 수행해야 한다.
    
- **Dynamic Linking** : 프로그램에서 사용하는 **라이브러리의 주소**를 가지고 있다가, 런타임에 **해당 주소로 가서 로드**해오는 방식
    
    -> 파일의 크기를 증가시키지 않고, 메모리와 디스크의 공간을 아낄 수 있다.
    
![image](https://user-images.githubusercontent.com/86834982/205444125-ad51fde9-28c3-49a9-806b-1fd07db46577.png){: width="700px", height="302px"}



<br/>   <br/> 
<br/><br/>
[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}
