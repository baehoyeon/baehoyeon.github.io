---
layout: post
title: "[SWIFT] Guard Statements"
author: "Qoopooh"
category: Swift
---

# Guard Statements
Swift 2 부터 사용할 수 있는 문법이다.

if 로 대체할 수 있지만,  <조건식>을 피하기 위해서 사용하는데 특화되어 있습니다.

## 사용법
```
guard (조건식) else {
  (실팽코드)
}
```
조건식이 false 일 때 코드를 실행하도록 구현할 수 있습니다.

### 예제
```coffeescript
func save (name: String, phoneNumber: String) {
    guard name != "" else {
        print("cannot save! name is empty!")
        return
    }
    
    guard phoneNumber != "" else {
        print("cannot save! phoneNumber is empty!")
        return
    }
    
    print("save complete! name : \(name) phoneNumber: \(phoneNumber)")
}

var name = ""
var phoneNumber = "011-9992-1111"
save(name: name, phoneNumber: phoneNumber)

name = "Qoo"
phoneNumber = ""
save(name: name, phoneNumber: phoneNumber)

name = "Qoo"
phoneNumber = "011-9992-1111"
save(name: name, phoneNumber: phoneNumber)

```

### 결과
```
cannot save! name is empty!
cannot save! phoneNumber is empty!
save complete! name : Qoo phoneNumber: 011-9992-1111
```

### 예제 with Optional Binding
옵셔널의 값을 벗겨서 값이 있다면 조건문 안으로 들어가고, 값이 nil 이라면 그냥 통과하게 됩니다.
```coffeescript
class User {
    var name: String
    var phoneNumber: String
    
    init(name: String, phoneNumber: String) {
        self.name = name
        self.phoneNumber = phoneNumber
    }
}

func save (user : User) {
    let optionalName :String? = user.name
    let optionalPhoneNumber :String? = user.phoneNumber
    
    guard let name = optionalName, !name.isEmpty else {
        print("cannot save! name is empty!")
        return
    }

    guard let phoneNumber = optionalPhoneNumber, !phoneNumber.isEmpty else {
        print("cannot save! phoneNumber is empty!")
        return
    }
    
    print("save complete! name : \(name) phoneNumber: \(phoneNumber)")
}

var user = User(name: "", phoneNumber: "011-9992-1111");
save(user: user)

user = User(name: "Qoo", phoneNumber: "");
save(user: user)

user = User(name: "Qoo", phoneNumber: "011-9992-1111");
save(user: user)
```

### 결과
```
cannot save! name is empty!
cannot save! phoneNumber is empty!
save complete! name : Qoo phoneNumber: 011-9992-1111
```