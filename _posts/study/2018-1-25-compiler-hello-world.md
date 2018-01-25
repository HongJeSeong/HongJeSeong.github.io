---
layout: post
title: "컴파일러 hello world"
date: 2018-01-25
category: posts
comments: false
tag:
- Linux
- Compiler
---

# 환경 설정과 hello world 찍기

![컴파일러 책(나뭇잎)](https://raw.githubusercontent.com/HongJeSeong/compiler/master/image/bookCover.PNG)  
2010년에 한글 번역본으로 나온 책이다.  
참고할 자료가 많지 않고, 지금 버전과는 달라 hello world 를 출력하는데 전혀 쉽지가 않았다.  
_결국 성공하였다._  


## 1. 세팅
- 환경 : 우분투~~16.04(64bit)~~, 10.04(~~64bit~~, **32bit**), **11.10(32bit)** 에서 성공
- [한빛 미디어 자료실](http://dw.hanbit.co.kr/exam/1768/) 에서 cbc-1.0.tar.gz 다운로드

_조사 결과  32비트 환경에서 돌아가는 코드로 되어 있다함._


 [64비트 환경] (https://github.com/leungwensen/cbc-ubuntu-64bit)

----------------------------------------------
* 모든 작업은 **sudo** 권한으로 진행한다
* 우분투 버전이 높다면 쉽게  ``` apt-get install ``` 로 아래와 같이 컴파일에 필요한 것들을 설치해준다.   그러나 버전이 낮으면 레포지토리가 기존과 다르거나 , 등록이 안되어 있을 수 있다 .  
 (구 버전은 old-release 한곳에 몰아 놨나?)  
 따라서 구버전들은 주소들을 ``` old-release ``` 로 바꾼 후 ```apt-get install``` 을 사용해 설치한다.


``` vi /ete/apt/source.list ``` 으로  편집기를 연다.


다음은 구버전들의 주소이다
```
deb http://kr.archive.ubuntu.com/ubuntu/ lucid 어쩌구~
deb-src http://kr.archive.ubuntu.com/ubuntu/ 어쩌구~~
.
.
.
deb http://security.ubuntu.com/ubuntu 어쩌구~~
```
이들 주소를 http://old-releases.ubuntu.com/ubuntu 어쩌구~~

kr 우분투를 old-release 우분투로 바꾸기.


변환, 저장 후에 **apt-get update** 를 해준다.



### Requirements

아래 필요한 것을 ```apt-get install```  설치

----------------------------------------------


**To compile cbc itself:**

        * JDK 1.5 or later
        * JavaCC 4.0 or later
        * ant
        * make

**To run cbc and compiled program:**

        * Linux 2.4 or later
        * util-linux (ld-linux.so.2)
        * GNU libc 2.3 or later
        * GNU binutils (as, ld)


### Installation

- 다운로드한 cbc-1.0.tar.gz를  /usr/local/에 저장
- ```cd /usr/local/``` 경로 이동
- ```tar xzvf cbc-1.0.tar.gz``` 를 하여 압축 풀기
- ```cd cbc-1.0``` 경로이동

----------------------------------------------

**To install all files under /usr/local/cbc:**

        # sudo ./install.sh
        # sudo ln -s ../cbc/bin/cbc /usr/local/bin/cbc

**To install all files under $HOME/cbc:**

        $ ./install.sh $HOME/cbc
        $ ln -s ../cbc/bin/cbc $HOME/bin/cbc


### Build

나는 건들지 않았다.

----------------------------------------------

**Edit build.properties for your environment and invoke make:**

        $ vi build.properties
        $ make


### Test

```make test```를 진행한다.
![make test](https://raw.githubusercontent.com/HongJeSeong/compiler/master/image/makeTest.PNG)  실행을 하다가 fail.. 아무리 코드를 파해치며 경로를 수정해도 중간에 실패함..  그냥 넘어가지만 나중에 걸림돌이 될까 걱정됨  
  
----------------------------------------------

    Invoke "make test":

        $ make test

    Note that you need bash (not bourne sh) to run test scripts.
    ksh or zsh may work.


### Usage

```cbc --help``` 로 cbc에 관한 도움 글이 출력 된다면 cbc 명령어 호출에는 성공


    $ cbc --help


## 2. 실행


cbc 명령어를 사용하게 되었으니 ```test``` 폴어데 있는 ```hello.cb```를 실행.

그러나 에러를 뱉어버림..;  

![error1](https://raw.githubusercontent.com/HongJeSeong/compiler/master/image/compileError.PNG)  
- /usr/lib/crt* 들이 없다고 하는 것을 확인
  - 그러나 /usr/lib/i386-linux-gnu/ 에 잘 있는 것을 확인
  - ![find](https://raw.githubusercontent.com/HongJeSeong/compiler/master/image/findCrt.PNG)
  - 링킹으로 crt* 들을 연결해도 안됨.
  - **그냥 ```mv```로 파일 들을 ```/usr/lib/``` 으로 옮겨주니 인식함**

## 3. 결과

```cbc test/hello.cb``` 를 하니 hello.o hello.s hello를 생성  
![res1](https://raw.githubusercontent.com/HongJeSeong/compiler/master/image/res1.PNG)  

```./hello``` 를 실행하면   
![res2](https://raw.githubusercontent.com/HongJeSeong/compiler/master/image/res2.PNG)  
    Hello, World! 출력

