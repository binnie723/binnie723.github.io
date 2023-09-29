---
title:  "Bits, Bytes, and Integers의 표현" 

categories:
  - Computer Systems
tags:
  - [bit, byte, integers]
toc: true
toc_sticky: true
use_math: true
date: 2023-03-23
last_modified_at: 2022-03-23
---

<br/> 
<br/>  

## Representing information as bits

---

### Everything is bits

> *bit = 0 or 1*
> 

컴퓨터는 모든 데이터를 비트(bit) 단위로 표현하고 처리한다. 우리에게 익숙한 십진수(decimal )를 다루는 것은 복잡하기 때문에 이진 형태로 표현되어 왔다고 한다. 


<br/>  
### Encoding Byte Values

> *byte = 8bits*
> 

컴퓨터는 각각의 비트에 하나씩 접근하지 않고, 8개씩 블럭 단위로 정보를 불러온다. 이것을 바이트라고 하며, 참조할 수 있는(addressable) 가장 작은 메모리 단위라고도 한다. 


- 2진법 (Binary) : $00000000_2$ 부터 $11111111_2$
- 10진법 (Decimal) : $ 0\_{10} $ 부터 $ 255\_{10} $

하지만 이진법으로 표현하면 값이 너무 방대하고, 십진법은 변환 과정이 복잡하기 때문에, 두 가지 방법 모두 데이터를 표현하는데 있어 비효율적이다. 

