---
layout: post
title: "우분투(이클립스+톰캣 연동)"
date: 2019-02-08 16:34
category: posts
comments: false
tag:
- Linux
---

# 우분투(이클립스+톰캣 연동)

우분투환경에서 이클립스와 톰캣을 사용하여 개발하게 되어 정리했다.  

우분투에서 이클립스를 사용하여 개발하기는 윈도우 환경에서 개발하는 것보다 많이 불편하였다.

톰캣8과 이클립스가 설치되어 있는 환경이다.  
`톰캣`을 아파치 톰캣 페이지에서 `tar.gz로 다운`받아 압축풀어 사용하면 **설정파일들과 실행파일 등이 같이 묶여있어** 이클립스에서 연결할때 편하지만, 서버에 `apt-get을 사용`하여 톰캣을 설치하면
톰캣의 홈 디렉토리는 `/usr/share/tomcat8`  
톰캣의 설정 디렉토리는 `/var/lib/tomact8/conf`  
이런식으로 흩어져 설치되기 때문에 **이클립스에서 바로 연동하려하면 conf 파일에 접근이 불가능** 하다고 에러가 난다.  
따라서 흩어져 있는 파일들을 링킹을 해주어 한 폴더에서 접근 가능 하도록 만들어 주어야한다.  

```sh
cd /usr/share/tomcat8 #톰캣 폴더로 이동

#링킹하기
ln -s /var/lib/tomcat8/conf conf
ln -s /etc/tomcat8/policy.d/03catalina.policy conf/catalina.policy
ln -s /var/log/tomcat8 log
```
설정 파일에 접근에 `permission 에러`를 뱉는 것을 없애기 위하여 이클립스에서 링크된 파일들에 접근이 가능하도록 하기 위하여 **접근권한을 준다.** 

```sh
chmod -R 777 /usr/share/tomcat8/conf
```
