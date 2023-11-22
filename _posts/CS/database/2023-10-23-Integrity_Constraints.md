---
title:  "[Inter SQL] Integrity Constraints" 

categories:
  - Database System
tags:
  - [SQL]
toc: true
toc_sticky: true
use_math: true
date: 2023-10-23
last_modified_at: 2022-10-23
---


Chapter 4.4에 나와 있는 Integrity Constraints에 대한 내용 정리 

<br/>  
## Constraints on Single Relation

**Integrity Constraints**는 특정 제약 조건을 통해 데이터베이스에 접근을 제한해서 내부의 데이터의 integrity(무결성)를 지키도록 하는 역할을 한다. 
<br/>
### **Not Null**

테이블을 생성할 때, attribute 도메인 뒤에 붙여서 해당 속성이 null 값을 가질 수 없도록 지정한다. 

> <*attribute*> <*domain*> **not null**
> 

ex. *name*, *budget* 속성은 null 값을 가질 수 없다. 

```sql
/* create table 내부에 있는 코드 */
name varchar(20) not null 
budget numeric(12,2) not null
```
<br/>
### Primary key

해당 속성이 Primary key(고유 키)가 되도록 설정한다. 

> **primary key** ($A_1$, $A_2$, … , $A_m$)
> 
- null 값을 포함할 수 없다.
- attribute 혹은 attribute의 조합이 unique해야 함
- 한 번 지정되면 Primary key의 값은 변경할 수 없다.

<br/>
### **Unique**

해당 attribute set에 있는 모든 속성들이 unique하게 구분된다는 것을 보장해준다. 즉,  해당 속성들이 candidate key가 될 수 있도록 설정하는 것.

> **unique** ($A_1$, $A_2$, … , $A_m$)
> 

**Unique**와 **Primary key**의 차이점 

- Unique는 null값을 포함할 수 있지만, Primary key는 포함할 수 없다.
- Primary key는 한 테이블 당 하나의 속성 combination만 가능하지만
    
    Unique는 여러 개의 속성을 Candidate key로써 설정할 수 있다. 
    
<br/>
### **Check (P)**

P = Predicate로, attribute에 속하는 모든 튜플이 조건 P를 만족하는지 체크한다. 여기서 predicate(명제)는 참과 거짓으로 구분될 수 있는 문장을 의미한다. 

> **check** (<*predicate*>)
> 

ex. *semester* 속성 값이 Fall, Winter, Spring, Summer 중 하나에 해당하도록 함

![image](https://github.com/binnie723/binnie723.github.io/assets/86834982/54a84294-361e-4d24-bf6f-d786c7744916){: width="500px"} 


**Complex Check Conditions**

일부 데이터베이스 시스템에 한해서 <*predicate*> 자리에 subquery를 넣을 수 있다. 

ex.  *section* 테이블의 *time_slot_id*에 대한 제약 조건

`check (time_slot_id in (select time_slot_id from time_slot))`

이 경우, *section* 뿐 만 아니라 *time_slot* 테이블에 변경사항이 추가될 때마다 조건을 다시 체크한다.  


<br/>   
## Referential Integrity

Foreign key를 사용하는 과정에서, 즉 서로 참조하는 과정에서 integrity(무결성)을 유지하도록 한다. 

**Referential Integrity**는 하나의 relation에서 나타난 값들이 다른 relation에서도 똑같이 나타나도록 하는 것을 말하며, Foreign Key로 지정된 열의 값은 반드시 참조하는 테이블의 Primary Key 값 중 하나여야 한다. 
<br/>  
### Foreign Key

**Foreign key**(외래키)는 다른 테이블의 Primary key를 참조하는 열을 말한다. 두 테이블 간의 관계를 만들고 설정하는데 있어서 중요한 역할을 한다. 

테이블을 생성할 때 Foreign key도 같이 지정할 수 있다. 

> **foreign key**  <*attribute*>  **references**  <*relation*>
> 
> 
> **foreign key**  <*attribute*>  **references**  *relation*(<*attribute*>)
> 
- null 값을 포함할 수 있다.
- 하나의 테이블에 여러 개의 Foreign keys를 가질 수 있음

<br/>  
### Cascading Actions

Foreign Key가 참조하는 Primary Key 값을 변경(update)하거나 삭제(delete)하려고 할 때, referential integrity가 위반된다. 

ex.  참조되는 *Students* 테이블에서 행이 삭제되거나, pk 값이 변경되는 경우 

![image](https://github.com/binnie723/binnie723.github.io/assets/86834982/847fd8ba-9c95-4dfd-8c44-9c356e737739){: width="500px"} 

보통 요청을 무시하지만 특정 옵션을 통해 fk의 값을 어떻게 처리할지 지정할 수 있다.

- **set null** : Foreign key 값을 NULL로 설정
- **set default**  : Foreign key 값을 default로 설정
- **on delete cascade** : 해당 Primary key 값을 참조하는 모든 행을 삭제
- **on update cascade** : 해당 변경 사항을 Foreign key 값을 가진 모든 행에 반영
    
    ex. *department*를 참조하는 *course*에 cascade 옵션 설정
    
    ![image](https://github.com/binnie723/binnie723.github.io/assets/86834982/493ed038-dda3-4d85-be28-322136356987){: width="500px"} 
    
<br/>
이렇게 여러 옵션들을 통해 **Referential Integrity를 보장**해줄 수 있다.

<br/>  
<br/>  
@Database System Concepts sixth edition 내용을 참고함
    
<br/><br/>
[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}

