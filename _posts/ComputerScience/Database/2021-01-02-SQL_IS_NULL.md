---
title: Programmers SQL 고득점 Kit - IS NULL편
excerpt: SQL 고득점 Kit - IS NULL편
search: true

categories:
  - Database
  
tags: 
  - [SQL, Coding Test, IS NULL]

last_modified_at: 2021-01-02
---



### IS NULL

[programmers SQL 고득점 Kit - IS NULL 편](https://programmers.co.kr/learn/courses/30/parts/17045)

#### 1. 이름이 없는 동물의 아이디

- IS NULL

  => Select ~ From ~ WHERE (COLUMN명) IS NULL 

  ```sql
  SELECT ANIMAL_ID FROM ANIMAL_INS WHERE NAME IS NULL
  ```

  

#### 2. 이름이 있는 동물의 아이디

- IS NOT NULL

  => Select ~ From ~ WHERE (COLUMN명) IS NOT NULL 

  ```sql
  SELECT ANIMAL_ID FROM ANIMAL_INS WHERE NAME IS NOT NULL
  ```

  

#### 3. (*) NULL 처리하기

- IF(조건문, 참일때 출력값, 거짓일때 출력값)

- MySQL에서 조건문 사용하기

  => SELECT IF(조건문, 참이면, 거짓이면) FROM ~

  ```sql
SELECT ANIMAL_TYPE, IF(NAME IS NULL, "No name", NAME) AS NAME, SEX_UPON_INTAKE FROM ANIMAL_INS ORDER BY ANIMAL_ID
  ```
