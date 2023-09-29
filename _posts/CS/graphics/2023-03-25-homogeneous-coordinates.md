---
title:  "Transformation, Homogeneous Coordinates" 

categories:
  - Computer Graphics
tags:
  - [rendering, transformation]
toc: true
toc_sticky: true
use_math: true
date: 2023-03-25
last_modified_at: 2022-03-25
---

<br/> 
<br/> 
### Transformation

렌더링에서는 크게 Affine과 Viewing transformation으로 나눌 수 있는데, 이 중에서 Affine transformation이 일반적으로 알려져 있는 오브젝트의 위치와 모양 등을 변형시키는 것을 말한다. 

![image](https://user-images.githubusercontent.com/86834982/227723811-8682f74f-ffee-49c1-8508-45fe030eb361.png){: width="510px"} 

<br/>  
Transformation의 종류들은 다음과 같다.   
- **Translation** : 특정 위치로 좌표를 이동시키는 것
- **Rotation** : 특정 각도로 회전시키는 것
    
    ![image](https://user-images.githubusercontent.com/86834982/227723844-3e0f4ab5-cd88-44f4-8787-4e0459d82c1c.png){: width="520px"} 
    
    -> 회전 행렬이 저러한 형태로 나오는 이유를 보이면 :
    
    ![image](https://user-images.githubusercontent.com/86834982/227723846-10557ef0-1609-4dbf-88ca-da936c50303a.jpg){: width="500px"} 
    

- **Scaling** : 오브젝트의 길이를 키우거나 줄이는 것
    
    ![image](https://user-images.githubusercontent.com/86834982/227723864-a8c0a3ca-3cbc-47af-a05e-23416ef86d53.png){: width="520px"} 
    
- **Shearing** : 수평 혹은 수직 방향으로 오브젝트를 미는 것
    
    ![image](https://user-images.githubusercontent.com/86834982/227723869-50533f9c-9423-445c-bd5c-91d197db635e.png){: width="520px"} 
    
- **Reflection** : 좌표의 축을 기준으로 오브젝트를 뒤집는 것
    
    ![image](https://user-images.githubusercontent.com/86834982/227723900-0ff968a2-812f-4101-b731-6e732b73b25b.png){: width="520px"} 
    
- **Composition** : 두 개 이상의 변형을 오브젝트에 적용시키는 것
    
    ![image](https://user-images.githubusercontent.com/86834982/227723908-cd4d4211-8c00-424f-b832-8498e2e1efc2.png){: width="520px"} 
    

<br/> 
<br/> 
### Homogeneous Coordinates

> ***" n차원 사영 공간을 n+1개의 좌표로 나타내는 좌표계 "***
> 

동차 좌표계라고 부르며, 차원을 추가해서 operation을 진행하는 것을 말한다. 

<br/>   
**2D Translation**

좌표의 미세한 차이만 발생하기 때문에 $2*2$ 크기의 행렬의 곱셈 형태로 나타낼 수 없다는 문제점이 있다. 

$x^{'}=x + x_{\delta}$

$y^{'}=y + y_{\delta}$

따라서 이러한 상황을 해결하기 위해서 도입한 개념이 Homogeneous coordinates!

![image](https://user-images.githubusercontent.com/86834982/227723952-df0576f1-283b-4fe8-ae89-513c00e6546b.png){: width="520px"} 

하나의 차원을 추가하면 다음과 같이 훨씬 수월한 방식으로 translation 좌표를 계산할 수 있기 때문에 동차 좌표계를 도입해서 사용해야 한다. 

![image](https://user-images.githubusercontent.com/86834982/227723964-97485ae4-a69b-47ad-a6e8-e53a513cd5c0.jpg){: width="520px"} 


<br/> <br/> 
**Translation과 Homogeneous coordinate 적용 사례** 

특정 위치의 직사각형 물체의 크기를 키우고자 한다고 할 때,

![image](https://user-images.githubusercontent.com/86834982/227723982-0eeafaea-1c82-4e82-92a1-61cadf50f509.png){: width="520px"} 

1. 먼저 원점으로 직사각형 물체를 translation
2. scaling으로 물체의 크기를 크게 조정 
3. 다시 적절한 원래 위치로 물체를 translation




<br/><br/>  
<br/><br/>
[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}
