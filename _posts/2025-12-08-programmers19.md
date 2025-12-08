---
title: "[프로그래머스] JadenCase 문자열 만들기 (문자열 처리)"
date: 2025-12-08
categories: [Algorithm, Programmers]
tags: [문자열, 구현, Java]
layout: single
author_profile: true
read_time: true
share: true
show_tags: true
show_categories: true
toc: true
sidebar:
  nav: "c"
---

# 📝 문제 풀이: JadenCase 문자열 만들기

프로그래머스의 문자열 구현 문제로, 문자열의 각 단어 첫 글자는 대문자로,
나머지는 소문자로 변환하는 **JadenCase** 스타일을 만드는 문제입니다.

---

## 📌 접근 방법

- 문자열을 처음부터 끝까지 순회하며 단어의 첫 글자인지 아닌지를 판단.
- 공백이 여러 개 연속해서 나올 수 있으므로 단순 split 사용하지 않고
  문자 단위 처리.
- `isStart` 플래그로 현재 문자가 단어의 시작인지 파악.

---

## 📂 전체 코드

```java
package Programmers;

public class programmers19 {
    public String solution(String s) {
        StringBuilder sb = new StringBuilder();
        boolean isStart = true;  // 단어의 첫 글자인지 여부

        for (char c : s.toCharArray()) {

            if (c == ' ') { // 공백이면
                sb.append(c);
                isStart = true; // 다음 문자는 단어의 첫 글자
            } else {
                if (isStart) { // 단어 첫 글자
                    sb.append(Character.toUpperCase(c));
                    isStart = false;
                } else { // 단어 첫 글자 아닐 때
                    sb.append(Character.toLowerCase(c));
                }
            }
        }

        return sb.toString();
    }
}
```

---

## 🔍 핵심 로직 & 자료구조 설명

### ✔ StringBuilder 사용

- 문자열을 반복적으로 이어 붙여야 하므로 `StringBuilder` 사용 → 효율적
  O(n)

### ✔ 단어 첫 글자 판별 `isStart`

- 공백을 만나면 다음 문자는 무조건 단어 시작.
- 문자가 공백이 아니라면:
  - 첫 글자면 대문자로 변환
  - 아니면 소문자로 변환

### ✔ Char 배열 순회

- `s.toCharArray()`로 한 글자씩 처리 → split 대비 공백 문제 해결

---

## ⏱ 시간 복잡도

- 문자열을 한 번 순회하므로 **O(n)**\
- n은 문자열 길이

---

## 💡 느낀 점

처음엔 split으로 단어 단위 처리하려 했지만, 공백이 여러 번 등장하는 경우
원본 문자열 형태를 그대로 유지하기 어렵다는 문제를 깨달았다.\
문자 단위로 직접 순회하는 방식은 조금 번거롭지만, **입력 문자열을 그대로
보존하며 원하는 형태로 가공해야 하는 문제**에서 매우 유용하다는 점을
다시 느꼈다.

JadenCase 문제는 단순한 문자열 문제 같지만, "공백 처리"를 제대로 하지
않으면 틀리기 쉬운 함정이 있는 문제였다.

---

## ✅ 결론

- 문자열 구현에서 가장 중요한 점 중 하나는 "원본 문자열의 구조를
  유지하는 것".
- 공백이 연속 등장하는 입력이 있는 경우 split 기반 처리보다 **문자
  단위 처리 방식이 안정적**이다.
