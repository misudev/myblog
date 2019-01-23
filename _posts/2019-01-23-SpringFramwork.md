---
layout: post
title: "[Spring] Spring Framework 란?"
category: Spring
tags: [Spring]
---
Framework란?
------------------
'반제품'같은 개념이다. 필요한 부분, 어려운 부분이 구현되있는 반제품을 이용해서 내가 원하는 제품을 손쉽게 만들 수 있다.


**라이브러리 vs 프레임워크**  
1. 라이브러리 : 단순히 필요한 가져다 쓰는 것, 내가 어플리케이션의 흐름을 직접 제어한다.  
2. 프레임워크 : 내가 작성한 코드가 프레임워크에 의해 실행된다. 즉 제어의 역전이 일어난다. 라이브러리 형태로 제공되는 프레임워크는 처음에는 내가 부르지만 이 프레임워크가 정한 틀 내에서 작업해야 한다.  

----------------

Spring Framework 란?
---------------------
- 엔터프라이즈급 어플리케이션을 구축할 수 있는 가벼운 솔루션이자, One-Stop-Shop (모든 과정을 한꺼번에 해결하는 상점)이다.
- 원하는 부분만 사용할 수 있게 모듈화가 잘 되어있다.
- IoC 컨테이너이다.  
>  **IoC 컨테이너**  
  프레임워크에서 객체의 생성부터 생명주기 관리까지 객체에 대한 제어권을 가지는 것을 '컨테이너'라고 한다. 이는 개발자가 아닌 컨테이너에게 제어가 넘어갔기 때문에 제어의 역전(IoC)이라고 한다.   
- 선언적으로 트랜잭션을 관리할 수 있다.
- 완전한 기능을 갖춘 MVC Framework를 제공한다.
- AOP를 지원한다.(Aspect Oriented Programming)  
>  **AOP**  
  AOP는 핵심기능과 공통기능을 분리시키고, 핵심 기능에 영향을 미치지 않게 중간 중간 공통기능을 잘 끼워넣어주는 개발 방식이다.

Spring Framwork의 구조
----------------------
<center><img src = '/assets/image/spring-structure.png' width = '700' height = '450' /></center>


**출처 사이트**  
부스트코스 - 웹프로그래밍 https://www.edwith.org/boostcourse-web/lecture/20655/  
블로그 https://jongmin92.github.io/2018/02/11/Spring/spring-ioc-di/
