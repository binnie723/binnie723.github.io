---
title:  "Graphics Overview Pipeline" 

categories:
  - Computer Graphics
tags:
  - [rendering, rasterization]
toc: true
toc_sticky: true
use_math: true
date: 2023-03-24
last_modified_at: 2022-03-24
---

<br/> 
<br/> 
### Graphics Area

컴퓨터 그래픽스의 분야는 크게 3가지로 나눌 수 있다. 

- **Modeling** 모델링
    
    3D 형태로 모델을 디자인하고 구축하는 것 (블렌더 등의 툴을 사용)
    
- **Rendering** 렌더링
    
    입력으로 받은 3D 모델을 photorealistic 실사 이미지로 변환하는 것
    
- **Animation** 애니메이션
    
    가상 모델로부터 motion, 캐릭터의 움직임을 구현하는 것 
    

그래픽스의 응용 분야에서는 3D 애니메이션에서 캐릭터를 배경에 맞게 렌더링하는 일을 하거나, 도비 같은 가상 캐릭터를 생성하기도 하고 AR과 VR을 구축하기도 한다. 

본 그래픽스 강의는 rendering에 초점을 두고 진행할 예정 !


<br/> 
<br/> 
### Graphics Pipeline

그래픽스, 정확히는 렌더링 파이프 라인은 다음과 같다. 

![image](https://user-images.githubusercontent.com/86834982/227723383-d4468c13-b83b-4f65-89e2-a5a02df80423.png){: width="550px"} 

- **Modeling Transformation**
    
    object space에 구현되어 있는 3d 모델의 배치를 구성에 맞게 바꾸는 것.
    
    이 단계에서는 rotation, translation, scaling 등을 수행하게 되는데, 예를 들면 주전자의 크기를 축소하고 몬스터 손이 있는 쪽으로 이동시키는 작업을 말한다. 
    
    ![image](https://user-images.githubusercontent.com/86834982/227723391-6b9fe9b5-7ca2-4bae-a837-919a5a81d5e8.png){: width="500px"} 
    
- **Illumination**
    
    광원의 위치를 지정한 후, 그 지점으로부터 3D 오브젝트에 빛을 쏘는 것. 
    
    -> 광원은 여러 개가 될 수 있고, 주위 환경에서 빛이 발생할 수도 있다.   
  <br/> 
    
- **Viewing Transformation**
    
    모든 메쉬 포인트를 world space 관점에서 eye space 관점으로 바꾸는 것. 
    
    즉, 모두가 볼 수 있는 관점에서 특정 관찰자 시점으로 바꾸어 표현하는 것을 말한다. 
    
    ![image](https://user-images.githubusercontent.com/86834982/227723436-fd34da54-5c9d-45ce-ae2d-7f75d63f207e.png){: width="500px"} 
    

- **Clipping, Projection**
    
    클리핑을 통해 시야에서 보이지 않는 물체들을 모두 제거하고, projection을 통해 3D 공간에 있는 물체를 2D 공간으로 옮기는 과정을 말한다. 
    
    ![image](https://user-images.githubusercontent.com/86834982/227723412-a01a700e-8208-4252-83f5-2f34cb2551ec.png){: width="480px"} 
    
- **Rasterization, Display**
    
    픽셀 값을 지정해서 물체에 색을 입히고 직접 볼 수 있는 물체로 시각화한다. 
    
<br/>  
대략 이런 과정으로 렌더링이 진행될 예정 !


<br/><br/>  
<br/><br/>
[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}
