---
title: "프로그래머스 더 맵게 문제 풀이 - PriorityQueue(힙) 활용"
categories: [코딩테스트]
tags: [힙, Java, 우선순위 큐, 그리디]
date: 2025-07-08 15:00:00 +0900
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

## 📘 문제 설명

모든 음식의 스코빌 지수를 K 이상으로 만들기 위해 가장 맵지 않은 두 개의 음식을 섞습니다.

- 스코빌 지수 계산 공식: `새로운 음식 = 가장 맵지 않은 음식 + (두 번째로 맵지 않은 음식 * 2)`
- 이 작업을 최소 횟수로 반복하여 모든 음식이 K 이상이 되도록 만듭니다.

불가능한 경우 -1을 반환합니다.

---

## 💡 핵심 아이디어

- 가장 작은 값 2개를 빠르게 꺼내기 위해 **힙(Heap)** 자료구조 사용
- 자바에서는 `PriorityQueue`를 통해 \*\*최소 힙(Min Heap)\*\*을 구현할 수 있음
- 모든 스코빌 지수가 K 이상이 될 때까지 반복

---

## 📄 자바 코드 풀이

```java
package Programmers;
import java.util.*;

public class programmers9 {
    public int solution(int[] scoville, int K) {
        int answer = 0; // 섞은 횟수를 저장할 변수
        PriorityQueue<Integer> pq = new PriorityQueue<>();
        // 최소 힙(우선순위 큐)을 사용하여 가장 작은 스코빌 지수를 빠르게 꺼내기 위함

        // 모든 스코빌 지수를 우선순위 큐에 추가
        for (int s : scoville) {
            pq.offer(s);
        }

        // 큐에 원소가 2개 이상이고, 가장 낮은 스코빌 지수가 K보다 작을 때 반복
        while (pq.size() > 1 && pq.peek() < K) {
            int fir = pq.poll(); // 가장 맵지 않은 음식 꺼냄
            int sec = pq.poll(); // 두 번째로 맵지 않은 음식 꺼냄
            int sum = fir + (sec * 2); // 두 음식을 섞어서 새로운 음식 생성
            pq.offer(sum); // 새로운 음식의 스코빌 지수를 큐에 추가
            answer += 1; // 섞은 횟수 증가
        }

        // 모든 음식의 스코빌 지수가 K 이상이면 섞은 횟수 반환, 그렇지 않으면 -1
        return pq.peek() >= K ? answer : -1;
    }
}

```

---

## 🧠 PriorityQueue (힙)의 특징

- 기본적으로 **Min Heap** 구조
- `peek()` : 가장 작은 원소 반환
- `poll()` : 가장 작은 원소 제거 및 반환
- `offer(x)` : 원소 x를 삽입하면서 자동으로 정렬 유지

자바 PriorityQueue는 내부적으로 \*\*이진 힙(Binary Heap)\*\*을 사용합니다. 삽입과 삭제 연산은 평균적으로 O(log n)의 시간 복잡도를 가집니다.

---

## ⏱️ 시간 복잡도

| 항목           | 설명                      |
| -------------- | ------------------------- |
| 삽입/삭제 연산 | O(log n) (힙의 연산 특성) |
| 전체 반복      | 최악의 경우 O(n log n)    |

---

## ✅ 풀이 요약

| 항목      | 설명                                        |
| --------- | ------------------------------------------- |
| 자료구조  | PriorityQueue (Java 힙 구현체)              |
| 반복 조건 | 큐에 2개 이상, 가장 작은 값 < K             |
| 섞는 방식 | 가장 작은 2개를 꺼내 새 음식 생성 후 재삽입 |
| 종료 조건 | 모든 음식 >= K 또는 불가능할 경우 -1 반환   |

---

## 🔚 결론

이 문제는 \*\*우선순위 큐(PriorityQueue)\*\*를 통해 효율적으로 해결할 수 있는 대표적인 그리디 문제입니다.

- 작은 값부터 선택해야 하므로 힙 자료구조가 적합
- 반복적으로 최소 두 개를 꺼내 조합하여 조건을 만족시키는 문제
- 알고리즘 및 자료구조(특히 힙)의 개념을 연습하기 좋은 문제입니다.
