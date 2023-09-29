---
title:  "Deep Learning 개요" 

categories:
  - Deep Learning
tags:
  - [deeplearning]
toc: true
toc_sticky: true
use_math: true
date: 2023-03-14
last_modified_at: 2022-03-14
---


<br/>
### Deep Learning Overview

딥러닝을 한 마디로 설명하면 **인공지능을 위한 알고리즘 중 하나**라고 할 수 있다. 

인공지능, 머신러닝, 딥러닝의 개념이 자주 등장하는데

- 인공지능 (AI) : 인간의 지능을 모방한 것
- 머신러닝 (ML) : 인공지능의 일종으로, 인간이 아닌 머신이 학습하도록 하는 것
- 딥러닝 (DL) : 머신러닝의 일종으로, deep neural network를 기반으로 한 학습 방법

![image](https://user-images.githubusercontent.com/86834982/224998326-77dae60b-98d8-444e-b94e-b4e370294932.png){: width="540px"} 

그래서 이들의 관계를 다시 정리하면,

> **인공지능**  >  **머신러닝**  >  **신경망** (얇은 신경망, 심층 신경망 = 딥러닝)
> 

<br/>
### Neural Network

- Biological Neural Network
    
    우리의 뇌에 있는 신경 세포의 구조는 다음과 같다. 먼저 dendrite를 통해 전기 신호를 받으면, soma에서 전압 상승된다. 어느 이상 값이 되면 unstable한 상태로 바뀌면서 spike가 형성되고, axon으로 신호가 넘어가면서 다음 뉴런까지 전송된다. 
    
    ![image](https://user-images.githubusercontent.com/86834982/224998341-83074e74-ccce-47c3-87f4-35596c5c34d8.png){: width="540px"} 
    

그렇다면 이를 컴퓨터에 적용하면 사람과 비슷한 인공지능을 만들 수 있지 않을까? 에서 시작해서 만들어진 것이 인공 신경망 !  
<br/>
- Artificial Neural Network
    
    인공 신경망은 다음과 같이 크게 세 가지의 레이어로 구분된다. 
    
    > *input layer  ->  hidden layers  ->  output layer*
    > 
    
    여기서 중간에 있는 hidden layer의 수에 따라 신경망을 나눌 수 있다. 
    
    - Shallow neural network : 히든 레이어가 **3개보다 적은** 경우
    - Deep neural network : 히든 레이어가 **3개 이상**인 경우  
    
    이와 같은 레이어들을 한 층씩 쌓아나가면서 더 깊은 신경망을 구축할 수 있는데, 이는 더 복잡한 연산들을 가능하게 한다. 
    
    ![image](https://user-images.githubusercontent.com/86834982/224998357-956c0403-5242-43b8-ae2a-4c048fe4c83c.png){: width="540px"} 
    
    또한 synaptic efficacy (뉴런과 뉴런을 연결하는 부분의 효율성)를 모방하여 weight라는 개념을 도입했다. 딥러닝에서 학습시킬 때, 임의의 가중치 값을 넣어서 다음 노드로 넘어갈 때마다 다르게 계산하도록 구현되어 있다. 
    

<br/>    
### Brief History of Deep Learning

딥러닝의 아이디어는 1940-50s부터 등장했다. 1960-70s에 이르러서는 데이터를 밀어넣고 학습을 하면 neural network를 구축할 수 있을 것이라는 의견도 제기되었지만,  computing power가 부족해서 구현되지 못했다. 

그러다 기술이 계속 발전하면서 게이머들이 더 좋은 게임 퀄리티를 위해 GPU를 개발하였고, NVIDIA는 이 GPU가 딥러닝에 적합하다는 것을 발견하고 도입하면서 딥러닝이 활성화 되었다. 이로 인해 이미지나 스피치 인식, 자연어 처리 등 다방면으로 활용되며 최근 크게 주목 받게 되었다. 


<br/>    
### Deep Learning Application

Neural Network를 사용해서 어떤 것들을 할 수 있을까?

딥러닝을 문제에 적용할 때, 대부분 다음과 같은 매커니즘으로 적용된다. 

> *input -> (application model) -> output*
> 

모든 입력 데이터는 **숫자**로 표현해 주어야 한다. 여기서 숫자는 단순한 scalar 형태가 아닌 차원을 갖는 tensor 형태로 만들어주어야 한다.

딥러닝을 통해 문제에 접근할 때, divide and conquer 방식으로 문제를 나누어 해결하는 것이 좋다. 신경망을 학습시키기 위해서는 엄청난 양의 데이터가 필요한데, 이들을 한 번에 처리하는 and-to-and 방식보다 모델을 나누어서 해결하는 편이 효율적이기 때문이다.

<br/>  
**Deep Learning을 적용한 여러 가지 사례들**

- Computer vision : object detection (객체 탐지)
    
    입력 : 이미지  ->  출력 : bounding box (양 끝 대각선 좌표. 한 점과 width, height 값)
    
- Computer vision : segmentation (분할)
    
    입력 : 이미지  ->  출력 : 색칠된(마스크가 포함된) 이미지
    
- Computer vision : image caption generation
    
    입력 : 이미지  ->  출력 : one-hot vector들의 나열
    
- Self driving car (자율주행 자동차)
    
    입력 : 이미지  ->  출력 : 핸들 각도 및 엑셀 감도 등 
    
- AI Designer
    
    주로 생성(generative) 모델을 사용해서 디자인을 생성하다가 이거 괜찮은데 싶으면 고름
    
    입력 : 랜덤 숫자  ->  출력 : 해당 숫자에 해당하는 옷 이미지




<br/>   
<br/><br/>
[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}
