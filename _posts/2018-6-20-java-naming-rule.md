---
layout: post
title: "[Java] Java Naming Rule"
author: "Qoo"
category: Java
---
# 명명규칙(Naming Rule)
명명규칙은 프로그래밍을 할 때 어떻게 이름을 생성할 것 인가에 대한 규칙이다.

변수이름은 어떻게 할 것인가? 함수이름은 어떻게 할 것인가? 

이런 고민은 개발자라면 한번 쯤 해봤을 것이다.

아무리 코드를 멋지고 간결하게 작성해도 명명규칙이 통일되어 있지 않다면 가독성이 매우 떨어지는 코드가 될 것이다. 

회사의 팀, 조직에서 사용하는 명명 규칙이 있다면 그 것을 사용하자! 

사용하는 명명 규칙이 없다면 일반적으로 많이 사용 하는 방법은 CamelCase를 적용해 보자.



## CamelCase

- 각 단어의 첫문자를 대문자로 표기하고 붙여쓰되, 맨처음 문자는 소문자로 표기함

- 띄어쓰기 대신 대문자로 단어를 구분하는 표기 방식

ex) getUser, toString, deleteUser, updateService 등 


자바에서는 일반적으로 어떻게 사용하는지 알아보자.

클래스

클래스는 항상 대문자로 시작하고 나머지는 CamelCase를 따른다.

ex) ArrayList, LinkedList, UserDao, MainActivity, HttpClient 등


변수, 메소드
변수와 메서드는 카멜케이스를 따른다.
ex) customService, getInstance 등 다양하다.

상수 
변경할 수 없는 상수는 보통 static final 으로 사용하며 모두 대문자를 사용한다.
public static final int NOT_FOUND = 404;
public static final String TRUE = "TRUE";

이 정도의 아주 간단한 명명 규칙만 지켜도 코드가 많이 가독성이 좋아질 것이고, 추가적으로 자신만의 명명 규칙이 생길 것이다.