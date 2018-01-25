---
layout: post
title: "컴파일러란"
date: 2018-01-25 19:48
category: posts
comments: false
tag:
- Compiler
---

#  C♭Compiler

- C♭는 한정된 process와 메모리를 사용하는 C언어의 Subset  
- C++의 현대적 기능과 구조를 사용 가능한 장점을 가진 언어  
- 특히 이 책에서는 X86 계 CPU에서 동작하는 Linux를 Target
    -  쉽게 구할 수 있는 Linux S/W
    - 쉽게 접할 수 있는 x86계 H/W
- 선행지식
    - C♭은 C언어를 바탕으로 하고, 어셈블러를 이해하기 위해서는 C 기본지식 필요
    - 컴파일러가 Java로 만들어지므로 compiler의 이해를 위해 Java 기초 지식 필요
    - Linux Shell의 기본 커맨드인 cd, ls, cp.mv에 대한 지식

# Compiler  란?
- 프로그래밍 된 소스 코드를 다른 형식으로 바꾸어주는 S/W
- Compiler로 소스 코드를 변환하는 과정을 가리켜 Compile이라 부름
- Ex) C언어의 GCC (GNU Compiler Collection), Java의 Javac
- 이 책에서는 한 개의 컴파일러만 설명하게 되며 실제 컴파일러를 만들며 얻은 지식은 다른 언어에서도 유용하게 사용될 것이다.

# C♭(C플랫) 컴파일러

- PC용 Linux에서도 동작이 가능하다.
- 가상 소프트웨어나 KNOPPIX등을 사용해 쉽게 리눅스 환경 구축 가능
- 실제로 컴파일 해서 실행해 보는 것이 학습을 돕는다
- C언어로는 컴파일러를 만들기 어렵고, 이해하는 것도 어렵다.
- C언어를 단순화하고 현대적인 기능을 넣어 편하게 만든 언어.

# 컴파일이란?

Hello.c와 같이 1개의 파일로 한 개의 실행파일을 만들 때는 gcc 명령 한 번으로 끝나지만 내부에서는 4단계를 거치게 된다.

- Preprocess
- (좁은 의미의) Compile
- Assemble
- Link

위의 작업 전체를 Compile이라 하는 경우도 있지만 엄밀하게는 2단계 좁은 의미의 컴파일만이 진짜 ‘compile’이다.   
4단계 전체를 합쳐 부를 때는 빌드(Build)라고 한다.


#어셈블
 어셈블리 언어란 기계어를 인간이 읽기 쉽게 텍스트 형식으로 변환한 언어
- 어셈블리 소스코드 확장자는 *.S
- 어셈블리 소스 소코드를 어셈블러에 의해 기계어로 변환시키는 과정을 **어셈블** 
- 이 출력 결과물을 Object file 이라 하며 확장자는 *.o

# 링크
오브젝트 파일을 최종적으로 사용할 수 있게 변환하는 작업
- 라이브러리를 결합시키는 과정이다.

### 빌드 과정
![buildProcess](https://raw.githubusercontent.com/HongJeSeong/compiler/master/image/buildProcess.PNG)

