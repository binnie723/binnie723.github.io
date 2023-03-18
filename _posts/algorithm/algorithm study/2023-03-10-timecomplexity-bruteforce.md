---
title:  "시간 복잡도와 완전 탐색" 

categories:
  - Algorithm Study
tags:
  - [algorithm, bruteforce]
toc: true
toc_sticky: true
use_math: true
date: 2023-03-10
last_modified_at: 2022-03-10
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
보통 코딩 테스트 문제의 시간 복잡도는 **1초 안(= 1억번)**으로 해결해야 한다. 

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
<br/><br/>
[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}
