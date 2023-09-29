---
title:  "Introduction to Computer Systems" 

categories:
  - Computer Systems
tags:
  - [cpu, memory]
toc: true
toc_sticky: true
use_math: true
date: 2023-03-18
last_modified_at: 2022-03-18
---

<br/><br/>
### Central Processing Unit (CPU)

컴퓨터의 중앙 처리 장치로, 컴퓨터의 두뇌 역할을 담당한다.

- **Registers**
    
    가장 작은 메모리 단위로, CPU 내부에 있으며 RAM에서 로드한 데이터를 저장한다. 컴퓨터의 기본 처리 단위로 알려진 32bit, 64bit은 이 레지스터의 크기를 말한다. 
    
- **Transistors**
    
    전자 신호나 전력을 증폭(amplify)시키는 장치이다. 실리콘으로 만들어졌고, on/off 스위칭 매커니즘을 사용된다.  
    
- **Control Unit**
    
    RAM으로부터 instruction을 받아오고 작은 명령어 단위로 분해해서 전달하는 전체 실행 과정을 통제한다. 주로 ALU와 memeory unit과 협력해서 명령을 수행한다. 
    
- **Arithmetic Logic Unit(ALU)**
    
    2개 이하의 입력을 받아서 이에 대한 산술 연산과 논리 연산을 하는 장치
       
    -> addition, subtraction(산술 연산), comparison(논리 연산)
