---
title:  "Machine Programming I : Basics" 

categories:
  - Computer Systems
tags:
  - [register, processor, assembly, compiler]
toc: true
toc_sticky: true
use_math: true
date: 2023-04-12
last_modified_at: 2022-04-12
---

<br/> 
<br/> 


## **History of Intel processors and architectures**

---

### **Intel x86 Processors**

이 강의는 Intel 프로세서를 중심으로 설명하지만, 최근에는 Arm 프로세서로 추세가 바뀌어 가고 있다. 

- **Intel** (**CISC** - Complex Instruction Set Computer)
    
    : 세계 최초의 마이크로프로세서 개발(1978), 오랜 기간 동안 독보적인 위치를 차지
    
    **8086** (16 bit) -> **IA32** (32bit) -> **x86-64** (64 bit)  -> (more cores!)
    
    -> RISC에게 밀린 이유 : 많은 instruction마다 다른 logic gate 형태로 저장하고 있기 때문에 많은 에너지가 소모됨. 하지만 요새 메인 이슈는 sustainable developement, 따라서 에너지를 절약하는 RISC에 밀리게 됨. 
    
- **Arm** (**RISC** - Reduced Instruction Set Computer)
    
    : 영국 기업 Arm Holdings가 디자인한 마이크로 프로세서 아키텍쳐 
    
    -> 이후 애플은 이 아키텍쳐를 구매해서 자체 칩(M1 chip)을 만들어 사용하고 있고, 최근 들어 삼성도 Arm 기술을 사용해서 자체 프로세서를 제작하고 적용 중에 있음.
    
<br/> 
<br/> 
## C, assembly, machine code

---

**Machine Code** : 프로세서가 실행하는 byte-level 기계어 코드

**Assembly  Code** : 0과 1로 이루어진 기계어를 텍스트로 보완한 코드

-> 어셈블리어는 하이레벨 코드와 기계어의 **중간 단계**라고 보면 된다!

<br/> 
<br/> 
### Turning C into Object Code

작성된 파일이 머신 레벨 코드로 컴파일되는 전체적인 과정 : 

**C 파일**(test.c) -> **어셈블리 파일**(test.s) -> **오브젝트 파일**(test.o) -> **실행 파일**(a.out)

