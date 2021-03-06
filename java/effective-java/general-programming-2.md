---
title: 이펙티브자바 8장. 일반적인 프로그래밍 원칙들 - 2
date: 2017-10-09 18:56:00
tags:
- java
- effective java
desc: chapter 8. gerneral programming in effective java
---

[규칙 48](../../../../2017/10/09/general-programming-2/#48-정확한-답이-필요하다면-float와-double은-피하라) - 정확한 답이 필요하다면 float와 double은 피하라
[규칙 49](../../../../2017/10/09/general-programming-2/#49-객체화된-기본-자료형-대신-기본-자료형을-이용하라) - 객체화된 기본 자료형 대신 기본 자료형을 이용하라
[규칙 50](../../../../2017/10/09/general-programming-2/#50-다른-자료형이-적절하다면-문자열-사용은-피하라) - 다른 자료형이 적절하다면 문자열 사용은 피하라

<!-- more -->

<div class="tip">
    <div>아래 책를 참고하여 학습한 내용을 정리/기록한 포스트입니다. 자세한 내용은 책을 참고하시기 바라며, 문제가 있을 경우 연락 부탁드립니다.</div>
    <ul>
        <li>조슈아 블로크, 이병준(옮긴이), Effective Java, 2판, 인사이트, 2015.</li>
    </ul>
</div>

---

## 48. 정확한 답이 필요하다면 float와 double은 피하라

- `float`와 `double`은 이진 부동 소수점 연산(binary floating-point arithmetic)을 수행하는데, 이것은 넓은 범위의 값(magnitude)에서 대해 정확도가 높은 근사치를 제공할 수 있도록 세심하게 설계된 연산이다. 하지만 **정확한 결과를 제공하지 않기 때문에** 정확한(exact) 결과가 필요한 곳에는 사용하면 안 된다.
- **정확한 답을 요구하는 문제를 풀 때(e.g. 돈 계산)는 `BigDecimal`, `int` 또는 `long`을 사용하라.**
- `BigDecimal`: 소수점 이하 처리를 시스템에서 알아서 해 줬으면 하고, 기본 자료형보다 불편하고 조금 느려도 상관 없을 경우 사용하라.
- `int`: 관계된 수치들이 십진수 아홉 개 이하로 표현이 가능할 때 사용하라.
- `long`: 관계된 수치들이 십진수 18개 이하로 표현 가능할 때 사용하라. 그 이상일 경우 `BigDecimal`을 써야 한다.

---

## 49. 객체화된 기본 자료형 대신 기본 자료형을 이용하라

- 기본 자료형(`int`, `double`, `boolean`, ...)에 대응되는 참조 자료형(reference type)을 boxed primitive type(혹은 wrapper class)이라 부른다(e.g. `Integer`, `Double`, `Boolean`, ...).
- 기본 자료형이 더 단순하고 빠르다.
- 객체화된 기본 자료형에 == 연산자를 사용하는 것은 거의 항상 오류라고 봐야 한다.
- 기본 자료형과 객체화된 기본 자료형을 한 연산 안에 엮어 놓으면 객체화된 기본 자료형은 자동으로 기본 자료형으로 변환되며, 그 과정에서 NPE이 발생할 수 있다.
- autoboxing은 객체화된 기본 자료형을 사용할 때 생길 수 있는 문제들까지 없애주진 않는다.

---

## 50. 다른 자료형이 적절하다면 문자열 사용은 피하라

> 제대로 사용하지 못한 경우 문자열은 다른 자료형에 비해 다루기 성가시고, 유연성도 떨어지며, 느리고, 오류 발생 가능성도 높다.

#### 문자열은 값 자료형(value type)을 대신하기에는 부족하다.

- 숫자라면 int, 예/아니오를 묻는 질문의 답이라면 boolean으로 변환해야 한다. 즉, 적절한 값 자료형이 있다면 그것을 사용해야 한다. 
- 당연해 보이는 지침인데 지켜지지 않는 일이 많다.

#### 문자열은 enum 자료형을 대신하기에는 부족하다.

- 규칙 30에서 설명한 대로, enum은 문자열보다 훨씬 좋은 열거 자료형 상수를 만들어 낸다.

#### 문자열은 혼합 자료형(aggregate type)을 대신하기엔 부족하다.

- `String id = name + "#" + id;` 이러한 코드에는 많은 문제가 있다.
- 필드 구분자로 들어간 문자가 필드 안에 들어가면 문제가 생긴다.
- parsing하는데 시간 및 오류 발생 가능성도 높다.
- `equals`, `compareTo` 같은 메서드를 제공할 수 없고, String이 제공하는 기능들만 사용해야 한다.

#### 문자열은 권한(capability)을 표현하기엔 부족하다.

- 문자열을 사용해 기능 접근 권한을 표현하는 것은 어렵다.
- (참고) ThreadLocal 사용법과 활용: http://javacan.tistory.com/entry/ThreadLocalUsage