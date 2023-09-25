---
title:  "Machine-level 프로그래밍 : Memory Layout, Buffer Overflow" 

categories:
  - Computer Systems
tags:
  - [memory, stack, buffer]
toc: true
toc_sticky: true
use_math: true
date: 2023-06-12
last_modified_at: 2022-06-12
---
<br/> 

## Memory Layout

---

이번 단원은 메모리의 구조와 스택에서 발생할 수 있는 버퍼 오버플로우에 대한 내용이다. 

### x86-64 Linux Memory Layout

프로그램을 실행시키면 OS는 실행 파일을 메모리에 로드하면서, 여러 개의 독립적인 영역을 생성한다. 할당되는 메모리 구조는 다음과 같다. 

![image](https://github.com/binnie723/binnie723.github.io/assets/86834982/3a71513c-060a-4a2c-8b23-2782a44de918){: width="480px"}

ex. 실제로 할당되는 예시를 나타낸 그림 

![image](https://github.com/binnie723/binnie723.github.io/assets/86834982/7b79e34f-63cd-499f-95e4-d7ec7a62bfbe){: width="480px"}
<br/>
## Buffer Overflow

---

### Memory Referencing Bug : segfault

아래 코드는 buffer overflow를 발생시키는 예시 코드이다. 

`fun(i)` 함수는 a[i]에 접근해서 배열의 값을 할당하고, 리턴 값으로 실수 값 d를 반환한다. 

![image](https://github.com/binnie723/binnie723.github.io/assets/86834982/4df038b0-d1ed-459b-9c33-66d519733d16){: width="480px"}

만약 i가 0 또는 1인 경우에는 d의 값을 3.14로 정상 출력하지만, 2 이상인 경우에는 a배열의 인덱스 범위를 초과하게 되면서 memory reference bug를 유발한다. 

![image](https://github.com/binnie723/binnie723.github.io/assets/86834982/198ea991-3927-4f78-935b-0cdcbe0e4b08){: width="480px"}

`i = 2,3` : a 배열의 버퍼를 초과하면서 d의 값을 overwrite

`i = 4` : stack이 할당한 extra 공간에 저장, overwrite가 발생하지 않음

`i = 6` : i가 많이 큰 경우, stack의 범위를 완전히 벗어나면서 segfault 발생

->  이 시점부터 critical state이라고 부른다. 


👩🏻‍💻 Segmentation fault는 프로그램에 속하지 않은 메모리 위치를 접근하려고 할 때 발생하게 됨. <br/>하지만 런타임 스택은 return address를 저장하기 위해 추가 공간을 할당하기 때문에 바로 에러가 발생하지는 않음.
{: .notice--primary}
<br/>
이러한 상황을 **buffer overflow 현상**이라고 한다. 

- 일반적인 현상은 입력 string의 길이를 제대로 확인하지 않는 경우
    
    스택 내에 정해진 string 배열의 범위를 초과하는 입력을 넣게 되면, 인접한 메모리 영역이나 return address를 overwrite하면서 예상치 못한 문제가 발생할 수 있다. 
    
- **Stack Smashing** 공격 가능
    
    이 현상을 악용해서 해커들은 buffer의 경계를 초과하는 입력을 정교하게 조작해서 return address를 악의적인 코드로 덮어 씌워서 실행시킬 수 있다. 
    
    -> 따라서 buffer overflow를 발생시키지 않도록 주의해서 코드를 구성해야 함!
    
    - worms : 자체적으로 실행가능한 악성 소프트웨어로, 네트워크로 자동 전파
    - viruses : 다른 호스트 프로그램에 의존해서 전파되는 소프트웨어

<br/>  
### Protection Mechanisms

buffer overflow를 예방하기 위해서는 크게 세 가지 방법이 있다. 

1. **string 입력을 제한하는 Library routines**
    
    ex.  `fgets()`, `strncpy`, `scanf(%ns)`를 사용해서 코드를 구현
    
    ![image](https://github.com/binnie723/binnie723.github.io/assets/86834982/1ea4867f-e16b-4d48-9170-5f2fe0574b0d){: width="430px"}
    
2. **System-level Protection**
    
    코드 상 구현이 아닌, 시스템 내부에서 알아서 방지하도록 구성하는 방법들도 있다.
    
    - **Address Space Layout Randomization (ASLR)**
        
        프로그램이 실행될 때마다 stack 영역이 random allocation되기 때문에 해커들은 주입한 exploit 코드의 위치(B)를 예측하기 어려워진다.  
        
    - **Non-executable code segments**
        
        x86-64부터 “execute” 권한이 생기면서 stack 영역 일부에 실행 권한을 설정하지 않을 수 있다. 해커들이 코드를 주입하더라도 실행시킬 수 없도록 만든다. 
        
        
        👩🏻‍💻 하지만 이후 해커들은 새로운 코드를 주입하지 않고 기존의 코드를 변형시키는 Return-Oriented Programming 기법으로 발전
        {: .notice--primary}
        <br/>
3. **Stack Canaries** 
    
    스택에 있는 버퍼 뒤에 특정 값을 지정해서, 함수를 종료 시점에 해당 값이 corrupt 되었는지 사전에 확인하는 매커니즘이다. 
    
    `gcc -fstack-protector` 플래그를 통해 canary를 코드에 포함시킬 수 있다. 
    
    canary가 작동하는 과정은 다음과 같다.
    
    ![image](https://github.com/binnie723/binnie723.github.io/assets/86834982/c3dbc823-084e-4579-98c6-d2620d3790d8){: width="440px"}
    
    ![image](https://github.com/binnie723/binnie723.github.io/assets/86834982/ee8a03e3-e653-4ecb-9161-95d1d266e464){: width="440px"}
    
<br/>
### Attack Mechanisms : ROP

대표적인 attacking 매커니즘은 두 가지로 정리할 수 있다.

1. **Code injection**을 통한 악성 코드 주입
    
    -> stack randomization과 실행 권한을 설정하는 보안 기법 등으로 방지
    
2. **Return-Oriented Programming** : 기존의 코드를 활용하는 방법
    
    표준 라이브러리나 기존 코드에 있는 segment와 gadget을 활용한다. 해커들은 여러 가젯들을 연결해서 새로운 코드를 삽입하지 않고도 원하는 명령을 실행한다. 
    

    👩🏻‍💻 Gadget은 ret(return)으로 끝나는 명령어의 나열 혹은 조각을 말한다. <br/>실행 파일 코드 영역에서 가젯의 위치는 실행마다 일정하게 고정되어 있기 때문에 해커가 ROP chain을 구성하기에 잘 사용된다.
    {: .notice--primary}  

    
    -> 하지만 이 기법 역시 stack canary 기법으로 방지할 수 있다. 
    
<br/>
**Gadget을 통해 변형되는 예시**

ab_plus_c 함수와 setval 함수의 마지막 부분을 잘라서 gadget을 생성하고, 이들을 연결해서 실행시켜서 새로운 명령어를 생성한다.  ab_plus_c에서 lea

로 계산된 %rax의 값을 이후 %rdi에 넣는 것으로 변형되었다. 

![image](https://github.com/binnie723/binnie723.github.io/assets/86834982/ba144bf1-d226-4856-8f69-6b10d1abc64e){: width="480px"}

정리하면, 각 코드 영역 중에서 마지막 부분에 ret을 포함하도록 잘라서 생성한 gadget들을 조합하여 새로운 명령어를 만든다. ret을 통해 다음 gadget의 주소로 점프하게 되고 전체적인 Gadget Chain을 형성된다. 

![image](https://github.com/binnie723/binnie723.github.io/assets/86834982/ba5fc0bf-2c9a-4666-970f-2df1788a1959){: width="480px"}

<br/>  

<br/> <br/> 
@Computer Systems a programmer’s perspective third edition 내용을 참고함
  
  
    
<br/><br/>
[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}
