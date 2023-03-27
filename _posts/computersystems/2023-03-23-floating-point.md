---
title:  "Floating Point" 

categories:
  - Computer Systems
tags:
  - [IEEE, encoding]
toc: true
toc_sticky: true
use_math: true
date: 2023-03-26
last_modified_at: 2022-03-26
---

<br/>
저번 챕터에서 floating point는 표현하는 방식이 달라서 일반 정수와 다른 hexadecimal notation으로 표기되는 것을 확인할 수 있었는데, 이번 챕터에서는 이 실수가 표기되는 정확한 방식과 원리에 대해 다루고 있다. 


<br/> 
<br/>  
### Fractional Binary Numbers

실수를 일반적인 이진법으로 접근하면, binary point를 기준으로 두 영역으로 나눌 수 있다. 

- 정수(integer) 영역 : exponent power of  $2$ ($= 2^i$)
- 실수(precision) 영역 : fractional power of  $2$ ($= 2^{-j}$)

![image](https://user-images.githubusercontent.com/86834982/227237467-fe55d2c8-818a-4dc0-818a-f9e710b68c0a.png){: width="540px"} 

그리고 이를 수식화하면 : 
$$
\sum_{k = -j}^ib_k*2^k
$$

하지만 이와 같은 방식으로 실수를 표기하면 몇 가지 문제점이 발생한다. 

1. $x/2^k$  형태로 나타낼 수 있는 경우만 표현 가능 
    
    ex. $1/3 = 0.01010101..$ : 무한 소수가 되는 경우 표현할 수 없음 
    
2. 표현할 수 있는 실수의 범위가 매우 한정적 (8 bit fixed point)
    
    ![image](https://user-images.githubusercontent.com/86834982/227237575-6f89f862-8303-43bf-afd6-f9f6d39c5007.png){: width="540px"} 
    

따라서 이러한 수에 대한 문제점들을 보완하고자 도입된 실수 표현법이 IEEE 표준안이다.   



<br/> 
<br/>  
### IEEE Floating Point standard

IEEE 754 표준안은 현재 가장 널리 대표적으로 사용되는 소숫점 표기법으로, 실수 전체의 비트를 크게 3부분으로 나누어서 표기한다. 

![image](https://user-images.githubusercontent.com/86834982/227237792-39239506-6154-406f-bd7f-6e4e50d2de94.png){: width="540px"} 

- **Sign bit (S)** : negative(-)인지 positive(+)인지 판별
- **Exponent (E = Exp-Bias)**
    - Biased **Exp** : 2의 지수 부분에 해당
    - **Bias** : $2^{(k-1)} - 1$,  ( $k$ : exponent bit)
- **Mantissa (M)** : 숫자의 body 부분 담당, $[1.0, 2.0)$ 사이의 수를 가진다.
    
    그리고 위의 구조를 수식화하면 : $(-1)^SM2^{E}$

이러한 표현 방식의 장점은 exponent의 개념을 도입함으로써 

1. 자유자재로 decimal point를 이동하는 것이 가능해졌고
2. mantissa 값과 상관없이 매우 작은 수부터 큰 수까지 표현할 수 있게 되었다.

<br/>    
**Precision options**

표현하는 숫자의 정밀도에 따라 다음과 같이 구분할 수 있다. 

- Single precision : 32 bits 메모리를 사용하는 방식
    
    ![image](https://user-images.githubusercontent.com/86834982/227237847-34623a88-9847-42db-b879-2ae1d96e1788.png){: width="540px"} 
    
- Double precision : 64 bits 메모리를 사용하는 방식
    
    ![image](https://user-images.githubusercontent.com/86834982/227237853-2f8e571b-6db2-45be-9fd0-e502056e8e7c.png){: width="540px"} 
    


<br/>  
<br/>  
### Constructing Floating Point number

다음과 같이 주어졌을 때, IEEE 표준 방식을 사용해서 실수로 나타내본다고 하면, 

![image](https://user-images.githubusercontent.com/86834982/227237950-9f9a2769-3f05-4545-9283-d83fe8c8f122.png){: width="540px"} 

각각 sign은 $S$, exponent는 $exp$, Mantissa는 $M$에 해당하는 값을 의미한다. 

![image](https://user-images.githubusercontent.com/86834982/227238021-2f66ae08-adb1-44a0-8af5-f4959bcb850e.jpg){: width="540px"} 

수식을 적용해서 풀면 해당 실수는 ${137/8}_{(10)}$의 값을 가지게 된다.

반대로 $17.125_{(2)}$를 IEEE 형식을 사용해서 이진 실수로 나타내보면,

![image](https://user-images.githubusercontent.com/86834982/227238033-b9145513-c67a-4d7f-890a-e54ce22f088f.jpg){: width="540px"} 

다음과 같이 잘 변환되는 것을 확인할 수 있다. 




<br/>  
### Normalized and Denormalized Values

- **Normalized Values** : $exp ≠ 000..00$  and  $exp ≠ 111..11$ 가 아닌 모든 경우 
    
    -> 위에 설명한 수식 그대로  $E = Exp - Bias$를 적용해서 계산한다.   
<br/>   
    
- **Denormalized Values** : Normalized가 아닌 경우
    
    -> 예외 케이스로 간주해서 exponent를 다르게 계산한다. 

<br/>  
1. if  $exp = 000..00$
        
    $0$을 그대로 대입하지 않고, $E = 1- Bias$를 적용해서 계산한다. 
        
    exponent 부분이 $0$인 경우를 예를 들어 계산해보면, $0$에 가까운 매우 작은 숫자를 나타내고 있다는 것을 확인할 수 있다. 
        
    ![image](https://user-images.githubusercontent.com/86834982/227238182-4700fa99-300f-4351-8380-1977317904dc.jpg){: width="540px"} 
        
    -> 수가 너무 작은 경우에는 그냥 0으로 간주되기 때문에, $+0$과 $-0$은 numerical 상 같은 값을 의미하지만 내부적인 표현 방식은 다르기 때문에 주의해야 한다. 
<br/> <br/> 
2. if  $exp = 111..11$
        
    $frac = 000…0$이면  $infinity$ 
        
    $frac≠000..0$이면  $NaN$
        
    -> $NaN$의 경우, 매우 큰 수를 의미하기 때문에 comparision 대상이 될 수 없다
        

    <br/>  
    **Floating Point Encoding** 전체 분포를 확인해보면 다음과 같다. 
    
    ![image](https://user-images.githubusercontent.com/86834982/227238360-42a00901-eecb-46d2-a415-e790ee5e504c.png){: width="540px"} 
    
    0과 가까워질수록 촘촘하고, 0에서 멀어질수록 띄엄띄엄 배치된 것을 확인할 수 있다.
    
    ![image](https://user-images.githubusercontent.com/86834982/227238374-997b6b32-aa4e-405b-8cf0-0cbc5b53ab12.png){: width="540px"} 


<br/>  
<br/> <br/> 
@Computer Systems a programmer’s perspective third edition 내용을 참고함
  
  
    
<br/><br/>
[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}