![image](https://user-images.githubusercontent.com/86834982/231742739-548b3068-3dd0-4d4a-a786-d93a6fa96ce2.png){: width="480px"}

ex. 로컬 터미널에서 한 단계씩 컴파일한 결과 

test.c / test.s / test.o / a.out / objdump(-> disassembler)

![image](https://user-images.githubusercontent.com/86834982/231742765-8cf36e04-6772-44ed-8c3d-b68d35726610.png){: width="480px"}

<br/> 
<br/> 
### Compiling Into Assembly

`gcc -Og -S sum.c`  —> sum.s 파일 생성 

- `-S` : 어셈블하는 단계에서 컴파일 과정을 stop하고 반환하라는 의미.
- `-Og` : 코드를 최적화시켜주는 optimization flag. -> 없으면 코드가 매우 복잡해서 읽기 어려움.
    
    ![image](https://user-images.githubusercontent.com/86834982/231742899-b252192c-fcb9-4eb0-bbd6-a0bca2d13542.png){: width="480px"}
    
    확인할 수 있는 instruction set : *pushq*, *movq*, *call*, *popq*, *ret* 등 
    
    여기서 보면 명령어 뒤에 알파벳(q)가 붙어있는 것을 확인할 수 있는데, 현재 레지스터가 처리하는 데이터의 크기에 따라 정해진다. 
    
    - *b* = byte (8 bit)
    - *w* = word (16 bit)
    - *l* = long (32 bit integer, 64-bit floating point)
    - ***q* = quad (64 bit) - 주로 많이 사용!**
    
    -> 현재 많은 컴퓨터 시스템에서 64 비트 아키텍쳐(데이터 타입)를 사용하기 때문
    

터미널에서 C 코드를 어셈블리 파일로 변환한 내용은 다음과 같다. 

![image](https://user-images.githubusercontent.com/86834982/231742928-34269d5b-59d1-4292-923f-f569275c4ea4.png){: width="480px"}

-> 앞에 .로 시작하는 라인은 CPU가 실행하는 명령어에 속하지 않고, 어셈블러와 링커 그리고 디버거에게 부가 정보를 전달하는 역할. 나중에 실행파일 만들 때 없어진다.

<br/> 
<br/> 
### Disassembling Object Code

`objdump -d sumstore.o` -> disassemble 결과를 띄워준다

기존 assemble 단계에서는 어셈블리 파일을 오브젝트 파일로 변환해준다면, 여기서는 반대로 오브젝트 파일을 다시 어셈블리 파일로 변환하는 것.

![image](https://user-images.githubusercontent.com/86834982/231743127-7a3db07a-4a08-49d8-acef-bd7043ed8f51.png){: width="480px"}

터미널에서 objdump를 출력한 결과

![image](https://user-images.githubusercontent.com/86834982/231743063-64db2245-2161-422a-be73-e859a19e5608.jpg){: width="480px"}

<br/> 
**Disassembler를 사용하는 이유**

- 오브젝트와 어셈블리 코드를 같이 보여줘서 명령어별로 코드를 확인하기 쉬움
- 생성된 실행 파일을 디버깅하거나 분석하는데 용이

하지만 이렇게 실행 파일을 거꾸로 분석하는 reverse engineering은 위험하다! 

해당 파일에 license가 있는지 확인하고, 있을 경우 copyright 이슈를 조심해야 한다. 


<br/> 
<br/> 
## Assembly Basics: Registers, operands, move

---

### x86-64 Integer Registers

레지스터는 일반 목적(흰색)과 특수 목적(빨간색)으로 사용되는 레지스터로 나눌 수 있다. 

![image](https://user-images.githubusercontent.com/86834982/231743172-bcf58240-31e6-4a8b-9757-fc6284ab73d3.png){: width="480px"}

일반 목적(general-purpose)의 레지스터는 데이터를 저장하거나, 메모리 주소를 가지고 있거나, 산술 연산을 수행한다. 반면에 특수 목적(special purpose)의 레지스터는 `%rip`(instruction pointer)나 `%rsp`(stack pointer) 등 주로 다른 특정 기능을 수행할 때 사용된다. 

<br/> 
<br/> 
### Moving Data : movq

***movq***   $**Source, Dest**$

movq 명령어는 소스 데이터(source)를 특정 목적지(destination)로 이동(복사)하는 명령어

처리하는 데이터의 크기, 즉 레지스터의 크기에 따라서 move 명령어 뒤에 다른 접미사 수식이 붙는데, movq는 64bit operation을 사용하는 경우 해당된다. 

![image](https://user-images.githubusercontent.com/86834982/231743251-081843ae-d750-4f0f-ab55-10db678ee9be.png){: width="480px"}

6     *movabsq* : source는 항상 immediate, destination은 항상 register만 가능 

<br/>   
**Operand Combination**

- **Immediate** : 상수로 된 정수 데이터
    
    ex.  `$0x400`,  `$-533`  ($ : 일시적인 값이라는 의미)
    
- **Register** : 16가지의 정수 레지스터 중 하나
    
    ex.  `%rax`,  `%r13` 등
    
- **Memory** :  연속적인 8byte 메모리 주소
    
    ex.  `(%rax)`  (괄호 안은 메모리 주소를 저장하고 있다는 의미)
    

![image](https://user-images.githubusercontent.com/86834982/231743260-d09bafbf-0eae-41fa-bb0a-e02aaf1160a0.png){: width="480px"}

-> **memory-memory**로는 **이동할 수 없다!**

따라서 (메모리-레지스터) -> (레지스터-메모리)로 나누어 이동해야 한다. 

ex. *movb*, *movw*, *movl*, *movq*로 값이 바뀌는 과정 

먼저 moveabsq로 레지스터의 값을 초기화한다. 그리고 뒤에 movb, movw, movl, movq를 사용해서 레지스터의 값을 바꾸는 과정으로 나타낸다. 

![image](https://user-images.githubusercontent.com/86834982/231743367-ec816796-85a2-4c9a-bcd0-9e3c2fe0fa02.jpg){: width="480px"}

-> 여기서 -1(FF) 이렇게 각 byte수의 2배로 값이 들어가는 이유는 hexadecimal notation에 따라 4개의 bit씩 끊어서 변환되기 때문이다. 

기존 move 명령어는 source와 destination의 operand 사이즈를 맞추어 주어야 한다는 한계점이 있다. 그래서 이를 극복하기 위해서 남은 비트를 확장해서 채우는 매커니즘을 도입한 명령어가 다음과 같다. 

- *movz* (zero extension) : 남은 destination bit를 0으로 채운다
- *movs* (sign extension) : 남은 destination bit를 sign bit으로 채운다
    
    -> 단, destination operand의 크기가 더 커야한다는 전제가 필요하다!
    
<br/> 
<br/> 
### Simple Memory Addressing Modes

데이터가 저장되는 장소를 memory로 한정 짓지 않고, 레지스터까지 넓게 보아서 레지스터내에 있는 데이터를 가르키는 방법까지 addressing mode라고 표현하고 있다.

- **Normal** :  $**(R)$  $= Mem[Reg[R]]$**
    
    `movq (%rcx), %rax` : rcx에 저장된 (메모리 주소)에 있는 데이터를 rax로 옮김
    
- **Displacement** :  $**D(R)$ $= Mem[Reg[R]+D]$**
    
    `movq 8(%rbp), %rdx` : rbp에 저장된 (메모리 주소+8)에 있는 데이터를 rdx로 옮김
    
    -> displacement는 일종의 메모리 주소를 jump 시키는 방법으로 생각하면 됨!
    
<br/> 
<br/> 
### Complete Memory Addressing Modes

가장 일반적인 addressing 형식 : 

$D(Rb, Ri, S)$ $= Mem[Reg[Rb] + S * Reg[Ri] + D]$

- $D$ : displacement 이동시키는 상수
- $Rb$ : base 레지스터 (16개의 정수 레지스터)
- $Ri$ : index 레지스터 (%rsp를 제외한 레지스터)

ex 1. 메모리 address 계산 문제 

![image](https://user-images.githubusercontent.com/86834982/231743473-6ad21466-d33b-48f7-9c56-961d1e7b1376.png){: width="480px"}

ex 2.  다음 코드에서 아래와 같이 element를 메모리 주소에 저장

![image](https://user-images.githubusercontent.com/86834982/231743478-54f05f7b-1685-4236-9b6c-70e7c70c9e63.png){: width="480px"}


<br/> 
<br/> 
## Arithmetic & logical operations

---

### Address Computation : leaq

*leaq*  $Source, Dest$

*Source* : memory operand (address mode operation 계산)

*Dest* : destination register (항상 register에 저장)

lea 명령어는 load effective address의 줄임말로, 메모리 참조 없이 주소만을 가져와서 계산하고, 그 결과를 레지스터에 저장한다. 크게 두 가지를 수행하는데 :

1. **& operation** : `p = &x[i]`
    
    주소 값을 가져와서/계산해서 저장
    
2. **arithmetic operation** : `x*12` **(shift+add)** 
    
    곱하기 연산을 shift+add 연산으로 최적화 
    
    ex. 예시는 다음과 같다. 
    
    ![image](https://user-images.githubusercontent.com/86834982/231743744-044efab4-2965-4f57-801c-446d2bf5a340.png){: width="480px"}
    
    여기서 12 = 3 * 4 형태로 바꾸어서 계산하면 shift+add로 나타낼 수 있다. 
    
    ```perl
    leaq (%rdi, %rdi, 2), %rax  # x*3
    salq $2, %rax   # (x*3) * 4 = x*12
    ```
    
    일반적인 곱하기 연산은 많은 logical gate를 필요로 하기 때문에 이렇게 add와 shift연산으로 바꾸어서 수행할 경우, **훨씬 효과적**으로 계산할 수 있다. 
    
    위와 같은 과정은 최적화의 일부에 해당되고, 컴파일 과정에서 자동으로 처리된다. 
    
<br/> 
**movq vs. leaq**

다음과 같이 각각의 명령어에 같은 instruction을 준다고 할 때,

```perl
movq (%rdi, %rdi, 2), %rax   # 3*rdi 주소가 가르키는 **value 저장** 
leaq (%rdi, %rdi, 2), %rax   # 3*rdi **주소값 저장** 
```

즉, movq는 데이터를 메모리에서 레지스터, 혹은 레지스터에서 메모리로 옮길 때 사용하고 leaq는 메모리 주소를 계산해서 레지스터에 저장할 때 사용한다.

<br/> 
<br/> 
### Arithmetic Expression

- operand가 **2개**인 연산
    
    ![image](https://user-images.githubusercontent.com/86834982/231743669-97617bbe-bf15-4625-9c2f-4cd0c696bc5b.png){: width="480px"}
    
    -> unsigned와 signed의 구분이 없다. bit-level에서 arithmetic operation이 이루어지기 때문에 같은 방식으로 계산된다. 
    
- operand가 **1개**인 연산
    
    ![image](https://user-images.githubusercontent.com/86834982/231743919-8fd00204-ebb8-408b-bf11-f8924130495f.png){: width="480px"}
    
    더 자세한 부분들은 교과서를 참고하면 됨 ~
    

<br/>  

<br/> <br/> 
@Computer Systems a programmer’s perspective third edition 내용을 참고함
  
  
    
<br/><br/>
[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}
