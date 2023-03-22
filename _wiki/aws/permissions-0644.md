---
layout  : wiki
title   : Permissions 0644 for 'xxx.pem' are too open 에러
date    : 2023-03-22 18:45:00 +0900
updated : 2023-03-22 18:45:00 +0900
tag     : aws
toc     : true
public  : true
parent  : aws
latex   : false
---

* TOC
{:toc}

## 문제 상황
터미널에서 AWS EC2에 접속하기 위해 `ssh -i "xxx.pem" ec2-user@퍼블릭DNS주소` 명령어를 입력했더니 권한이 없다는 에러가 발생했다.
```bash
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@         WARNING: UNPROTECTED PRIVATE KEY FILE!          @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
Permissions 0644 for 'xxx.pem' are too open.
It is required that your private key files are NOT accessible by others.
This private key will be ignored.
Load key "xxx.pem": bad permissions
ec2-user@퍼블릭DNS주소: Permission denied (publickey,gssapi-keyex,gssapi-with-mic).
```

## 원인
> For SSH connections, you must set the permissions so that only you can read the file.

SSH 연결 시 오로지 소유자만 키 파일을 읽을 수 있도록 권한 설정을 해줘야 하는데, 권한 설정을 하지 않아서 발생한 에러이다.  
현재 키 파일의 권한 정보를 보니 `-rw-r--r--`로 설정되어 있었다.

## 해결 방법
chmod 명령어를 실행하여 권한을 `-r--------`로 축소시킨다.
```bash
chmod 400 xxx.pem
```

## 참고 문헌
[Locate the private key and set the permissions(AWS Documentation)](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/connection-prereqs.html#connection-prereqs-private-key)