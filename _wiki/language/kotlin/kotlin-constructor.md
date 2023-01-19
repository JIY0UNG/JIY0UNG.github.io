---
layout  : wiki
title   : Kotlin 생성자
date    : 2023-01-16 22:14:00 +0900
updated : 2023-01-20 02:03:00 +0900
tag     : kotlin
toc     : true
public  : true
parent  : kotlin
latex   : false
---

* TOC
{:toc}

코틀린의 클래스는 주 생성자(Primary constructor)와 한 개 이상의 부 생성자(Secondary constructor)를 가질 수 있다.

## 주 생성자(Primary Constructor)
클래스 헤더의 일부로, 클래스명과 Optional 파라미터(ex.`<T>`) 뒤에 `constructor`키워드와 함께 정의한다.
```kotlin
class Person<T> constructor(firstName: String)
```

주 생성자에 어노테이션이나 접근 제어자[^visibility-modifier]가 없는 경우에는 `constructor` 키워드를 생략할 수 있다.
```kotlin
class Person(firstName: String)
```

주 생성자에는 코드를 쓸 수 없어서, `init` 키워드가 붙어있는 초기화 블록 내에서 초기화할 수 있다. 이때 초기화 블록은 여러 개를 따로 배치할 수 있으며, 아래와 같은 순서로 실행된다.
```kotlin
class Person(
    name: String,
    out: Unit = println("Primary Constructor") // 1
) {
    val firstProperty = "First property".also(::println) // 2

    init {
        println("First initializer") // 3
    }

    val secondProperty = "Second property".also(::println) // 4

    init {
        println("Second initializer") // 5
    }
}
```

```bash
// 실행 결과
Primary Constructor
First property
First initializer
Second property
Second initializer
```

주 생성자의 매개 변수는 초기화 블록에서 사용할 수 있다.
```kotlin
class Person(
    name: String,
) {
    init {
        println("$name")
    }
}
```

클래스 본문에 선언된 속성 초기화에도 사용할 수 있다.
```kotlin
class Person(
    firstName: String,
    secondName: String,
) {
    val fullName = firstName + secondName
}
```

주 생성자 내에서 프로퍼티 선언과 함께 초기화를 할 수 있다. 이때 후행 쉼표를 쓴다.[^trailing-commas]  
일반 프로퍼티와 마찬가지로 주 생성자에 선언된 프로퍼티는 가변(var)이나 불변(val)이다.
```kotlin
class Person(
    val firstName: String = "",
    var secondName: String = "",
)
```

생성자에 어노테이션이나 접근 제어자가 있는 경우 `constructor`키워드를 필수로 붙여줘야 하고, 접근 제어자는 그 앞에 위치한다.
```kotlin
class Customer public @Inject constructor(name: String)
```

## 링크
[Constructors](https://kotlinlang.org/docs/classes.html#constructors)

## 주석
[^visibility-modifier]: Kotlin Docs에서는 ['visibility modifier(가시성 수정자)'](https://kotlinlang.org/docs/visibility-modifiers.html#constructors)라는 단어를 사용했지만, 나한테는 '접근 제어자'가 더 익숙한 표현이라서 이렇게 표기했다.

[^trailing-commas]: Kotlin 스타일 가이드에서 권장하는 스타일. [Trailing commas](https://kotlinlang.org/docs/coding-conventions.html#trailing-commas)
