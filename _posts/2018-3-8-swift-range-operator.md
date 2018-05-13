---
layout: post
title: "[SWIFT] Range Operators"
author: "Qoo"
category: Swift
---

# Range Operators
## The closed range operator
```
a...b
```
a 부터 b까지 a와 b를 포함한 범위를 나타낸다.

### 예
```
for idx in 1...10 {
    print("idx : \(idx)")
}

```
### 결과
```
idx : 1
idx : 2
idx : 3
idx : 4
idx : 5
idx : 6
idx : 7
idx : 8
idx : 9
idx : 10
```

## Half-Open Range Operator
```
a..<b
```
a 부터 b까지 b를 포함하지 않은 범위를 나타낸다.

### 예
```
for idx in 1..<10 {
    print("idx : \(idx)")
}
```
### 결과
```
idx : 1
idx : 2
idx : 3
idx : 4
idx : 5
idx : 6
idx : 7
idx : 8
idx : 9
```

## One-Sided Ranges
한쪽으로만 범위를 나타낼 수 있다.

### 예
```
let nameList = ["Qoo", "Bro", "Zoe", "Berry", "Pooh"]

print("nameList[3...]")
for name in nameList[3...] {
    print(name)
}

print("nameList[...2]")
for name in nameList[...2] {
    print(name)
}

print("nameList[..<2]")
for name in nameList[..<2] {
    print(name)
}
```

### 결과
```
nameList[3...]
Berry
Pooh
nameList[...2]
Qoo
Bro
Zoe
nameList[..<2]
Qoo
Bro

```

### 참고
https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/BasicOperators.html