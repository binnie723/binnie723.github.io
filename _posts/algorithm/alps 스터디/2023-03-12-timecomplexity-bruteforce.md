---
title: 시간 복잡도와 완전 탐색
subtitle: 
categories: Algorithm
tags:
  - [algorithm, bruteforce]
toc: true
toc_sticky: true
use_math: true
date: 2023-03-11 23:51:28 +0000
last_modified_at: 2022-03-11 23:51:28 +0000
---

<br/>
![image](https://user-images.githubusercontent.com/86834982/224554786-02b5dd54-3650-49ca-91bd-6cf2dddd94cb.png){: width="800px"} 

이번에  알프스라는 백준 알고리즘 클럽에 새로 들어가게 됐다. 이 카테고리에서는 여기서 공부하는 내용을 정리해볼 예정이다. 알고리즘 공부하는 습관을 길러봐야지. 화이땅 


<br/>  
### 알고리즘

 : 문제를 해결하기 위해 정해진 일련의 절차

ex. 그래프에서 최단거리 구하기, 15와 24의 최대공약수 구하기 등 

문제 풀이에 있어서 중요한 것 

- **풀이 구상(매우 중요)** + 코딩 구현력
- 문제를 많이 풀기 (중요), 기본기 숙달하기


<br/>  
### 시간 복잡도

프로그램 수행 시간과 입력 크기 사이의 관계를 나타낸 것 

```python
count = 0
n = int(input())

for i in range(n):
	for j in range(n):
		for k in range(n):
			count += 1
```
<br/>
이를 big-O notation으로 표기하면 “알고리즘의 시간 복잡도가 $O(n^3)$이다.”

![image](https://user-images.githubusercontent.com/86834982/224554807-d1bdf63a-a428-4176-9f10-4c979bb74dd0.png){: width="580px"} 

여기서 가장 큰 항만 표기하고 그 외의 것들은 무시된다. 상수도 무시된다. 

컴퓨터의 정확한 실행 속도를 측정할 수 없다. 따라서 대략적인 계산 정도로 생각하면 됨.

$O(logn) < O(n) < O(nlogn) < O(n^2) < O(2^n) < O(n!)$

시간 복잡도는 다음 정도의 경우들만 고려해서 생각해보면 된다. 

<br/>
보통 코딩 테스트 문제의 시간 복잡도는 **1초 안(= 10억번)**으로 해결해야 한다. 

- N의 범위가 500인 경우 : $O(n^3)$ 안에서 해결 가능
- N의 범위가 2000인 경우 : $O(n^2)$ 안에서 해결 가능
- N의 범위가 10만인 경우 : $O(nlogn)$ 안에서 해결 가능
- N의 범위가 1000만인 경우 : $O(n)$ 안에서 해결 가능


<br/>  
### 완전 탐색

: 문제에서 가능한 모든 경우의 수를 탐색하는 것 (= Bruteforce 알고리즘?)

구현 방법에는 BFS, 재귀, 반복문 등 여러 가지가 있다. 

문제를 보면 먼저 “완전 탐색으로 풀 수 있는가”를 생각해보고, 시간복잡도를 고려하기


<br/>
### 백준 연습문제 (1) - 단순한 문제

[25494번: 단순한 문제 (Small)](https://www.acmicpc.net/problem/25494)

```python
t = int(input())
for _ in range(t):
  a, b, c = map(int, input().split())
  print(min(a, b, c))
```

<br/>
### 백준 연습문제 (2) - 수학은 비대면 강의입니다

[19532번: 수학은 비대면강의입니다](https://www.acmicpc.net/problem/19532)

연립 일차 방정식의 경우, 두 직선의 교차점은 하나이기 때문에 x,y도 모두 하나씩 존재

```python
# 시행 착오 1
a, b, c, d, e, f = map(int, input().split())

a1 = a * d
b1 = b * d
c1 = c * d
d1 = d * a
e1 = e * a
f1 = f * a

y = (c1 - f1) / (b1 - e1)
x = (c1 - b1 * y) / (a1)
print(int(x), int(y))  # x,y는 모두 정수임 
```
<br/>
근데 위의 방법으로 풀었더니 -> 런타임 에러 발생(zero-division error)

(b1 - e1)이랑 a1이 0인 경우를 고려해주지 않아서 발생한 것 같다. 이 말은 a 혹은 d가 0일 경우가 존재한다는 뜻인데, 이때 나머지 값들도 모두 0을 곱해주게 되는 문제도 발생한다. 예외 처리를 시도해보려고 했지만,애초에 초반에 a와 d를 곱할 때부터 경우를 나눠서 해줘야해서 매우 복잡했다.  
 
결국 더 간단하게 해결할 방법을 찾아봤는데,  
<br/>
**행렬의 곱셈**으로 해결할 수 있었다..!

![image](https://user-images.githubusercontent.com/86834982/224554836-bf869617-8b4d-4f45-8b65-24d6178a22bb.png){: width="580px"} 

선대 공부를 좀 곁들여보면,

행렬은 연립 일차방정식의 해를 구하기 위해서 연구되었다. 

행렬을 이용한 연립 일차방정식 풀이법 2가지는 : 

- **가우스-조던-소거법** : 행렬을 간소화해서 연립방정식 재구성
- **역행렬**을 이용하는 방식 : $Ax = B$에서 $A$의 역행렬이 존재하는 경우, 양변에 $A$의 역행렬을 곱해서 $x$의 해를 구할 수 있다. (존재하지 않는 경우: 해 존재 X)
    
    ![image](https://user-images.githubusercontent.com/86834982/224554858-cb8ca974-d0d1-48af-bb64-32042dbf58ab.jpg){: width="580px"} 
 
<br/> 
위의 방법을 적용해서 풀어봤지만 소숫점에서 미세한 오차가 발생 

```python
# 시행 착오 2 - 행렬의 곱셈
a, b, c, d, e, f = map(int, input().split())

mat = [a, b, d, e]
det = a * e - b * d
mat_rev = [e / det, (-b) / det, (-d) / det, a / det]

x = (mat_rev[0] * c + mat_rev[1] * f)
y = (mat_rev[2] * c + mat_rev[3] * f)

print(x, y)  # 1.9999999999999998  -1.0
```

<br/>
이유를 찾아본 결과, 우리가 사용하는 십진법에서의 소수는 컴퓨터가 이해하는 이진 소수로 정확하게 변환될 수 없다고 한다.   
결국 값을 근사하게 되면서 문제가 발생하는 것. 

[부동 소수점 산술: 문제점 및 한계](https://docs.python.org/ko/3/tutorial/floatingpoint.html)


<br/>
**맞았습니다 코드 !!**

나눗셈을 바로 하지 않고 계산 후 마지막에 하는 방식으로 수정했다. 

```python
# 맞은 풀이 - 행렬의 곱셈
a, b, c, d, e, f = map(int, input().split())

mat = [a, b, d, e]
mat_rev = [e, -b, -d, a]
det = a * e - b * d

x = (mat_rev[0] * c + mat_rev[1] * f) / det
y = (mat_rev[2] * c + mat_rev[3] * f) / det

print(int(x), int(y)) # 2  -1
```

<br/><br/>
오늘은 집중력 딸려서 여기까지 ..

<br/>   
<br/><br/>
[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}
