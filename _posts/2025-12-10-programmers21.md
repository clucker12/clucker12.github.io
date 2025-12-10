---
title: "[프로그래머스] 짝지어 제거하기 (Stack)"
date: 2025-12-10
categories: [Algorithm, Programmers]
tags: [스택, 문자열, 구현, Java]
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

# 🧹 프로그래머스 - 짝지어 제거하기 (Java, Stack)

## 📌 문제 설명

문자열 `s`가 주어졌을 때,  
**같은 알파벳이 연속으로 붙어 있는 경우 쌍으로 제거**하는 작업을 반복하여  
모든 문자를 제거할 수 있으면 `1`,  
남아 있으면 `0`을 반환하는 문제이다.

이 문제는 **Stack**을 활용하면 매우 간단하게 해결할 수 있다.

---

# 🧠 접근 방법 요약

### ✔ 핵심 아이디어

- 스택의 top 과 현재 문자를 비교
- 같다면 → 짝이므로 제거(pop)
- 다르면 → 스택에 push
- 마지막에 스택이 비어있으면 성공

### ✔ 왜 Stack인가?

스택은 **가장 최근에 들어간 문자(바로 앞 문자)** 와 쉽게 비교할 수 있기 때문에  
연속된 쌍 제거 문제와 매우 잘 맞는다.

---

# 🧰 사용한 자료구조

| 자료구조 | 용도 |
|---------|------|
| `Stack<Character>` | 문자 쌍 비교 및 제거 |

---

# 📘 전체 코드 (Java)

```java
package Programmers;

import java.util.Stack;

public class programmers21 {
    public int solution(String s)
    {
        // 문자 비교를 위한 스택 생성
        Stack<Character> stack = new Stack<>();

        // 문자열을 처음부터 끝까지 반복
        for (char c : s.toCharArray()) {

            // 스택이 비어있지 않고,
            // 스택의 top(마지막 문자)이 현재 문자와 같으면 → 짝이므로 제거
            if (!stack.isEmpty() && stack.peek() == c) {
                stack.pop();      // 짝을 제거
            } else {
                stack.push(c);    // 짝이 아니면 스택에 넣음
            }
        }

        // 스택이 비어있으면 모든 문자 짝지어 제거 완료 → 1
        // 스택에 문자가 남아 있으면 제거 실패 → 0
        return stack.isEmpty() ? 1 : 0;
    }
}
```

---

## 🔍 핵심 로직 설명

✔ `stack.peek() == c`  
→ 바로 이전 문자와 현재 문자가 같은지 확인

✔ 같으면 `pop()`  
→ 쌍이 제거됨

✔ 다르면 `push()`  
→ 다음 문자와 비교해야 하므로 스택에 쌓음

---

## 📈 시간 복잡도

문자열 길이를 N이라고 할 때

- 각 문자는 스택에 push 또는 pop → **O(1)**
- 전체 문자열 순회 → **O(N)**

총 시간 복잡도: **O(N)**

---

## 💡 느낀점

스택의 기본 개념만 이해하면 정말 간단하게 풀리는 문제였다.

- 최근 값과 비교할 때 스택이 강력하다는 점을 다시 확인
- 문자열 처리 문제에서 스택이 자주 등장하는 이유를 이해할 수 있음
- 쌍 제거 문제는 대부분 스택으로 풀면 깔끔하게 해결됨

자료구조 기초를 복습하기에도 아주 좋은 문제였다.
