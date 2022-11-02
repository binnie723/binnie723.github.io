---
title:  "[따로 정리하는] Memory Hierarchy" 

categories:
  - System Programming
tags:
  - [Linux, Unix, memory, hierarchy]

toc: true
toc_sticky: true

date: 2022-11-01
last_modified_at: 2022-11-01
---
<br/> 
컴퓨터 구조 안 듣고 시프 들으니까 너무 어려운게 많아서 잠깐 중간에 정리해두는 Memory Hierarchy..
<br/> 
<br/> 
## Program Execution Process
메모리 계층 구조를 이해하기 전에 보면 좋은 프로그래밍 실행 과정   

<br/> 
컴퓨터 구조를 단순화하면 : **Processor**(CPU), **Main Memory**(RAM), **Disk**(HDD)  

![image](https://user-images.githubusercontent.com/86834982/199549542-8a0edd5f-1f26-408e-9687-99401388029f.png){: width="80%" height="80%"}

  

프로그램을 실행하면 OS는 메모리(RAM)에 공간을 할당한다. HDD가 프로그램에 필요한 데이터를 메모리에 로드하면, 메모리는 CPU로 보내서 연산을 처리한다. 지금 자세히 보려고 하는 부분은 **RAM <-> CPU**가 교류하는 부분.    

- 메모리가 여러 종류로 계층화된 구조를 가지는 이유 : CPU가 메모리에 더 빨리 접근하게 하기 위함
- 메모리 계층 구조는 “어떻게 하면 최적의 효율을 가진 컴퓨터를 만들까”에서 시작된다.
    
    CPU는 작은 메모리에 더 빨리 접근할 수 있다. 따라서 프로그램에서 자주 쓰이는 데이터를 작은 메모리에, 그리고 나머지 덜 쓰이는 데이터를 큰 메모리에 저장하면 효율적으로 메모리를 작동시킬 수 있다.     
    
    
<br/> 
## Memory Hierarchy

메모리 계층 구조는 크게 **Register**, **Cache**(L1, L2, L3), **Main Memory**, **Hard Disk**로 나뉜다.   

![image](https://user-images.githubusercontent.com/86834982/199549605-6018d866-007c-4afe-a7e3-ac5b608cf986.png){: width="85%" height="85%"}


<br/> 
### Register

CPU 내부에 존재하며 **CPU가 현재 처리하는데 필요한 데이터**를 일시적으로 저장하는 공간. 주로 데이터 주소와 명령어들을 저장하고, 용량은 작지만 매우 빠른 속도로 접근할 수 있는 고속 메모리   

- CPU의 데이터 처리 속도 높이기 위해 도입 (= Memory Access time 감소)
- 1 bit의 정보를 저장할 수 있는 flip-flop의 집합 -> 32 bit/64 bit로 나뉜다.
    
    여기서 비트 수는 명령을 한 번에 처리할 수 있는 레지스터의 비트 수   
    
- CPU 자체적으로 데이터를 저장할 방법이 없어서, 메모리로 보내기 전에 임시로 저장

<br/> 
### Cache

CPU 내부에 존재하며 레지스터 다음으로 빠른 메모리. 한 번 사용한 데이터를 다시 사용할 가능성이 높다는 데이터의 지역성을 이용해서, **CPU에서 자주 쓰일 것 같은 데이터**를 메인 메모리로부터 복사해서 저장해둔다. 

- 캐시 메모리도 CPU와의 거리에 따라 **L1, L2, L3 캐시**로 구분
    
    -> L1 캐시가 가장 가깝고 빠르며, L3 캐시가 가장 멀리 있고 느리다. 
    
- CPU <-> 메인 메모리 간의 속도 차이를 극복하기 위해 사용
- 주로 SRAM으로 구성된다

<br/> 
### Main Memory

CPU 외부에 존재하며 **프로그램을 실행하는데 직접 사용되는 데이터**를 저장하는 주 기억장치. 일시적으로 저장하는 **휘발성 메모리**로 전원이 꺼지면 내용들은 사라진다. 모든 프로그램은 컴퓨터에 실행되기 위해 메모리의 일부를 차지한다. 

- 메인 메모리 = 주 기억장치 = **RAM (Random Access Memory)**
- CPU <-> SSD, HDD 간의 속도 차이를 극복하기 위해 사용
- 주로 DRAM으로 구성된다

<br/> 
### SSD, HDD

**프로그램이 실행될 때 직접적으로는 쓰이지 않는** 보조 기억장치. 영구적으로 저장되는 **비휘발성 메모리**로 전원 꺼져도 사라지지 않는다. 

- **Solid State Drive**(SSD) : 반도체 기기, 메모리를 읽고 쓰는 속도는 빠르지만 1GB당 가격이 비싸다
- **Hard Disk Drive**(HDD) : 물리적 장치, 데이터를 읽고 쓰는 속도는 느리지만 1GB당 가격이 싸다는 장점

대부분의 컴퓨터는 SSD, HDD 둘 다 사용하며, SSD가 HDD보다 상위에 위치해 있다.  용량이 작지만 자주 쓰이는 운영체제와 같은 프로그램은 SSD에, 용량이 크지만 작업이 자체는 단순해서 빠른 엑세스가 필요하지 않은 이미지, 영상 파일을 HDD에 저장되어 있다.


<br/><br/>

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}
