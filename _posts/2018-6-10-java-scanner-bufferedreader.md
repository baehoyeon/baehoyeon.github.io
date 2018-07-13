---
layout: post
title: "[Java] Java Scanner vs BufferedReader"
author: "Qoo"
category: Java
---
자바 1.5버전부터는 BufferedReader를 대신할 만한 Scanner라는 클래스가 생겼다. 
BufferedReader는 무조건 String형으로 받기 때문에 다른 형으로 형 변환 해줘야 하는 불편함도 가지고 있다. 


하지만 Scanner는 토큰을 이용하며 파싱을 할 수 있게 되는 등 다양한 기능들을 내장하고 있고, 자료형에 맞게 선택해서 사용할 수 있기 때문에 형 변환을 해줘야 하는 불편함을 해소할 수 있다. 

nextInt(), nextDouble(), nextLine() 등 각자의 입맛에 따라서 선택해서 할 수 있다. 

하지만 Scanner의 버퍼 사이즈는 1024 chars이고, Buffered Reader의 버퍼 크기는 8192 chars이기 때문에 많은 입력이 있다면 BufferedReader가 성능상 우위를 가질 수밖에 없다. 

이 둘을 선택할 때, 어떻게 선택을 할지 고민해봐야 한다. 

파싱이 필요한 작업이라면 Scanner를 사용하는 것이 좋고, 단순히 한 줄 씩 읽는 작업이라면 Buffered Reader 늘 사용하는 편이 훨씬 Performance가 좋다.
