---
layout: post
title: "[SWIFT] String template"
author: "Qoo"
category: ISSUE
---

# String template
Swift 에서 문자열 템플릿 처리를 어떻게 하는지 알아보겠습니다.

문자열 템플릿 처리를 할 때 Java 같은 경우에는
### 예 
```java
String.format("에러 내용은 %s", "NullPointerException");
```
### 결과
```text
에러 내용은 NullPointerException
```
이런식으로 처리하곤 합니다.

## Swift String template
스위프트에서는 원하는 위치에 
```
\(<변수명>)
```
를 넣어주면 끝입니다.

예를 통해서 Swift 에서는 어떻게 처리하는지 알아보겠습니다.

### 예
```text
var errorMessage : String

let errorVariable = "제목"
errorMessage = "\(errorVariable) 이 없습니다."
print(errorMessage)

let errorCount = 100
errorMessage = "\(errorCount) 건의 에러가 발생했습니다."
print(errorMessage)

let tomcatErrorCount = 500
let nginxErrorCount = 1000
errorMessage = "오늘 하루 총 \(nginxErrorCount + tomcatErrorCount) 건의 에러가 발생했습니다."
print(errorMessage)

```

### 결과
```text
제목 이 없습니다.
100 건의 에러가 발생했습니다.
오늘 하루 총 1500 건의 에러가 발생했습니다.
```

