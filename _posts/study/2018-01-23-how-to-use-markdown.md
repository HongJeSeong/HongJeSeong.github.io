---
layout: post
title: "마크다운(markdown) 작성법"
date: 2018-01-23
category: posts
comments: false
tag:
- markdown
---

# 마크다운(Markdown) 작성법
개발에서 필요한, 깃허브(github)를 사용하다 보면 마크다운이 필요하다.
깃허브에 자신의 레포지토리에 관한 설명이나, 문서를 작성할때 필요하게 된다.


## 마크다운의 장점
	* 간결함.
	* text로 용량이 적다
	* 변경이력 관리에 수월
	* 지원하는 플랫폼, 프로그램이 다양하다.


## 마크다운의 단점
	* 표준이 없다.
	* 모든 html 마크업을 대신하지 못한다.


## 헤더(Header)
html의	``` <h1>~<h6>``` 헤더와 같다.
```
# This is a H1
## This is a H2
### This is a H3
#### This is a H4
##### This is a H5
###### This is a H6
```
# This is a H1
## This is a H2
### This is a H3
#### This is a H4
##### This is a H5
###### This is a H6


## 리스트(List)
html의 ```<ol> <ul>``` 기능을 한다.
* 순서가 있는 리스트, 숫자와 점을 사용
```
1. 리스트1
2. 리스트2
3. 리스트3
```
1. 리스트1
2. 리스트2
3. 리스트3


* 순서 없는 리스트, *, +, - 를 사용
```
* 리스트1
  * 리스트2
    * 리스트3

+ 리스트1
  + 리스트2
    + 리스트3

- 리스트1
- 리스트2
- 리스트3
```
* 리스트1
  * 리스트2
    * 리스트3

+ 리스트1
  + 리스트2
    + 리스트3

- 리스트1
  - 리스트2
    - 리스트3

## 단락(Paragraph)
html ```<p></p>``` 태그로 그냥 텍스트를 작성하면 된다.
``` 글 본문입니다 ```
글 본문입니다


## 수평선(Horizontal Rules)
html의 ```</hr>```  태그의 기능을 한다
```
* * *

***

*****

- - -

---------------------------------------
```
* * *
***
*****
- - -

-----------------------------------


## 링크(Link)
html의 하이퍼링크의 기능을 한다. title생략 가능
```
[example](http://example.com "title")
클릭하세요 [구글](https://www.google.com "구글")
```
[example](http://example.com "title")
클릭하세요 [구글](https://www.google.com "구글")
