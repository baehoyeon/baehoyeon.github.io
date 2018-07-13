---
layout: post
title: "[Java] Java Parameter Passing"
author: "Qoo"
category: Java
---
자바의 인자 전달 방식에 대해서 말하기 앞서서, 얼마 전 친구의 공무원 전산 시험문제를 구경하다가 만난 문제입니다. 

이 문제를 보고 생각이나서 간단히 포스팅 합니다.

결과를 예측해 봅시다.


코드) 
```java
public class ParaPassing {
    public void change(int i, int[] j){
        i = 20; j[3] = 400;
    }
         
    public void display(int i, int[] j){
        System.out.println("i: " + i);
        System.out.println("j: ");
        for(int k = 0; k<j.length; k++)
            System.out.print(j[k] + " ");
        System.out.println();
}
 
public class ParaPassingTest {
    public static void main(String[] args){
        ParaPassing pp = new ParaPassing();
        int i = 10;
     int j[] = {1, 2, 3, 4};
        pp.change(i, j);
        pp.display(i, j);
    }
}
```

결과 ) 
```
i: 10
j: 1 2 3 400
```

이 결과를 쉽게 예측 할 수 있었다면 더이상 포스팅을 보지않아도 좋습니다.

결과적으로 말하면 자바에서 메소드에 변수를 전달할 때 

기본 데이터 타입(int, char .....) 은 Call-By-Value 로 처리를 하고, 기본 데이터 타입이 아닌 객체 타입은 Call-By-Reference로 처리를 하기 때문입니다.

### Call-By-Value 
Call-By-Value는 말그대로 값으로 부른다 입니다. 값을 복사해서 전달을 한다고 생각을 하시면 좋습니다. 그렇기 때문에 함수 안에서 지역변수의 역할밖에 하지 못합니다.

### Call-By-Reference 
Call-By-Reference는 말그대로 참조로 부른다 입니다. 주소를 전달한다고 생각하시면 좋습니다.

간단히 예를 들어보겠습니다.

```java
public static void main(String[] args){
    ParaPassing pp = new ParaPassing();
    int i = 10;
    int j[] = {1, 2, 3, 4};
	pp.change(i, j);
	pp.display(i, j);
}

public void change(int i, int[] j){
	i = 20; j[3] = 400;
}

```
위의 코드에서 실행 된 결과입니다.

main() 함수에서 change() 메소드로 i,j를 전달하고 있습니다. 


#### 1) 마치 change()에서 i=20;을 해서 main()함수의 i가 바뀔 것 같지만 바뀌지 않습니다.

이유는 기본 데이터 형이기 때문에 Call-By-Value를 사용합니다.

change()함수의 i는 main() 함수의 i 에서 값만 복사해 와서 change() 함수의 지역 변수로써 역할을 하고 있기 때문에 main() 함수의 i에 영향을 미치지 않습니다.  


#### 2) 하지만, change()에서 j[3] = 400;을 해서 main()함수의 j가 바뀐것을 알 수 있습니다.

이유는 array는 객체 타입이기 때문에 Call-By-Reference를 사용합니다.

change()함수의 j는 main()함수의 j 값에서 주소를 전달하기 때문에 main() 함수의 j 값을 가르키고 있다.

그래서 실제로 값을 변경하면 main()함수의 j에 영향을 미치게 됩니다.  


#### cf) JVM에서 배열은 Object로 취급되어 Heap 영역에 Object를 생성하는 Object 이다.
