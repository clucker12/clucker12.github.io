---
title: "[프로그래머스] 이진 변환 반복하기 (재귀, 문자열 처리)"
date: 2025-12-09
categories: [Algorithm, Programmers]
tags: [재귀, 문자열, 구현, Java]
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

# 🔢 프로그래머스 - 이진 변환 반복하기 (Java, 재귀)

## 📌 문제 설명

문자열 `s`에서 반복적으로 **0을 제거하고**,  
남은 1의 개수를 **이진수로 변환하는 작업을 반복하여**,  
최종적으로 문자열이 `"1"`이 될 때까지의

- 변환 횟수(`depth`)
- 제거된 0의 총 개수(`cnt`)

를 구하는 문제이다.

---

# 🧠 접근 방법 요약

### ✔ 핵심 아이디어

- 문자열에서 `0`을 제거하고 `1`만 남긴 뒤 길이를 이진수로 바꾸는 과정을 반복
- 이 작업을 문자열이 `"1"`이 될 때까지 시행
- 변환 횟수(`depth`)와, 제거된 0의 총 개수(`cnt`)를 구하는 문제
- 재귀 DFS를 활용해 구현하면 로직이 깔끔함

### ✔ 왜 재귀 DFS로 풀었는가?

반복 구조이지만,  
`현재 문자열 → 변환 → 다음 문자열`  
형태가 명확히 이어지는 구조이기 때문에  
재귀로 구현하면 가독성이 매우 좋아진다.

---

# 🧰 사용한 자료구조 & 변수

| 변수명          | 타입          | 설명                               |
| --------------- | ------------- | ---------------------------------- |
| `cnt`           | int           | 전체 과정에서 제거된 0의 누적 개수 |
| `depth`         | int           | 이진 변환이 수행된 횟수            |
| `StringBuilder` | StringBuilder | 문자열에서 0 제거 후 1만 유지      |

---

# 📘 전체 코드 (Java)

```java
package Programmers;

public class programmers20 {

    // 모든 호출에서 누적해서 사용할 전역 변수
    static int cnt;    // 제거된 0의 총 개수
    static int depth;  // 이진 변환 횟수

    public int[] solution(String s) {
        // 초기화
        cnt = 0;
        depth = 0;

        // 재귀 함수 호출
        dfs(s);

        // depth(변환 횟수), cnt(제거된 0의 총 합) 반환
        return new int[] { depth, cnt };
    }

    public void dfs(String s) {
        // 종료 조건 : 문자열이 "1"이면 더 이상 변환할 필요 없음
        if (s.equals("1")) {
            return;
        }

        // 이진 변환을 한 번 수행하므로 횟수 증가
        depth++;

        // 문자열에서 0 제거하고 1만 모을 StringBuilder
        StringBuilder sb = new StringBuilder();

        // 이번 변환에서 제거된 0 개수를 세기 위한 변수
        int count = 0;

        // 문자열을 순회하면서 0 제거 작업 수행
        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) != '0') {
                // 1이면 sb에 추가
                sb.append(s.charAt(i));
            } else {
                // 0이면 제거 ⇒ 카운트 증가만 함
                count++;
            }
        }

        // 전체 제거된 0 개수 누적
        cnt += count;

        // 제거 후 남은 1의 개수 → 문자열 길이
        int len = sb.length();

        // 길이를 2진법 문자열로 변환
        String next = Integer.toBinaryString(len);

        // 다음 단계 이진 변환 수행
        dfs(next);
    }
}
```

---

## 🔍 핵심 로직 정리

### ✔ 1. 종료 조건

문자열이 `"1"`이 되는 순간 변환 종료.

### ✔ 2. 0 제거 과정

- `StringBuilder`로 1만 모으고
- 제거된 0의 수를 `cnt`에 누적

### ✔ 3. 이진수 변환

남은 1의 개수 = 다음 변환 문자열을 만들기 위해  
`Integer.toBinaryString()` 사용

### ✔ 4. 재귀 DFS 호출

매 단계에서 새로운 문자열을 만들고 DFS 반복

---

## 📈 시간 복잡도 분석

각 변환에서 문자열 전체를 한 번 순회하므로 **O(N)**  
최대 변환 횟수는 문자열 길이에 따라 매우 적음  
➡️ 전체적으로 **O(N log N)** 수준으로 충분히 빠르다.

---

## 💡 느낀점

- 반복 로직을 재귀로 옮기면 코드가 훨씬 직관적
- 전역 변수 사용이 오히려 깔끔한 흐름을 만든다
- 문자열 길이를 이진수 변환하는 과정이 문제의 핵심 포인트

문자열 조작과 재귀 개념을 동시에 연습하기 좋은 문제였지만, 실제 구현 관점에서는 while문을 이용한 반복 구조가 더 바람직한 풀이였던 것 같다.
