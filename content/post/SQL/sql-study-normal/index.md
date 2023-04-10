+++
author = "IceBlueHalls"
title = "[백문이불여일타] 데이터 분석을 위한 중급 SQL 강의 요약"
date = "2023-04-10"
description = "인프런 강의 [백문이불여일타] 데이터 분석을 위한 중급 SQL 강의 요약"
tags = [
    "SQL"
]
categories = [
    "SQL"
]
series = ["SQL"]
aliases = ["SQL"]
image = "main.png"
slug = "sql-study-normal"
+++

# SQL 중급

## COUNT
```sql
SELECT COUNT(*) FROM product  
```
product 테이블 내에 모든 row의 갯수를 구한다.  

```sql
SELECT COUNT(name) FROM product  
```
product 테이블 내에 name이 NULL이 아닌 모든 데이터를 반환  

```sql
SELECT COUNT(DISTINCT company) FROM product  
```
product 테이블 내에 company이 NULL이 아니고, 이름이 중복되지 않은 모든 데이터를 반환  

## AVG
```sql
SELECT AVG(total) FROM product  
```
product 테이블 내에 total의 평균을 구한다. 이때 NULL값은 계산에 포함이 안된다.  

```sql
SELECT SUM(total)/COUNT(*) FROM product  
```
product 테이블 내에 (total의 합계 / 총 row량)  구한다. Null은 계산에 대입이 되나 0으로 설정한다.  

## MIN
```sql
SELECT MIN(total) FROM product  
```
product 내에 total이 제일 큰 row를 구한다  

## MAX
```sql
SELECT MAX(total) FROM product  
```
product 내에 total이 제일 큰 row를 구한다  

## GROUP BY
```sql
SELECT SupplierID, AVG(Price)  
FROM Product  
GROUP BY SupplierID  
```
Product 테이블 내에 동일 SupplierID의 평균을 구해라  

```sql
SELECT SupplierID, CategoryID, AVG(Price)  
FROM Product  
GROUP BY SupplierID, CategoryID  
```
Product 테이블 내에 동일 SupplierID 중 Category 또한 동일한 값들의 평균을 구해라  


## WHERE
```sql
WHERE : 테이블의 모든 데이터 내에서 조건을 검색하고 그 이후에 GROUP BY를 한다  
SELECT SupplierID, CategoryID, AVG(Price)  
FROM Product  
WHERE Price >= 100  
GROUP BY SupplierID, CategoryID  
```
Product 테이블 내에 Price가 100골드 이상인 데이터만 가져와서 동일 SupplierID 중 Category 또한 동일한 값들의 평균을 구해라  

## HAVING
```sql
HAVING : GROUP BY 한 데이터 내에 검색되는 조건을 부여한다  
SELECT SupplierID, CategoryID, AVG(Price)  
FROM Product  
GROUP BY SupplierID, CategoryID  
HAVING AVG(Price)  
```
Product 테이블 내에 Price가 가져와서 동일 SupplierID 중 Category 또한 동일한 값들 중 평균 100골드 이상인 데이터들의 평균만 보여줘라  



(권장 X, 명확하게 하는 것이 가독성에 좋다)  
```sql
SELECT SupplierID, CategoryID, AVG(Price)  
FROM Product  
GROUP By 1, 2  
```
SELECT 내에 첫번째(SupplierID)와 두번째 요소(CategoryID)를 GroupBy 해라  

## Tip 1
```sql
SELECT SupplierID  
--    ,  CategoryID  
    , AVG(Price)  
FROM Products  
GROUP BY SupplierID  
       , CategoryID   
```
위와 같이 요소들을 분리하여 쓰면 주석 처리에 용이하다  

## 소수점 처리
### CEIL
SELECT CEIL(5.5)  
-> 6  

### FLOOR
SELECT FLOOR(5.5)  
-> 5  

### ROUND 
SELECT ROUND(5.1234, 1)  
-> 소수점 1번째 자리수까지만 보여주고 나머지는 반올림  

## CASE 
### CASE WHEN THEN END
조건에 맞는 데이터를 태깅해준다.(단순 보여주기)  

```sql
SELECT CASE  
           WHEN categoryId = 1 THEN "과자"  
           WHEN categoryId = 2 THEN "음료수"  
           WHEN 3 < categoryId AND categoryId < 10 THEN "화장품"  
           ELSE "비판매품"  
       END AS "CategoryName"  
FROM Market  
```
-> Market테이블에서 categoryId가 1이면 과자를, 2면 음료수를 3~10이면 화장품을 그 외 조건이면 비판매품이라고 태깅한다. 그리고 새로 만들어진 컬럼을 CategoryName이라고 칭한다.  

## JOIN
두 테이블의 연결고리를 이용하여 통합한다.  

### INNER JOIN ON
```sql
SELECT * 
FROM Orders
     INNER JOIN Customers ON Orders.CustomerID = Customers.CustomerID  
     INNER JOIN Shippers ON Orders.ShipperID = Shippers.ShipperID  
```
Orders 테이블에 있는 CustomerID와 Customers 테이블에 있는 CustomerID가 일치하는 것들을 매칭하여 한번에 보여준다.  
그리고 Orders 테이블에 있는 ShipperID와 Shippers 테이블에 있는 ShippersID가 일치하는 데이터 또한 Customers의 오른쪽에 표시한다.  
다만 Orders가 가지고 있지 않은 데이터는 보여지지 않는다.(Customers.CustomerID가 4까지 있고 Orders.CustomerID가 3까지 있으면 매칭되지 않고 리스트에서 제외된다.)  

### LEFT JOIN ON
```sql
SELECT *   
FROM Customers  
     LEFT JOIN Orders ON Customers.CustomerID = Orders.CustomerID  
```
Customers 테이블에 존재하는 데이터를 기준으로 Orders의 CustomerID와 매칭되는 데이터들을 같이 표시한다. 이때 Orders에 없다면 Null로 표시한다.  

### LEFT JOIN ON
```sql
SELECT *   
FROM Customers  
     RIGHT JOIN Orders ON Customers.CustomerID = Orders.CustomerID  
```
Orders 테이블에 존재하는 데이터를 기준으로 Customers의 CustomerID와 매칭되는 데이터들을 같이 표시한다. 이때 Orders에 없다면 Null로 표시한다.  
다만 사람은 주로 왼쪽부터 보기 때문에 LEFT만 사용하며 값을 바꾸는 것을 추천한다.  
