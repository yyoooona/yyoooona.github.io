---
title: Programmers SQL 고득점 Kit - SUM,MAX,MIN편
excerpt: SQL 고득점 Kit - SUM,MAX,MIN편
search: true

categories:
  - Database
  
tags: 
  - [SQL, Coding Test, SUM, MAX, MIN]

last_modified_at: 2021-01-01
---



### SUM, MAX, MIN

[programmers SQL 고득점 Kit - SUM, MAX, MIN편](https://programmers.co.kr/learn/courses/30/parts/17043)

#### 1. 최댓값 구하기

- MAX

  => Select MAX(COLUMN명) From ~ 

  ```sql
  SELECT MAX(DATETIME) FROM ANIMAL_INS
  ```

  

#### 2. 최솟값 구하기

- MIN

  => Select MIN(COLUMN명) From ~ 

  ```sql
  SELECT MIN(DATETIME) FROM ANIMAL_INS
  ```

  

#### 3. 동물 수 구하기

- COUNT

  => SELECT COUNT(COLUMN명) || COUNT(*) FROM ~

  ```sql
SELECT COUNT(*) FROM ANIMAL_INS
  ```



#### 4. (*) 중복 제거하기

- DISTINCT

  - SQL 중복제거

    => SELECT DISTINCT (COLUMN명) FROM ~

  - (*) COUNT와 함께 쓰면

    => SELECT COUNT(DISTINCT(COLUMN명)) FROM ~ WHERE ~

  

- IS NOT NULL

  - NULL값이 아닌 경우 조건문 WHERE절에 넣어준다
  - NULL 값인 경우 IS NULL

  ```sql
  SELECT COUNT(DISTINCT(NAME)) FROM ANIMAL_INS WHERE NAME IS NOT NULL
  ```
