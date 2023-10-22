---
title:  "[Inter SQL] Join Expressions" 

categories:
  - Database System
tags:
  - [SQL, join]
toc: true
toc_sticky: true
use_math: true
date: 2023-10-20
last_modified_at: 2022-10-20
---


Chapter 4는 더 복잡한 형태의 SQL query 구문과 그 형식들을 다루고 있다. 

이번에 정리해보려고 하는 것은 그 중에서 자주 쓰이는 **Join operation.**

<br/>  
## Join Expressions

두 개 이상의 relation을 묶어서 새로운 하나의 relation을 만든다. join을 하기 위해서는 **join type**과 **join condition**을 지정해주어야 한다. 

![ image](https://github.com/binnie723/binnie723.github.io/assets/86834982/46e87585-cf58-4b4e-bccd-3f07eb052897){: width="500px"} 
<br/>
**Join**은 relation에서 공통된 attribute을 기준으로 결합시키기 때문에 Cartesian Product와 유사하다. From 구문의 subquery로 사용된다.
  
**Join**종류는 다음과 같이 나눌 수 있다. 
- **Inner join**
- **Outer join** (Left, Right, Full)


<br/>
<br/>
### **Natural Join**

Natural join은 **같은 이름의 attribute**를 가진 두 칼럼을 자동적으로 결합해준다. 

> **select**  *A1, A2, … An*
**from**  *r1* **natural join** *r2* **natural join** .. **natural join** *rn*
**where** *P* ;
> 
  
**일반적인 SQL** 구문 (where 조건을 활용)

```sql
select * 
from student, takes
where student.ID = takes.ID;
```
    
**Natural join**을 사용한 구문 

```sql
select *
from student
natural join takes;
```
<br/>  
두 query의 결과는 위와 같다. 항목의 이름이 같다면, Natural join을 사용하는 편이 좋다! 
<br/>  
(참고) *student* 학생 테이블, *takes* 학생이 들은 과목 테이블

![image](https://github.com/binnie723/binnie723.github.io/assets/86834982/343585fd-9e91-44c9-b558-b8dc5784559b){: width="500px"} 

![image](https://github.com/binnie723/binnie723.github.io/assets/86834982/647d7087-b3a9-411b-9480-1b0d4316fb71){: width="500px"} 


<br/>
<br/>
### Inner Join

**Inner** **Join .. On**은 Natural join과 유사한 기능을 수행하지만, 명시적으로 조건을 지정해줄 수 있다는 점에서 다르다.  **on (또는 using)**을 통해 조건을 넣고, 성립하는 테이블의 칼럼들을 결합한다.  

> inner join *r1* on *(condition)* ;
> 
> 
> join *r1* on *(condition)* ;
> 
  
앞에 단어 명시 없이 그냥 join을 쓰는 경우, SQL은 자동적으로 inner join으로 인식한다. 

```sql
select *
from student inner join takes on student.ID = takes.ID;
/* ID가 일치할 때, student 튜플과 takes 튜플을 매칭시키겠다는 의미 */
```   
<br> 
조건을 직접 지정할 수 있다는 유연성 때문에 Natural join보다 실제에서 많이 사용된다. 

<br/>
<br/>
### **Outer Join**
  
**Natural Join의 문제점**

Natural Join과 Join .. On은 조건을 충족하지 못하는 **일부 칼럼이 누락**된다는 단점이 있다. 
  
ex.  학생들 *name*과 수강하는 과목 *title*을 출력하는 query를 보낸다고 하자.

![image](https://github.com/binnie723/binnie723.github.io/assets/86834982/c8153d68-aa64-4dba-b7bd-dd1120cfa123){: width="500px"} 

2번째 query에서 문제점이 발생한다. *course_id*와 *dept_name*이 동시에 필터링이 되면서 다른 department에서 수업을 듣는 학생들에 대한 정보가 모두 누락된다. 그래서 이와 같은 문제를 **해결**하기 위해 **Outer join**을 사용한다. **Outer join**은 먼저 join을 계산한 후, 매칭되지 못한 나머지 정보들을 **null로 업데이트**해서 포함시킨다.

  <br/>
**Outer join의 종류**

예시로 설명하기 위해서 다음과 같은 *course*, *prereq* 테이블이 있다고 하자. 

![image](https://github.com/binnie723/binnie723.github.io/assets/86834982/4cdf4eca-c73d-493b-a2d9-5d75591beb2a){: width="500px"} 
  
- **Left Outer Join** : 왼쪽에 있는 tuple 정보 보존
    
    `from course natural left outer join prereq`
    
    ![image](https://github.com/binnie723/binnie723.github.io/assets/86834982/4ded98ab-a3a4-4780-bf2b-e609c9b24fe3){: width="500px"} 
    
- **Right Outer Join :**  오른쪽에 있는 tuple 정보 보존
    
    `from course natural right outer join prereq`
    
    ![image](https://github.com/binnie723/binnie723.github.io/assets/86834982/500cd171-595c-4722-9559-8e08de00e3d8){: width="500px"} 
    
- **Full Outer Join :**  양쪽에 있는 tuple 정보 보존
    
    `from course natural full outer join prereq`
    
    ![image](https://github.com/binnie723/binnie723.github.io/assets/86834982/a2f0a6c4-9911-488c-a5b5-aeb905bb24ce){: width="500px"} 
    
<br/>
<br/>
@Database System Concepts sixth edition 내용을 참고함
  
  
    
<br/><br/>
[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}
