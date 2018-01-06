---
layout: post
title: "[SWIFT] Multi line string literals"
author: "Qoo"
category: ISSUE
---

# Multi line string literals
Swift4 에서 부터는 Multi line string literals 을 지원합니다.

## 이전 버전
### 예
```text
var multiLineText: String = "괜찮아요.\n"
multiLineText += "내가 옆에 있어요.\n"
multiLineText += "사랑해요."
print(multiLineText)
```
### 결과
````text
괜찮아요.
내가 옆에 있어요.
사랑해요.
````

## Swift 4
```
"""
(내용)
"""
```
이런 포멧으로 작성할 수 있습니다.

첫번째 \"\"\" 뒤에는 아무것도 올 수 없습니다.

두번째 \"\"\" 앞에도 아무것도 올 수 없습니다.

### 예
```text
let multiLineText: String = """
괜찮아요.
내가 옆에 있어요.
사랑해요.
"""
print(multiLineText)
```
### 결과
````text
괜찮아요.
내가 옆에 있어요.
사랑해요.
````