<br/>   
    ![image](https://user-images.githubusercontent.com/86834982/227555569-d9f5c61c-fa7d-4603-aa41-0b16a2a2b885.png){: width="500px"} 
<br/> 
- **buses** (= wire들의 묶음을 말한다.)
    - **Data bus** : 데이터를 운반하는 전선
    - **Address bus** : 메모리 주소를 운반하는 전선
        
        -> data와 address bus를 합쳐서 **system bus**라고도 함
        
    - **Control bus** : enable(활성화)과 set(초기화)를 담당하는 전선
    - **I/O bus** : 입출력 장치와 연결된 전선으로, word size와 관련이 없다.  




<br/><br/> 
**CPU가 instruction을 처리하는 과정** 

먼저 control unit이 레지스터에 데이터를 설정(set)하고 활성화(enable)하면, ALU에 입력 값 A와 B가 전달된다. ALU는 RAM에서 전달받은 종류의 연산을 수행하고 결과를 출력해서 다른 레지스터에 저장한다. 전달 받은 연산이 조건문의 경우, flag 레지스터를 추가적으로 사용해서 여기에 결과값을 저장한다. 

![image](https://user-images.githubusercontent.com/86834982/227555592-5a2a572f-6681-45ca-b70e-439c41bdcd4f.png){: width="500px"} 

하나의 연산이 끝나면 control unit은 instruction address 레지스터에서 다음에 실행할 명령어의 주소를 확인한다. 그러면 주소 비트는 RAM으로 가서 다음 명령을 불러와 instruction 레지스터에 로드하게 되고, control unit은 다음 명령어를 실행한다. 


<br/><br/>  
### **Task manager** Identification

- **Cores** : 한 타임에 작동할 수 있는 물리적인 CPU의 개수를 의미
- **Logical Processors** : 논리상 작동할 수 있는 모든 CPU의 개수로, 가상 코어도 포함
- **Clock speed** (2.63GHz) : CPU가 1초에 2.63 billion 만큼의 정보를 처리
- **Processes** : 현재 실행되고 있는 프로그램의 인스턴스
    
    -> 독립적인 메모리 공간과 주소를 갖는다.
    
- **Threads** : 프로세스 안에서 실행되는 작은 단위
    
    -> 부모 프로세스의 메모리 공간과 자원을 공유해서 사용한다. 
    
    
    ![image](https://user-images.githubusercontent.com/86834982/227555649-48ce9de5-69fa-4afe-997b-7819bbdf6cce.png){: width="500px"} 



<br/><br/>   
### Process of executing

먼저 컴파일러는 사용자가 구현한 하이 레벨 코드를 로우 레벨 코드(= 어셈블러어)로 변환해준다. 어셈블리어는 다음과 같은 instruction set의 나열로 표현되는데, 최종적으로 0과 1로 이루어진 기계어 코드와 일대일 매핑 관계를 가진다.  

![image](https://user-images.githubusercontent.com/86834982/227555749-9a5941e3-3762-4cb7-b0ef-749aa59b1c6f.png){: width="540px"} 

컴파일된 프로그램을 실행하는 순간, RAM에는 프로그램에서 필요한 데이터와 코드를 로드한다. instruction, number, address, letter 등 다양한 형태의 데이터를 저장할 수 있다. 


<br/><br/>  
### CPU Cores

하나의 CPU 코어 당 하나의 프로그램만 실행시킬 수 있기 때문에, 코어의 갯수가 늘어날 수록 더 많은 연산을 동시에 수행할 수 있게 된다. 

- single-core processor : 독립적으로 L1, L2, L3를 하나씩 사용한다.
- multi-core processor : 독립적인 L1, L2 캐시를 가지고, L3 캐시를 공유한다.
    
    ![image](https://user-images.githubusercontent.com/86834982/227555860-eee96a5a-46dd-4f6e-bcca-8836b794178c.png){: width="500px"} 
    


<br/><br/>  
### Hyperthreading

- **Threading** : 프로세스를 실행시키는 단위로, 한 프로세스 내에서 쓰레드는 모두 같은 메모리 공간과 리소스를 공유하고 사용한다.
- **Hyper-threading** : 하나의 코어에서 두 개의 가상화된 코어를 실행시킬 수 있도록 하는 기술로, 마치 CPU가 여러 대인 것처럼 느껴지게 하는 것
    
    -> 하이퍼 쓰레딩은 하드웨어 기술의 한 종류라면, 쓰레드는 소프트웨어적인 개념!
    
    ![image](https://user-images.githubusercontent.com/86834982/227555885-2cc01b90-c911-4543-ac69-7e52c2d5eace.png){: width="500px"} 
    
<br/><br/> 
### Memory Hierarchy

> *Register*  <  *L1, L2, L3 cache (SRAM)*  <  *Main memory (DRAM)*  <  *Local Disks*
> 

컴퓨터 내부의 메모리 구조는 다음과 같은 계층 구조를 가진다. 위로 올라갈 수록 저장할 수 있는 메모리 단위가 작고 실행 속도도 빠르며, CPU와 가까이 위치한다. 

메인 메모리만으로는 CPU의 빠른 처리 속도를 따라갈 수 없었기 때문에, 이러한 속도 차이를 보정하고자 캐시의 개념이 도입되어 사용되었다. 

![image](https://user-images.githubusercontent.com/86834982/227556029-58946724-0781-4cae-8ff3-9c31003d8e1c.png){: width="540px"} 


<br/><br/>   
### Network

네트워크는 I/O 디바이스의 일종이라고 볼 수 있다. CPU와 메모리에서의 데이터는 I/O bus를 통해 네트워크 어댑터로 전달된다. 그리고 컴퓨터는 IP 주소를 통해 외부의 다른 기기와 연결할 수 있게 되고, 연결에 성공하면 기기의 MAC 주소를 얻게 된다. 

네트워크 어플리케이션은 MAC 주소와 port number를 사용해서 socket을 열어준다. 이를 통해 두 컴퓨터는 소켓을 사용해서 서로의 데이터를 주고 받을 수 있게 된다. 

![image](https://user-images.githubusercontent.com/86834982/227556051-b972d7f9-e09e-499f-9247-e8d77aeb08e0.png){: width="500px"} 

- IP 주소 (IP 프로토콜) : 컴퓨터의 네트워크 주소
- MAC 주소 (ARP 프로토콜) : 컴퓨터의 물리적인 주소
    
    IP는 임시적으로 할당된 변동성을 가진 주소이지만, MAC 주소는 통신 기기의 하드웨어 자체에 부여된 고유한 식별 번호를 의미한다. 
    
    
<br/>  
<br/> <br/> 
@Computer Systems a programmer’s perspective third edition 내용을 참고함
  
  
    
<br/><br/>
[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}

