---
published: true
layout: post
title: "[network]HTTP "
category: etc
tags: [network]
comments: true
classes: wide
---

## HTTP
HTTP는 Hypertext Transfer Protocol의 약자로 브라우저가 웹 서버와 통신하기 위해 사용하는 프로토콜이다.
HTTP에는 다음과 같은 메소드들이 있다.

+ GET : 정보를 요청하기 위해서 사용한다. (SELECT)

+ POST : 정보를 밀어넣기 위해서 사용한다. (INSERT)

+ PUT : 정보를 업데이트하기 위해서 사용한다. (UPDATE)

+ DELETE : 정보를 삭제하기 위해서 사용한다. (DELETE)

+ HEAD : (HTTP)헤더 정보만 요청한다. 해당 자원이 존재하는지 혹은 서버에 문제가 없는지를 확인하기 위해서 사용한다.

+ OPTIONS : 웹서버가 지원하는 메서드의 종류를 요청한다.

+ TRACE : 클라이언트의 요청을 그대로 반환한다. 예컨데 echo 서비스로 서버 상태를 확인하기 위한 목적으로 주로 사용한다.

위와 같은 메소드로 client가 Request 하면 server는 이를 받아 해석하고 요청에 해당하는 Response를 전달한다.
