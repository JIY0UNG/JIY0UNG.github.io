---
layout  : wiki
title   : IntelliJ IDEA
date    : 2023-02-05 16:27:00 +0900
updated : 2023-04-08 16:18:00 +0900
tag     : tools
toc     : true
public  : true
parent  : tools
latex   : false
---

* TOC
{:toc}

## Reader mode 폰트 크기 변경하기
`Reader mode에서 표시되는 docs 위에서 우클릭` > `Adjust Font Size...` > `나타나는 슬라이더에서 적절한 크기로 조절`  

## 스니펫 만들기
`Settings..` > `Live Templates` > 언어 선택 후 + 버튼 클릭 > `Abbreviation`: 단축어, `Description`: 스니펫 설명, `Template text`: 스니펫으로 만들 코드

## static method import 시 자동완성 코드가 단어 사이에 생성되지 않도록 하기

import할 메서드를 선택한 후 enter를 누르면 `assertArrayEquals();Equals();` 이런 식으로 단어 사이에 자동완성된 코드가 끼어들어서 항상 일일이 다 지워주느라 짜증 났었다.

이렇게 되지 않도록 하려면 메서드 선택 후 tab을 누르면 된다.
