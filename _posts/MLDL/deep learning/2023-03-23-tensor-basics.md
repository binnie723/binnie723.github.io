---
title:  "Pytorch Basics : Tensor 알아보기" 

categories:
  - Deep Learning
tags:
  - [deeplearning, pytorch]
toc: true
toc_sticky: true
use_math: true
date: 2023-03-23
last_modified_at: 2022-03-23
---

<br/>
## Tensors

파이토치에서 텐서(tensor)는 numpy에 있는 ndarray와 매우 유사하며, 한 마디로 정리하면 **다차원 배열**을 통칭하는 말이다. 

두 가지의 차이점은 numpy는 CPU에서만 돌아가는 반면, tensor는 GPU와 같은 연산 가속을 위한 하드웨어에서 실행할 수 있다는 장점이 있다. 따라서, 많은 양의 연산이 반복되는 딥러닝에서 사용하기 효과적이다. 

| scalar | vector | matrix | matrix sequence |
| --- | --- | --- | --- |
| 13 | (13, 42, 2011) | ((8, 1, 6), (3, 5, 7), (4, 9, 2)) | … |

텐서는 **여러 차원을 가질 수 있는** 데이터를 의미하기 때문에 우리가 접한 위와 같은 형태의 데이터들을 모두 포괄하는 개념이라고 볼 수 있다. 


<br/><br/>
### **Tensor 배열 선언하기**

- `.empty(r, c)` : 쓰레기 값으로 초기화
- `.zeros(r, c)` : 모두 $0$으로 초기화
- `.rand(r, c)` : $0$ ~ $1$  사이의 무작위 실수 값으로 초기화
- `.randn(r, c)` : $mean : 0, var : 1$로 정규화된 실수 값으로 초기화
- `.randint(a, b, size=(r, c))` :  $a$부터 $b$  사이의 랜덤 정수 값으로 초기화

```python
import torch
x = torch.empty(1, 3)    # tensor([4.2039e-45, 0.0000e+00, 4.2039e-45])
x1 = torch.rand(1, 3)    # tensor([0.7720, 0.2139, 0.0912])
x2 = torch.randn(1 ,3)   # tensor([0.5197, -0.7889,  0.0773])
x3 = torch.randint(0, 10, size = (1, 3))  # tensor([9, 4, 9])
x4 = torch.zeros(1,3)    # tensor([0., 0., 0.])
```


<br/><br/>  
### **Tensor 데이터 타입 설정**

- `dtype = torch.half`: 16bit floating point으로 설정
- `dtype = torch.float`: 32bit floating point으로 설정
- `dtype = torch.double`: 64bit floating point로 초기화

```python
import torch
x = torch.zeros(1, 3).cuda()  
# tensor([0., 0., 0.]], device='cuda:0')

y = torch.zeros(1, 3, dtype=torch.half).cuda()    
# tensor([0., 0., 0.]], device='cuda:0', dtype=torch.float16)

z = torch.zeros(1, 3, dtype=torch.double).cuda()  
# tensor([0., 0., 0.]], device='cuda:0', dtype=torch.float64)
```
<br/>
아래는 Torch가 정의하고 있는 CPU와 GPU 텐서의 타입들을 나타낸다. 