![image](https://user-images.githubusercontent.com/86834982/227836494-b8b4abe3-a5e5-4067-be56-131526874ec0.png){: width="450px"} 

- **16진법** (**Hexadecimal)**
    - *0x, 0X*으로 시작하는 숫자들은 16진법으로 해석
    - 이진법과 4자리씩 mapping 되기 때문에 변환 과정이 간편하다
    
    ![image](https://user-images.githubusercontent.com/86834982/227836048-a8857da7-b439-4a57-a447-6694d1b95e2a.jpg){: width="520px"} 


<br/>  
### Data Size( = Word Size)

모든 컴퓨터는 word size가 존재한다. 여기서 워드 크기는 :

- 컴퓨터 CPU가 **하나의 명령당 처리**할 수 있는 비트의 크기
- 컴퓨터 **메모리 주소 하나**를 나타내는 비트의 크기

컴퓨터의 종류에는 32bit와 64bit 짜리가 있는데, 이 숫자는 word size를 의미한다. 32bit 컴퓨터의 경우, 32bit 만큼의 크기인 레지스터를 가지고 있으며, 총 $2^{32}$byte(= $4$GB)만큼의 메모리 크기를 가질 수 있다. 



<br/>  
### Data Representations

컴퓨터는 각기 다른 방식과 길이로 데이터를 인코딩하여 저장한다. 

![image](https://user-images.githubusercontent.com/86834982/227835375-87f8fc50-a598-4cd0-a156-93fff7e2696a.png){: width="470px"} 

->  long과 pointer 변수의 경우, 32bit와 64bit에서 각각 4, 8 바이트씩 다르게 할당된다. 


<br/> 
<br/>  
### Byte Ordering

메모리에 데이터를 어디부터 먼저 저장할 것인지, 저장하는 순서에 따라 구분된다. 

- **Big Endian** : 첫번째 주소에 가장 큰 값(상위 바이트)을 먼저 저장하는 방식
    
     -> Sun, PPC Mac, Internet에 사용 
    
- **Little Endian** : 첫번째 주소에 가장 작은 값(하위 바이트)을 먼저 저장하는 방식
    
    ->  x86, ARM processor(주로 스마트폰, 테블릿에 쓰이는 모바일 프로세서)에 사용
    

만약, 0x100이라는 메모리 주소에 0x01234567이라는 정수를 저장한다고 하면, 

![image](https://user-images.githubusercontent.com/86834982/227835380-fdac3560-9d3f-4e6d-80ef-4e925d7f0dc3.png){: width="520px"} 

다음과 같이 메모리 주소 하나당 1byte가 할당되기 때문에 2개씩 묶어서 저장한다. 

**byte representation** - 확인해볼 수 있는 코드 

- *integer, float* : 기기에 따라 특정 순서(big, little endian)로 저장
- *pointers* : 기기에 따라 혹은 실행할 때마다 각기 다른 메모리 주소에 저장
- *strings* : 모든 기기에서 동일한 순서로 저장
    - 마지막에  문자열의 종료를 의미하는 00를 붙여서 저장한다.
    
    ex.  “12345” -> 31 32 33 34 35 00
    

```c
/* show-bytes - prints byte representation of data */
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef unsigned char* byte_pointer;

void show_bytes(byte_pointer start, size_t len) {
    size_t i;
    for (i = 0; i < len; i++)
        printf("%p\t0x%.2x\n", &start[i], start[i]);
    printf("\n");
}

int main(int argc, char* argv[])
{
    //int a = 0x12345678;
    int a = 2607352;
    printf("hexadecimal value : %.2x\n", a);
    printf("integer value : %d\n", a);

    show_bytes((byte_pointer)&a, sizeof(a));

    float f = (float)a;  // integer -> float 변환
    printf("floating point value: %f\n", f);

    show_bytes((byte_pointer)&f, sizeof(f));

    const char* m = "abcz";
    printf("character arra : %s\n", m);
    show_bytes((byte_pointer)m, strlen(m));

    return 0;
}
```
<br/>
![image](https://user-images.githubusercontent.com/86834982/227835422-a2fb1d18-0896-4108-8b59-dd98a6b41c3a.png){: width="450px"} 

위에 출력된 결과를 보면 integer -> float 타입으로 변환을 하면 숫자와 ordering 방식은 유지가 되어도, 16진수 표기가 완전히 다른 것을 확인할 수 있다. 그 이유는 integer를 표현하는 방식과 floating point를 표현하는 방식이 다르기 때문인데 

![image](https://user-images.githubusercontent.com/86834982/227835638-4c3fa7b8-d283-4ff6-b58d-c2baa69e678a.png){: width="450px"} 

챗 GPT에게 확인 받은 이유는 다음과 같다. 위의 내용을 정리하면 :

- `integer` : 2’s complement 방식을 사용해서 표현
- `float` : IEEE 754 standard 방식을 사용해서 표현

따라서 같은 숫자더라도 완전히 다른 bit pattern과 hexadecimal notation을 갖게 되는 것!

  
<br/> 
<br/>  
## Bit-level manipulations

---

과거의 개발자는 직접 비트 단위 연산을 사용해서 복잡한 연산을 더 빠르고 효율적으로 수행하도록 해야 했다. 하지만 현재는 하드웨어의 발달로, 비트 단위 연산 없이도 빠른 프로그램을 작성할 수 있게 되어 비트 단위 연산의 필요성이 많이 사라진 상태이다. 

하지만 시스템 하드웨어 프로그래밍의 경우, 메모리 공간 축소와 성능 향상을 위해 아직까지 사용되는 경우가 간혹 있다.

<br/> 
<br/>  
### Bit operations

| & (AND) | | (OR) | ^ (XOR) | ~ (NOT) |
| --- | --- | --- | --- |
| Intersection | Union | Symmetric Difference | Complement  |
| 두 개 모두 참일 때만 참 | 둘 중 하나라도 참이면 참  | OR 연산에서 둘 다 참인 경우 제외 | 참이면 거짓, 거짓이면 참  |

->   논리 연산자( $ \&\&,  \|\|,  ! $ ) 와의 구분 필요

논리 연산자는 0x00(False) 혹은 0x01(True)의 값으로만 반환한다


<br/> 
<br/>  
### Shift operations

- **Left Shift** ($x << y$)  : 비트를 왼쪽으로 이동시키는 연산자
    
    가장 왼쪽 비트가 이동 갯수만큼 지워지고, 비워진 뒷부분이 0으로 채워진다. 
    
- **Right Shift** ($x >> y$) : 비트를 오른쪽으로 이동시키는 연산자
    
    가장 오른쪽 비트가 이동 갯수만큼 지워지고, 비워진 앞부분이 다음 두 가지 매커니즘으로 나뉘어 채워진다. 
    
    - **Logical Shift** : 부호를 고려하지 않는 경우에 사용하며, 0으로 채운다.
    - **Arithmetic Shift** : 부호를 고려해야 하는 경에 사용하며, 부호 비트(most significant bit)의 숫자로 채운다.
    - **Undefined Behavior** : shift 사이즈가 0보다 작거나 word 사이즈보다 큰 경우
    
    ![image](https://user-images.githubusercontent.com/86834982/227835432-7f84a9e5-401a-4b45-a453-d7e6261f0c40.png){: width="370px"} 




<br/> 
<br/>  
## Integers

---

### Representation : unsigned vs. signed

정수는 다음과 같이 부호를 포함하는 수와 포함하지 않는 수로 나눌 수 있다. 

**unsigned** : non-negative, 0과 양의 정수만 포함 

**signed** : positive, zero, negative, 모든 범위를 포함하는 정수   
<br/>  
여기서 signed 정수를 이진법으로 표현하는 방법에는 두 가지가 있는데

- **1’s complement** (1의 보수) : 더해서 1이 되는 수로, 0을 1로, 1을 0으로 바꿔서 구할 수 있다
    
- **2’s complement** (2의 보수) : 더해서 2가 되는 수로, 1의 보수에 1을 더해서 구할 수 있다
    
    ![image](https://user-images.githubusercontent.com/86834982/227836150-3a43c420-85a6-415f-896e-ff6b2771402a.jpg){: width="520px"} 
    

하지만 위와 같이 1의 보수에서는 0이 두 개 존재하는 반면, 2의 보수는 하나로 통일되게 표현된다. 또한 연산 과정이 2의 보수가 훨씬 직관적이고 간편하기 때문에 실제로 2’s complement 방식을 더 많이 사용한다. 


<br/> 
<br/>  
### Encoding Integers

정수를 인코딩, 즉 컴퓨터가 처리할 수 있도록 변환하는 방법에는 :

- **Binary to Unsigned (B2U)**
    
    부호를 고려하지 않는 경우, 각 자리 수에 맞게 $2^{i}$를 곱해서 더하면 된다.  
    
    $$
    B2U(X) = $\sum^{w-1}_{i=0}x_i*2^{i}$
    $$
    
    위의 공식을 적용했을 때, unsigned가 가질 수 있는 최솟값과 최댓값은 다음과 같다. 
    
    - $UMin = 0 (000..0)$
    - $UMax =2^{w}-1 (111...1)$  
<br/>    
- **Binary to Two’s Complement (B2T)**
    
    부호를 고려하는 경우, 맨 앞자리 수는 부호 비트(Most Significant Bit)가 된다. 따라서 맨 앞자리 숫자는 부호에 맞게 계산을 하고, 이를 제외한 나머지에 대해  $2^{i}$를 곱해서 전부 더해주면 된다. 
    
    $$
    B2T(X) = -x_{w-1}*2^{w-1} + \sum_{i=0}^{w-2}x_i*2^{i}
    $$
    
    위의 공식을 적용했을 때, signed가 가질 수 있는 최솟값과 최댓값은 다음과 같다. 
    
    - $TMin = -2^{w-1}(100..0)$
    - $TMax = 2^{w-1}-1 (011..1)$  

<br/>
두 가지 공식을 통해  추가적으로 도출할 수 있는 것은 :

1. signed와 unsigned로 표현할 수 있는 전체 수의 범위는 같다.
    
    $2*TMax + 1= UMax$
    
2. signed에서 양수와 음수의 범위가 비대칭적(asymmetric)이다.
    
    $\|TMin\| = \|TMax\|+1$
    
    -> 양의 범위에 0이 포함되어 있기 때문에 음수에 비해 한 개가 부족하다.
    
3. $-1$은 $UMax$와 같은 표현이 되고, $0$은 두 가지 표현에서 모두 동일하다.

위의 함수를 역으로 계산하는 것을 $U2B(X)$, $T2B(X)$ 로 나타낼 수 있다. 정확히는 이진수로 바꾸는 이 과정이 인코딩에 해당된다!


<br/> 
<br/>  
### Conversion and Casting

정수를 선언했을 때, 상황에 따라 unsigned와 signed로 형변환을 시켜주는 경우가 생긴다.

정수의 타입을 casting하는 과정을 수학적인 관점에서 살펴보면,

![image](https://user-images.githubusercontent.com/86834982/227835510-206359a1-5bdb-47f0-8384-f79dd9735d76.png){: width="470px"} 

먼저 binary 형태로 정수를 바꾼 후, 원하는 타입에 맞게 캐스팅하는 과정으로 진행된다.  

<br/>
**unsigned <-> signed(2’s complement) 변환 관계**

양의 범위에서는 동일하게 변환되지만, 음의 범위의 경우 큰 양수로 변환된다. 

![image](https://user-images.githubusercontent.com/86834982/227835773-9cc9b35a-81c0-47e4-bb9d-c712912ada66.png){: width="470px"} 

( small tip : $2^w$를 더하거나 빼면 빠르게 음수를 변환할 수 있다. )

- **Constant** : 보통 signed로 선언되며, 뒤에 U를 붙이면 unsigned로 선언할 수 있다.
    
    ex.  $0U$, $4279979U$  -> unsigned !
    
- **Casting** :
    - Explicit Casting : `(int) ux` , `(unsigned) tx`
    - Implicit Casting : `tx = ux`, `uy = ty`
    
    -> 기본적으로 **unsigned가 우선순위**를 가져가기 때문에 둘을 섞어서 사용하는 경우, 컴퓨터는 내부적으로 전부 unsigned로 변환해서 처리한다.  
    

- unsigned int를 굳이 사용해야 하는 이유

![image](https://user-images.githubusercontent.com/86834982/227835586-43684248-7f1c-4e62-8bff-a3174cd26466.png){: width="470px"} 

그렇다고 한다. 그래서 unsigned 변환을 잘 지원해주는 C를 로우레벨 프로그래밍에서 많이 사용하고, 보통의 프로그래밍에서는 그냥 signed를 사용하기 때문에 파이썬을 주로 쓰는 것 같다. (파이썬에서도 numpy 라이브러리에서 unsigned 변환을 지원하기는 한다. )


<br/> 
<br/>  
### Expanding and Truncating

word size가 커지거나 작아질 경우, 비트가 어떻게 확장되고 축소되는지

- **Bits Expanding**
    
    늘어난 bit를 모두 0 또는 1인 sign 비트로 채워서 확장시킨다. 비트 수를 늘려도 계산하면 같은 숫자 결과 값이 도출된다. 
    
    ![image](https://user-images.githubusercontent.com/86834982/227835849-19ae5c56-7b81-4544-a8a7-305360babf21.png){: width="470px"} 
    
    예를 들어 $15213$을 short int에서 int로 바꾼다고 하면, 늘어난 비트를 모두 $0$으로 채우고, $-15213$의 경우 모두 $1$로 채운다. 
    
    ![image](https://user-images.githubusercontent.com/86834982/227835856-7799ff26-701e-45cc-b366-1f3c3b97fa68.png){: width="470px"} 
    

- **Bits Truncating**
    
    비트의 크기를 줄이는 경우, 부호 비트가 사라지기 때문에 값이 완전히 바뀔 수 있다. 음수에서 양수로 바뀌기도 하고 양수에서 음수로 바뀌기도 한다. 
    
    따라서 바뀌는 수가 무엇인지 알 수 있는 연산 과정이 필요하다. 
    
    - **unsigned** : (해당 수)  *mod*  $2^{w-1}$ 를 계산
    - **signed** : *mod*와 비슷한 연산
    
    -> 작은 수의 경우에는 값이 그대로 유지된다. 
    
<br/> 
<br/>  
### Addition, Negation, Multiplication, Shifting

- **Addition** : binary 형태로 표현한 후 덧셈 (carrying bit, MSB 무시)
    - **Unsigned Addition (UAdd)**
        
        ![image](https://user-images.githubusercontent.com/86834982/227835689-48bece01-ee58-4d9e-8bec-9465ac5d3e62.png){: width="470px"} 

        만약 true sum >= $2^w$인 경우, overflow 발생
        
        **(overflow)** :  $UAdd_w(u, v) = u + v$ *mod*  $2^w$
        
    <br/>
    - **Signed Addition (TAdd)**
        
        ![image](https://user-images.githubusercontent.com/86834982/227835694-5139ded7-ff43-4bd7-b795-42c697464c89.png){: width="470px"} 
        
        만약 true sum >= $2^{w-1}$인 경우 overflow, true sum <= $-2^{w-1}$은 underflow
        
        **(overflow)** :  $TAdd_w(u, v) =$  $- (u + v$  *mod*  $2^w)$  
        **(underflow)** :  $TAdd_w(u, v) =$  $- (u + v)$   *mod*  $2^w$
        <br/>  
        **signed**의 경우, 오버 플로우와 언더 플로우 덧셈을 계산하는 예시 : 
        
        ![image](https://user-images.githubusercontent.com/86834982/227836363-2aa7bb5c-000f-43d2-a980-1632ee2376f9.jpg){: width="470px"}   
        <br/>
    - **Signed와 Unsigned의 변환**
        
        unsigned와 signed 변수를 conversion을 해도 결과는 같다. 
        
        ```c
        #include <stdio.h>
        #include <stdbool.h>
        
        int s, t, u, v;  // all signed value
        bool is_true;
        
        s = (int)((unsigned)u + (unsigned)v); // -> unsigned -> signed
        t = u + v;  // signed
        is_true = (t == s)  // true
        ```
        
<br/>    
- **Multiplication** : binary 형태로 표현한 후 곱셈 (w bits 무시)
    - **Unsigned Multiplication** **(UMult)**
        
        곱셈 연산을 하면 결과는 기존의 비트의 두 배인 $2*w$ bits를 차지하게 된다. 
        <br/>
        **(overflow)** :  $UMult_w(u, v) = u * v$ mod $2^{w}$  
        
        ![image](https://user-images.githubusercontent.com/86834982/227835909-7d2e82b3-809b-4106-bba6-f22f46ec88eb.png){: width="470px"} 
        
        
    
    - **Signed Multiplication (TMult)**
        
        **(overflow)** :  $UMult_w(u, v) = u * v$ *mod*  $2^w$ 한 것을 signed 값으로 변환 
        ![image](https://user-images.githubusercontent.com/86834982/227835913-f7eea3ef-79a3-46fe-97dd-5bfd89041723.png){: width="470px"} 
        
        
    
    - **Multiplication (Power-of-2, Left Shift)**
        
        **unsigned**와 **signed** 모두 $u * 2^k$ ==  $u << k$ 같은 결과 !
        
        ![image](https://user-images.githubusercontent.com/86834982/227836034-ccb6db04-de3a-4cb6-9652-bca20cf79746.png){: width="470px"} 
        
        $2^k$가 아닌 경우 shift 연산 합성 : $(u << 5) - (u << 3) == u * 24$  
<br/>
- **Division (Power-of-2, Right Shift)**
    
    shift 연산을 하면 더 적은 cpu cycle을 가지기 때문에 더 효율적이다. 
    
    - **Unsigned Division** : $u / 2^k == u >> k$  (logical shift)
        
    - **Signed Division** : $u / 2^k == u >> k$  (arithmetic shift) 이 원래 성립해야 하지만,
        
        ![image](https://user-images.githubusercontent.com/86834982/227836375-026f39c7-a49d-4b8f-a767-25ac95f6b050.jpg){: width="470px"} 
        
        일부 케이스에서는 성립하지 않기 때문에, 컴퓨터 cpu는 **미리 $+1$**을 한 후, arithmetic shift를 수행해서 결과 값을 도출한다!
        
        ![image](https://user-images.githubusercontent.com/86834982/227836477-551ba711-d226-41a2-9953-4db32592ebb3.jpg){: width="470px"} 
        

<br/>  
<br/> <br/> 
@Computer Systems a programmer’s perspective third edition 내용을 참고함
  
  
    
<br/><br/>
[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}

