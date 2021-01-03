---
title: Programmers SQL 고득점 Kit - SELECT편
excerpt: SQL 고득점 Kit - SELECT편
search: true

categories:
  - Database
  
tags: 
  - [SQL, Coding Test, SELECT]

last_modified_at: 2020-12-30
---


### SELECT

[programmers SQL 고득점 Kit - SELECT편](https://programmers.co.kr/learn/courses/30/parts/17042)

#### 1. 모든 레코드 조회하기

- Order By

  => Select ~~~ From ~ where ~ Order By (정렬하고 싶은 열) ASC|DESC

  ```sql
  SELECT * FROM ANIMAL_INS
  ```

  

#### 2. 역순 정렬하기

- ```sql
  SELECT NAME, DATETIME FROM ANIMAL_INS ORDER BY ANIMAL_ID DESC
  ```



#### 3. 아픈 동물 찾기

- Like

  => 특정 문자 또는 문자열을 포함하고 있는 값을 검색하고 싶을 때 사용

  Like '%문자열%'

  LIKE의 부정은 NOT LIKE

  ```sql
  SELECT ANIMAL_ID, NAME FROM ANIMAL_INS WHERE INTAKE_CONDITION LIKE "SICK" ORDER BY ANIMAL_ID ASC
  
  ```



#### 4. 어린 동물 찾기

- 어린 동물은 INTAKE_CONDITION이 Aged가 아닌 경우기 때문에 Aged를 포함하지 않는 행을 출력해야 함

- LIKE의 부정은 NOT LIKE

  ```sql
  SELECT ANIMAL_ID, NAME FROM ANIMAL_INS WHERE INTAKE_CONDITION NOT LIKE 'Aged' ORDER BY ANIMAL_ID ASC
  ```



#### 5. 동물의 아이디와 이름

- Order By

  => Select ~~~ From ~ where ~ Order By (정렬하고 싶은 열) ASC|DESC

  ```sql
  SELECT ANIMAL_ID, NAME FROM ANIMAL_INS ORDER BY ANIMAL_ID ASC
  ```

  

#### 6. (*)여러 기준으로 정렬하기

- NAME이 같을 때,  DATETIME 내림차순

- SELECT ~ FROM ~ ORDER BY (COLUMN명) ASC|DESC, (COLUMN명) ASC|DESC 

  => ORDER BY를 이어서 사용할 수 있어, 먼저 이름을 사전 순으로(ASC) 우선적으로 정렬하고, 순차적으로 다음 정렬 실행

  ```sql
  SELECT ANIMAL_ID, NAME, DATETIME FROM ANIMAL_INS ORDER BY NAME ASC, DATETIME DESC
  ```



#### 7. (*)상위 n개 레코드

- 특정 개수의 값 출력할 때 TOP, LIMIT 쓴다

  (*) 단, 모든 DB시스템이 SELECT TOP을 사용하지는 않는다

  MYSQL은 LIMIT절, ORACLE은 ROWNUM 사용

  => Select ~~~ From ~ where ~ Order By (정렬하고 싶은 열) ASC|DESC LIMIT n(뽑고싶은 행 갯수 n)

  ```sql
  SELECT NAME FROM ANIMAL_INS ORDER BY DATETIME ASC LIMIT 1
  ```