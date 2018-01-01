---
layout: post
title: "[Spring] Spring 5 reactive programming"
author: "Qoo"
category: Spring
---

# Spring5 reactive programming
스프링 5는 Reactive Programming 에 중점을 두고 있고, 이 패러다임으로 서버와 클라이언트를 모두 작성할 수 있다.
Spring 5 구현은 널리 사용되는 Project Reactor 를 기본으로 사용합니다.

## Reactive Programming
간단히 말하면, 이는 전통적인 프로그래밍 솔루션의 non-blocking 모델입니다.

Reactive 서버는 나머지 어플리케이션은 데이터가 처리되는 동안 대기를 할 필요가 없으며, 작업 스레드가 다른 리소스를 기다리지 않기 때문에 기존 구현보다 더 확장할 수 있다.

#### 1. 기존 처리 
```java
List<String> names 
  = userDao.getUsers().stream()
  .map(user -> user.getName())
  .collect(Collectors.toList());
```

#### 2. reactive model 처리
```java
Flux<String> names 
  = reactiveUserDao.getUsers()
  .map(user -> user.getName());
```

크게 달라진 것이 없어보이지만, 2번은 reactive기 때문에 전체 명령이 non-blocking이 되므로 메인 스레드가 작업에 묶이지 않습니다.
추가적으로, 호출자도 reactive일 때 non-blocking 특성이 내내 전파된다.

웹 서버라면 요청을 처리하는 스레드는 즉시 다른 요청을 처리할 수 있고, 데이터는 데이터베이스에서 나타나고, 이는 자동으로 클라이언트로 전송된다.


#### 참고
https://stackify.com/reactive-spring-5/
http://www.baeldung.com/spring-5-functional-web
https://docs.spring.io/spring/docs/5.0.0.M4/spring-framework-reference/html/web-reactive.html