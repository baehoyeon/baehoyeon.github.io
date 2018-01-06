---
layout: post
title: "[JAVA] String constant pool"
author: "Qoo"
category: Java
---

## String constant pool
예제부터 보자

### 코드
```java
public class ConstantPoolTest {

	public static void main(String[] args) {
		String w = "pooh";
		String x = "pooh";
		String y = new String("pooh");
		String z = new String("pooh");

		System.out.println("w.equals(x) : " + w.equals(x));
		System.out.println("w.equals(y) : " + w.equals(y));
		System.out.println("w.equals(z) : " + w.equals(z));
		System.out.println("x.equals(y) : " + x.equals(y));
		System.out.println("x.equals(z) : " + x.equals(z));
		System.out.println("y.equals(z) : " + y.equals(z));

		System.out.println("w == x : " + (w == x)); // 주목
		System.out.println("w == y : " + (w == y));
		System.out.println("w == z : " + (w == z));
		System.out.println("x == y : " + (x == y));
		System.out.println("x == z : " + (x == z));
		System.out.println("y == z : " + (y == z));
	}
}
``` 

### 실행결과
```
w.equals(x) : true
w.equals(y) : true
w.equals(z) : true
x.equals(y) : true
x.equals(z) : true
y.equals(z) : true
w == x : true
w == y : false
w == z : false
x == y : false
x == z : false
```

equals 와 == 의 차이는 equals는 String Value를 비교하고, == 은 주소를 비교한다고 생각하면 된다.

'//주목 ' 이 부분에 w==x가 왜 false가 아니고 true 라는 것은 같은 객체를 가르킨다는 것이다.

하지만 스트링을 선언하면 객체가 생기는 것이 아닌가?

new 를 사용해서 String 을 생성하면 Heap영역에 할당하기 때문에 가르키는 주소는 다를 것이다.

하지만 " " 를 이용해서 String을 선언하게 되면 런타임 데이터 영역 중 constant pool에 저장하고 주소 값 만을 가져온다.

그렇기 때문에 x = "pooh" 를 선언 할 때도 constant pool에 "pooh"가 있는지 확인하고 있기 때문에 주소만 가져온 것이다. 

이런 방법으로 Heap 영역에 할당되는 메모리를 줄인다.

String 객체가 immutable 의 특성을 지니고 있기 때문에 가능한 일이다.