---
layout: post
title: "[ETC] MarkDown Grammar"
author: "Qoo"
category: Etc
---

# MarkDown
## 제목
\# 의 개수로 h1~h6를 구분합니다.

```markdown
#테그 h1
##테그 h2
###테그 h3
####테그 h4
#####테그 h5
######테그 h6
```
### 결과
#테그 h1
##테그 h2
###테그 h3
####테그 h4
#####테그 h5
######테그 h6

## 문단
하나 이상의 개행을 통해서 구분합니다.
```
나는 소프트웨어 개발자입니다.
개발이 좋아요?
 
나는 소프트웨어 개발자입니다.
개발이 좋아요?
```
### 결과
````html
<p>나는 소프트웨어 개발자입니다.
개발이 좋아요?</p>
<p>나는 소프트웨어 개발자입니다.
개발이 좋아요?</p>
````

## 코드
\`\`\` 과 \`\`\` 사이에 넣으면 코드를 입력할 수 있습니다.
c, c++, c#, clojure, go, java, javascript, python, python3, ruby, scala, swift등 을 지원합니다.

java를 입력하고 싶다면

### 예
\```java

{content}

\``` 
를 입력하면 됩니다.

### 결과
```java
public class ClassicMusic {
	public void musicStart(Instrument instrument){
		instrument.play();
	}
}
```

## 링크
\[텍스트\]\(연결할 링크\) 이런 형식으로 입력할 수 있다.

### 예
\[BLOG](https://baehoyeon.github.io)

### 결과
[BLOG](https://baehoyeon.github.io)

## 이미지
```
![{altname}]({src})
<img src="" width="">
```
### 예
\!\[](../images/tree_6.png)

### 결과
![](../images/tree_6.png)

