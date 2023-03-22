---
layout  : wiki
title   : image's platform does not match 에러
date    : 2023-03-22 20:55:00 +0900
updated : 2023-03-22 20:55:00 +0900
tag     : docker
toc     : true
public  : true
parent  : docker
latex   : false
---

* TOC
{:toc}

## 문제 상황
EC2에 도커를 설치하고 도커 이미지를 pull 받아온 후 컨테이너를 실행하기 위해 `docker run` 명령어를 실행했더니, 이미지의 플랫폼이 일치하지 않는다는 에러가 발생했다.
```bash
WARNING: The requested image's platform (linux/arm64/v8) does not match the detected host platform (linux/amd64) and no specific platform was requested
```

## 원인
EC2에서 호환되는 플랫폼은 linux/amd64인데, 이미지는 linux/arm64/v8 플랫폼으로 빌드 되어 발생한 에러다.

## 해결 방법
이미지 빌드 시 `--platform` 옵션을 추가하여 플랫폼을 설정해 준다.
```bash
docker build --platform linux/amd64
```

## 참고 문헌
[WARNING: The requested image's platform (linux/amd64) does not match the detected host platform (linux/arm64/v8)(stackoverflow)](https://stackoverflow.com/questions/72152446/warning-the-requested-images-platform-linux-amd64-does-not-match-the-detecte)