![image](https://user-images.githubusercontent.com/86834982/227147090-980a4cf7-47ea-4960-a217-1ecbae5540b3.png){: width="540px"} 


<br/><br/> 
### **Tensor 배열의 크기 확인하기**

받아온 데이터나 결과 데이터의 출력 형태를 확인해볼 때 주로 사용한다. 

- `.size()` : 배열의 shape을 출력하는 함수
- `.shape` :  size와 동일한 기능을 하며,  `()` 없이 출력 가능

```python
x = torch.rand(5,3)
print(x.size()) # torch.Size([5, 3])
print(x.shape)  # torch.Size([5, 3])
```


<br/><br/> 
### **Tensor Addition 연산하기**

numpy와 유사한 연산 과정을 지원하고 있다. 

- `x+y` : 단순한 덧셈 기호를 통해서도 내부 원소를 더할 수 있다.
- `torch.add(x, y)` : 더하기 함수를 사용하는 방법

```python
x = torch.rand(5, 3)
y = torch.rand(5, 3)

# Addition
print(x + y)
print(torch.add(x, y))
print(y.add_(x))
```


<br/><br/> 
### **Tensor Resizing**

- `.view()` : 전체 원소의 갯수는 유지하면서 배열의 모양을 바꿔주는 함수

```python
x = torch.randn(4, 4)
y = x.view(-1)     # tensor를 (?) 사이즈로 변경
z = x.view(-1, 8)  # tensor를 (?, 8) 사이즈로 변경 -> (2, 8)
```
<br/>
여기서 -1은 사용자가 잘 모르겠으니 파이토치에게 알아서 맡기겠다는 의미를 가지고 있다. 따라서 랜덤으로 조정되고, 사용자가 직접 지정한 부분만 고정해서 바꾸어준다. 



<br/><br/> 
### **Tensor 배열의 값 꺼내기**

- `.item()` : tensor 배열 인덱스에 있는 값을 추출

```python
x = torch.tensor([1,2,3,4])
print(x[0].item())  # 1
print(x[0])         # tensor(1)
```
<br/>
기존처럼 배열의 인덱싱을 사용해서 값을 가져오는 경우, tensor에 감싸진 결과가 출력된다. 따라서 안에 있는 배열의 값을 추출하기 위해서는 item 함수를 사용해서 불러와야 한다. 


<br/><br/> 
### **Tensor <-> Numpy 변환**

numpy는 CPU, torch는 GPU를 사용해서 연산을 한다는 점을 제외하고는 매우 비슷한 원리를 가지고 있어서 상호 변환이 가능하다. 

- `.numpy()` : tensor -> numpy 배열로 변환
- `torch.from_numpy()` : numpy 배열 -> tensor로 변환

```python
# tensor -> numpy
a = torch.ones(5)  # tensor([1., 1., 1., 1., 1.])
b = a.numpy()      # [1. 1. 1. 1. 1.]

# numpy -> tensor
a2 = np.ones(5)    # [1. 1. 1. 1. 1.]
b2 = torch.from_numpy(a)  # tensor([1., 1., 1., 1., 1.], dtype=torch.float64)
```
<br/>
여기서 tensor와 numpy를 연결하는 경우, 둘 사이의 어떤 bridge가 형성되기 때문에 하나의 값을 바꾸면 다른 하나의 값도 자동적으로 바뀌게 된다. 

```python
a.add_(1)
# a : tensor([2., 2., 2., 2., 2.])
# b : [2. 2. 2. 2. 2.]
```


<br/><br/>  
### CUDA랑 연결하기

- `torch.cuda.is_available()` : 사용 가능 하면 True, 불가하면 False 출력
- `device = torch.device(”cuda:0”)` : 사용할 디바이스 종류 지정
- `.to(’cuda:0’)` : 0번째 cuda기기에 지정된 GPU에 데이터를 할당하겠다는 의미

```python
x = torch.rand(1)

if torch.cuda.is_available():
    device = torch.device("cuda")   
    y = torch.ones_like(x, device=device)  # directly create a tensor on GPU
    x = x.to(device)                       # or just use strings ``.to("cuda")``
    z = x + y
```
<br/> 
이를 통해 파이토치에서는 특정 GPU를 사용하도록 설정할 수 있고, 모델이나 데이터에 원하는 GPU를 직접 할당할 수 있다는 장점이 있다. 



<br/><br/>  
### **Tensor 차원 추가/축소하기**

데이터를 학습시키기 전에 전처리하는 과정에서 주로 사용된다. 

- `.unsqueeze(n)` : 기존의 배열에서 $n$번째에 차원을 추가
- `.squeeze(n)` : 기존의 배열에서 $n$번째 차원을 삭제/축소

```python
d = torch.rand(4,4)
print(d.shape)  # torch.Size([4, 4])

print(d.unsqueeze(0).shape)  # torch.Size([1, 4, 4])
print(d.unsqueeze(1).shape)  # torch.Size([4, 1, 4])
print(d.unsqueeze(2).shape)  # torch.Size([4, 4, 1])

print(e.squeeze(0).shape)    # torch.Size([4])
```

<br/><br/>   
### Tensor Pooling

간단하게 pooling은 텐서 내부의 데이터의 차원을 줄이거나 축소시킬 때 사용한다. 

- `MaxPool1d(n, stride = 1)` : n개씩 데이터를 묶어서 가장 큰 값 하나를 반환
- `AvgPool1d(n, stride = 1)` : n개씩 데이터를 묶어서 계산한 평균값을 반환
    
    -> stride는 몇 칸씩 건너뛰면서 pooling을 진행할 것인지를 의미
    

```python
a = torch.rand(1,4)    # tensor([0.7646, 0.3953, 0.5796, 0.8444])
max_p = torch.nn.MaxPool1d(3, stride = 1)
avg_p = torch.nn.AvgPool1d(2, stride = 1)

c = max_p(a)  # tensor([0.7646, 0.8444])
d = avg_p(a)  # tensor([0.5799, 0.4875, 0.7120])
```



<br/>   
<br/><br/>
[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}
