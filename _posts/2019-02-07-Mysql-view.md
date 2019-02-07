---
published: true
layout: post
title: "[DataBase] MySQL - View"
category: MySQL
tags: [MySQL]
comments: true
---

# View란?

뷰는 가상테이블로 실제로 존재하는 테이블은 아니지만, 테이블 처럼 조작할 수 있는 가상테이블이다.

뷰는 SELECT를 객체로 만든 기법으로 SELECT의 결과를 가상의 테이블로 만든 것이다. 

뷰의 장점은 복잡한 쿼리문을 간단하게 묶어서 사용할 수 있으며, 여러개의 쿼리문을 하나로 묶어둘 수 있다는 것이다. '쿼리 바로가기'라고 생각할 수 있다.

뷰의 생성문은 다음과 같다.

```
CREATE VIEW 뷰이름 AS

SELECT 필드이름1, 필드이름2, ...

FROM 테이블이름

WHERE 조건
```

뷰도 테이블과 같이 insert, update, delete가 가능하다. 하지만 대개 select를 위해 사용한다.

뷰의 호출, 삭제, 구조보기는 다음과 같다.

```
// View 호출
Select * From 뷰이름;
// View 삭제
Drop View 뷰이름;
// View 구조보기
Desc 뷰이름;
// View 목록 보기 ( 테이블도 함께 나온다. )
Show tables;
```

**주의점**

1. 뷰에 SUM, COUNT, AVG 와 같은 집계 값을 사용한다면, 데이터 변경에 뷰를 사용할 수 없다.
2. 뷰에 Group By, Distinct, Having 이 포함되어 있는 경우 데이터를 변경할 수 없다.
3. 테이블을 삭제하기전에 반드시 뷰를 먼저 삭제해야 한다.



-------------



출처 : https://m.blog.naver.com/PostView.nhn?blogId=seilius&logNo=130165456506&proxyReferer=https%3A%2F%2Fwww.google.com%2F

https://sjs0270.tistory.com/54