---
layout: post
title: "[JAVA] Access modifier"
author: "Qoopooh"
category: Java
---

# Access modifier
접근 제어자는 public, protected, private 3개 및 지정하지 않은default가 있습니다.

## public
모든 클래스에서 접근이 가능하다.

## protected
동일 패키지에 속하는 클래스와 하위클래스 관계의 클래스에 의해 접근이 가능하다.

## default
동일 패키지에서만 접근이 가능하다.

## private
해당 클래스에서만 접근이 가능하다.


### 정리

| 접근 제어자 | 모든 클래스 | 하위 클래스 | 동일 패키지 | 해당 클래스 |
|-------------|-------------|-------------|-------------|-------------|
| public      | o           | o           | o           | o           |
| protected   | x           | o           | o           | o           |
| default     | x           | x           | o           | o           |
| private     | x           | x           | x           | o           |

[마크다운 표 만들기](https://www.tablesgenerator.com/markdown_tables)