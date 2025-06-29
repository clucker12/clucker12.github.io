---
title: "Java 생성자, 상속, 필드 숨김 개념 정리 예제"
categories: [Java]
tags: [생성자, 상속, 필드숨김, 정적바인딩, 면접준비]
date: 2025-07-01 10:00:00 +0900
toc: true
layout: single
author_profile: true
read_time: true
share: true
show_tags: true
show_categories: true
toc: true
sidebar:
  nav: "counts"
---

# 🔍 핵심 개념 요약

| 항목        | 설명                                            |
| ----------- | ----------------------------------------------- |
| 생성자 순서 | 항상 부모 → 자식 순으로 실행됨                  |
| `super()`   | 생략 시 자동으로 부모 클래스의 기본 생성자 호출 |
| 필드 접근   | **참조 변수의 타입** 기준으로 정적 바인딩       |
| 메서드 호출 | **객체의 타입** 기준으로 동적 바인딩            |
| 출력 결과   | `가다가라30`                                    |

---

# 📌 예제 코드

```
class A {
    int a = 10;

    public A() {
        System.out.print("가");
    }

    public A(int x) {
        System.out.print("나");
    }
}

class B extends A {
    int a = 20;

    public B() {
        System.out.print("다");
    }

    public B(int x) {
        System.out.print("라");
    }
}

public class Main {
    public static void main(String[] args) {
        B b1 = new B();        // 기본 생성자 호출
        A b2 = new B(1);       // A 타입 참조 → B 객체 생성
        System.out.print(b1.a + b2.a);  // 필드 접근
    }
}
```

# 🧾 실행 결과

```
가다가라30
```

# ✅ 해설

## ✅ 1. new B()

B 클래스의 기본 생성자 호출

내부적으로 super() 호출 → A 클래스 기본 생성자 호출

출력 순서:

A() → "가"

B() → "다"

## ✅ 2. new B(1)

B 클래스의 매개변수 생성자 호출

내부적으로 super() 호출 → A 클래스 기본 생성자 호출

출력 순서:

A() → "가"

B(int) → "라"

# ✅ 3. System.out.print(b1.a + b2.a);

b1.a
→ 객체 타입: B, 참조 타입: B
→ B 클래스의 a = 20

b2.a
→ 객체 타입: B, 참조 타입: A
→ A 클래스의 a = 10 (정적 바인딩)

계산 결과: 20 + 10 = 30

# 🎯 핵심 개념 요약

필드(a)는 정적 바인딩 → 참조 변수의 타입에 따라 결정됨.

메서드는 동적 바인딩 → 실제 객체 타입 기준.

super()는 생략해도 부모의 기본 생성자가 자동 호출됨.

상속 구조에서 생성자 호출 순서는 항상 부모 → 자식 순서.
