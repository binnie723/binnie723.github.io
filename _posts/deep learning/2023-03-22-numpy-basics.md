---
title:  "Python Basics : Numpy 알아보기" 

categories:
  - Deep Learning
tags:
  - [deeplearning, numpy]
toc: true
toc_sticky: true
use_math: true
date: 2023-03-22
last_modified_at: 2022-03-22
---


<br/>
## Numpy

Numerical Python의 약자로, 파이썬에서 다차원 배열을 쉽게 처리하고 효율적으로 사용할 수 있도록 제공하는 패키지이다. 주로 pandas와 같이 데이터 분석하는 도구로써 알려져 있다. 

파이썬 공식 사이트에서 나와있는 특징 :

- 일반 파이썬 리스트에 비해 빠르고, 메모리를 효율적으로 사용한다.
- 반복문 없이 데이터 배열에 대한 처리를 지원하여 빠르고 편리하다.
- 선형대수와 관련된 다양한 기능을 제공한다.
- C, C++, 포트란 등의 언어와 통합이 가능하다.


<br/><br/>   
### N**umpy 패키지 설치하기**

터미널에 해당 명령어를 입력해면 쉽게 설치할 수 있다. 

```bash
pip install numpy
```


<br/><br/>   
### N**umpy 모듈 불러오기**

코드의 가장 상단부에 다음과 같은 형식으로 import하는 것이 일반적이다. 

```python
import numpy as np
A = np.array()  # default 배열 선언하기 
```


<br/><br/>   
### N**umpy 배열 초기화하기**

**수동으로 초기화한  numpy 배열**

```python
int_np = np.array([0, 1, 2, 3], dtype=np.int64)
float_np = np.array([0, 1, 2, 3.4], dtype='float64')
```
<br/>
float 타입으로 처리하는 경우, 데이터가 더 커지기 때문에 float로 표현할 필요가 없을 때(예를 들면 이미지 처리)는 int로 선언해주는 것이 좋다. 
<br/>
**자동으로 초기화한 numpy 배열**

```python
np.ones(10)       # array([1., 1., 1., 1., 1., 1., 1., 1., 1., 1.])
np.zeros((2,3))   # array([[0., 0., 0.], [0., 0., 0.]])
np.arange(3, 10)  # array([3, 4, 5, 6, 7, 8, 9])
np.eye(6)         # identity matrix (왼쪽 대각선만 1)
```


<br/><br/> 
### **Numpy 배열 연산**

numpy는 행렬의 연산이나 처리를 간편하게 하는데 초점을 맞춰 구현되었다.  

```python
scores = [100, 10, 80]
first_arr =np.array(scores)

print(first_arr * first_arr) # [10000   100  6400]
print(first_arr - first_arr) # [0 0 0]
print(1/(first_arr))     # [0.01   0.1    0.0125]
print(first_arr ** 0.5)  # [10.          3.16227766  8.94427191]
```
<br/>  
**<-> Python List 연산과의 비교** 

파이썬에서 제공하는 리스트는 연산보다는 시각적인 분류에 초점이 맞춰져 있다. 

```python
scores = [100, 10, 80]

print(scores + scores)  # [100, 10, 80, 100, 10, 80]
print(scores * 3)      # [100, 10, 80, 100, 10, 80, 100, 10, 80]
```





<br/>   
<br/><br/>
[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}
