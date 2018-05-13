---
layout: post
title: "[Java] Reactive Programming"
author: "Qoo"
category: Java
---
# Reactive Programming
간단히 말하면, 이는 전통적인 프로그래밍 솔루션의 non-blocking 모델입니다.

Reactive 서버는 나머지 어플리케이션은 데이터가 처리되는 동안 대기를 할 필요가 없으며, 작업 스레드가 다른 리소스를 기다리지 않기 때문에 기존 구현보다 더 확장할 수 있습니다.

### 1. 기존 처리 
```java
List<String> names 
= userDao.getUsers().stream()
  .map(user -> user.getName())
  .collect(Collectors.toList());
```

### 2. reactive model 처리
```java
Flux<String> names 
  = reactiveUserDao.getUsers()
  .map(user -> user.getName());
```

크게 달라진 것이 없어보이지만, 2번은 reactive기 때문에 전체 명령이 non-blocking이 되므로 메인 스레드가 작업에 묶이지 않습니다.

추가적으로, 호출자도 reactive일 때 non-blocking 특성이 내내 전파 됩니다.

웹 서버라면 요청을 처리하는 스레드는 즉시 다른 요청을 처리할 수 있고, 데이터는 데이터베이스에서 나타나고, 이는 자동으로 클라이언트로 전송됩니다.


# Reactor
[Reactor Core](https://github.com/reactor/reactor-core) 는 reactive programming  모델을 구현하는 Java 8 라이브러리 입니다.

Reactive Application을 만들기 위한 표준인 [Reactive Streams Specification](http://www.reactive-streams.org/) 위에 구축이 되었습니다.

스프링 5는 Reactive Programming 에 중점을 두고 있고, 이 패러다임으로 서버와 클라이언트를 모두 작성할 수 있습니다.

Spring 5 구현은 널리 사용되는 Project Reactor 를 기본으로 사용합니다.

## Reactive Streams Specification
기본적으로 비동기 스트림 처리를 위한 이고, 많은 이벤트가 생성되고 비동기적으로 소비되는 시스템입니다.

소비자에게 이벤트를 처리할 수 있는 것보다 더 빨리 이벤트를 보내는 생산자가 있다면, 결국 소비자는 시스템 리소스가 부족해져 다운될 수 있습니다.
Backpressure 는 소비자가 생산자에게 이런 문제를 방지하기 위해서, 얼만큼에 데이터를 보낼 지 알려 줄 수 있는것을 의미합니다.


## 구성
Reactor는 Mono 와  Flux 의 두 가지 핵심 유형으로 구성됩니다.

이 둘은 모두 데이터 스트림입니다.

### Mono
최대 1개 값의 스트림입니다.

### Flux
무한한 값의 스트림입니다.

이 두 유형의 차이는 상대적으로 매우 적습니다.

연산에 변경할 수 없는 단일값을 가지고있는 연산에 경우에는 Flux 가 아닌  Mono를 사용해야합니다.
물론 .. Flux 로도 사용할 수 있습니다.

JpaRepository의 findOne 을 생각하면 조금 더 이해가 쉬울 수 있습니다.

## 스트림 구독
스트림에서 데이터를 요청하는 것은 해당 스트림을 구독해서 수행됩니다.

물론 전통적인 방식으로 데이터를 얻고 싶다면, 데이터를 얻을 떄까지 stream 을 block 할 수 있습니다.

Mono 는 값이 해결되는 즉시 return 하는 single block() 을 사용할 수 있습니다..

Flux 는 blockFirst(), blockLast() 를 사용할 수 있습니다.

하지만 blocking 을 사용하려면 reactive stream 를 사용하는 의미가 없어지게 됩니다.

꼭 필요한 경우가 아니라면 subscribe 를 통해서 사용하는 편이 좋습니다. 

각각의 데이터가 얻어졌을 때 호출이 됩니다.

```java
reactiveUserDao.getUsers()
  .map(user -> user.getName())
  .subscribe(name -> System.out.println("name: " + name));
```
이렇게 되면 DAO 에서 데이터를 얻을 떄마다 print 가 호출되게 됩니다.

## Java Stream 과 차이
실제로 Java 콜렉션에 기능적으로 프로그래밍 기술을 제공하는 것 뿐입니다.
 
Reactive Stream 은 알 수없는 시간 동안 잠재적으로 무한한 수의 값을 생성 할 수있는 임의의 입력 소스를 가지도록 설계되었기 떄문에 이를 잘 처리합니다. 

추가적으로, Reactive Stream 는 push model 이고, Java 8 Streams 는 pull model 입니다.

Reactive Stream 은 event가 subscriber 가 들어오면 push 되는 모델입니다. 

또한, Project Reactor 스트림은 재사용이 가능 하며 Java 8 Streams 와는 큰 차이가 있습니다.

### 예 
```java
Flux<Integer> j = Flux.just(1, 2, 3, 4, 5);

j.map(i -> i * 10)
  .subscribe(System.out::println);

j.map(i -> i + 5)
  .subscribe(System.out::println);
  
```
재사용이 가능합니다.

```java
Stream<Integer> j = Arrays.asList(1, 2, 3, 4, 5).stream();

j.map(i -> i * 10)
  .forEach(System.out::println);

j.map(i -> i + 5)
  .forEach(System.out::println);
```
java 8 stream 은 IllegalStateException 이 발생하게 됩니다.

#### 참고
http://www.baeldung.com/reactor-core

https://stackify.com/reactive-spring-5/

http://www.baeldung.com/spring-5-functional-web

https://docs.spring.io/spring/docs/5.0.0.M4/spring-framework-reference/html/web-reactive.html