---
layout  : wiki
title   : EC2에서 실행중인 서버에 8080 포트로 접속할 수 없는 경우
date    : 2023-03-23 23:54:00 +0900
updated : 2023-03-23 23:54:00 +0900
tag     : aws
toc     : true
public  : true
parent  : aws
latex   : false
---

* TOC
{:toc}

## 문제 상황
EC2에 접속해서 도커 컨테이너를 실행했고, 헬스 체크까지 해서 서버가 정상적으로 실행되고 있다는 것을 확인했다.  
그런데 `EC2퍼블릭IP주소:8080`로 접속을 시도하니 접속이 거부되었다.

## 원인
EC2 인스턴스에서 8080 포트를 열어주지 않았기 때문에 8080 포트로 실행한 서버에 접근할 수 없어서 발생한 문제다.

## 해결 방법
`EC2 인스턴스의 Security Groups` > `Edit inbound rules` > `Add rule` > `Port range 8080으로 설정`
