+++
author = "IceBlueHalls"
title = "[백문이불여일타] 데이터 분석을 위한 기초 SQL 강의 요약"
date = "2023-04-09"
description = "인프런 강의 [백문이불여일타] 데이터 분석을 위한 기초 SQL 강의 요약"
tags = [
    "SQL"
]
categories = [
    "SQL"
]
series = ["SQL"]
aliases = ["SQL"]
image = "main.png"
slug = "sql-study-easy"
+++

# SQL 기초

## SELECT / FROM / LIMIT

### SELECT

SELECT : 데이터를 불러옵니다.
SELECT *일 경우, 조건 없이 모든 데이터를 가져옵니다.
SELECT {컬럼 이름} 일 경우, 해당 컬럼만 노출이 됩니다.

### FROM

FROM : 어느 티에블에서 가져올 것인지 설정합니다.
FROM {테이블 명}
FROM Customers 일 경우, Customer 테이블에 있는 데이터를 불러옵니다.

예시 : 
SELECT * FROM Customers
: Customers 테이블에 있는 모든 데이터를 불러옵니다.

SELECT CustomerName, Address FROM Customers
: Custmers 테이블에 있는 모든 데이터중 CustomersName과 Address만 노출하여 보여줍니다

### LIMIT 
LIMIT : LIMIT을 지정하지 않을 경우 모든 데이터를 불러올 수 있습니다. LIMIT은 상위 n개의 데이터만을 불러오게 합니다.

예시 :
SELECT * FROM Customers LIMIT 10
: Customers에 있는 데이터중 상위 10개의 row만 불러옵니다.

### Tip 1
SQL 문법은 강제성이 있는 것은 아니지만 가독성을 위해 대문자로 표기하는 것을 권장한다.
select (x), SELECT (o)

## 조건에 맞는 데이터 검색하기

### WHERE
WHERE : 비교연산자와 논리연산자. 컬럼 속 데이터의 조건에 따라 검색하는 것이 가능하다.

SELECT * FROM Customers WHERE CustomerName < "B"
: Customers의 전체 데이터중 CustomerName이 B로 시작하는 데이터를 가져와라

SELECT * FROM Customers WHERE CustomerID < 50
: Customers의 전체 데이터중 CustomerID가 50보다 작은 데이터를 가져와라

SELECT * FROM Customers WHERE CustomerName < "B" AND Country = "Germany"
: Customers의 전체 데이터중 CustomerName이 B로 시작하**면서** Country가 Germany인 데이터를 가져와라

SELECT * FROM Customers WHERE CustomerName < "B" OR Country = "Germany"
: Customers의 전체 데이터중 CustomerName이 B로 시작하**거나** Country가 Germany인 데이터를 가져와라

### LIKE
LIKE : 특정 문자열로 시작하거나, 끝나거나, 포함되어있는 데이터들을 가져오는 것이 가능하다.

%(와일드카드) : 기호를 통해 지정이 가능하다.

SELECT * FROM Customers WHERE country LIKE 'Br%'
: Customers의 전체 데이터중 country의 속성값이 Br로 시작하는 데이터들을 가져와라(Brazil등)

SELECT * FROM Customers WHERE country LIKE '%SA'
: Customers의 전체 데이터중 country의 속성값이 Br로 끝나는 데이터들을 가져와라(USA등)

SELECT * FROM Customers WHERE country LIKE '%TA%'
: Customers의 전체 데이터중 country의 속성값이 Br로 끝나는 데이터들을 가져와라(Italia, TAILLAND등)


SELECT * FROM Customers WHERE country LIKE 'BRAZIL'
: Customers의 전체 데이터중 country의 속성값이 Brazil인 데이터들을 가져와라. 다만 `LIKE 'BRAZIL'` 보다는 `= 'BRAZIL'`이 속도측면에서 훨씬 빠르다.

SELECT * FROM Customers WHERE country LIKE 'B_____'
: Customers의 전체 데이터중 country의 속성값이 B로 시작하는 5글자 짜리 데이터들을 가져와라.

SELECT * FROM Customers WHERE discount LIKE '50\%'
: Customers의 전체 데이터중 discount의 값이 50%인 데이터들을 가져와라.(% 기호는 예약어이므로 이스케이프를 통해 구분해야한다.)


### IN
IN : 일치하는 조건들을 여러 개 설정할 수 있다.

SELECT * FROM Customers WHERE country IN ( 'KOREA', 'JAPAN', 'USA')
: Customers의 전체 데이터중 country 값이 Korea, Japan, Usa 중 하나인 데이터들을 가져와라

### BETWEEN
BETWEEN : 지정한 두 값 사이에 포함되는 데이터들을 가져올 수 있다.

SELECT * FROM Customers WHERE population BETWEEN 1000 AND 20000
: Customers의 전체 데이터중 country 값이 population의 숫자형 데이터가 1000에서 20000 사이인 데이터들을 가져와라

### IS NULL
IS NULL : 해당 컬럼에 존재하지 않는 데이터들을 검색할 수 있다. NULL은 특수한 값이므로 '= NULL'은 검색이 불가능하다.

SELECT * FROM Customers WHERE customerID IS NULL
: Customers의 전체 데이터중 customerID가 NULL인 데이터들을 불러와라


(잘못된 예시)SELECT * FROM Customers WHERE customerID = NULL
: customerID가 NULL인 데이터가 불러와지지 않는다.

### Tip2
컬럼명은 대소문자 구분하지 않는다.
따옴표와 작은 따옴표도 구분하지 않는다.

### DISTINCT
DISTINCT : 중복되는 데이터를 없게 만들어준다.

SELECT DISTINCT city FROM station WHERE city LIKE 'a%' OR city LIKE 'america'
: america는 a% 조건과 'america'조건 모두 겹치지만 한번만 출력됩니다.

## 데이터 순서 정렬하기

### ORDER BY
ORDER BY : 오름차순 혹은 내림차순으로 데이터를 정렬한다. 보여주는 형식만 변경할 뿐 실제 데이터베이스의 순서가 바뀌지는 않는다.
기본은 오름차순으로 되어있으며, ASC를 입력하여도 오름차순으로 정렬된다.(값이 큰게 아래)
내림차순의 경우 DESC를 입력하면 된다.(값이 큰게 위)


SELECT * Customers ORDER BY customerid DESC
: 모든 Customers의 데이터들을 불러오 되 customerid가 내림차순으로 정렬하여 보여줘라.



### Tip3
명령어는 SELECT, FROM, WHERE, ORDER BY, LIMIT 순이다.

## 그 외 함수

### MOD
MOD : 앞의 수 에서 뒤의 수를 나눈 후, 나머지를 반환한다.

SELECT * FROM city WHERE MOD(customer_id, 2) = 0
: city 테이블의 전체 데이터중에서 customer_id를 2로 나누었을 때 나머지가 0인 데이터를 가져와라

### COUNT
COUNT : 검색된 데이터의 갯수를 구한다.
SELECT COUNT(ALL *) - COUNT(DISTINCT CITY) FROM STATION
: STATION 테이블의 (모든 데이터의 수) - (CITY가 중복되는 데이터의 수)를 반환하라

