---
title: "프로그래머스 모의고사 문제"
categories: [코딩테스트]
tags: [완전탐색, Java, 구현]
date: 2025-06-27 10:00:00 +0900
layout: single
author_profile: true
read_time: true
share: true
show_tags: true
show_categories: true
sidebar:
  nav: "counts"
---

## 🧩 문제 설명

수포자 세 명이 각각 다른 방식으로 정답을 찍습니다. 실제 정답과 비교하여 가장 많은 정답을 맞힌 수포자의 번호를 **오름차순 배열**로 반환하는 문제입니다.  
(프로그래머스 완전탐색 문제 - Level 1)

---

## 💡 풀이 아이디어

- 각 수포자의 답안 패턴을 만들어 `answers` 배열과 비교합니다.
- 각 수포자가 몇 개의 문제를 맞혔는지 세어 `최고 점수`를 구합니다.
- 최고 점수를 받은 수포자의 번호를 리스트에 추가해 반환합니다.

---

## 📄 자바 풀이 코드

```java
package Programmers;

import java.util.*;

public class programmers2 {

    class Solution {
        public int[] solution(int[] answers) {
            int cnt = answers.length;

            int[] a = new int[cnt]; // 수포자 1
            int[] b = new int[cnt]; // 수포자 2
            int[] c = new int[cnt]; // 수포자 3

            // 수포자 1 패턴: 1, 2, 3, 4, 5 반복
            for (int i = 0; i < cnt; i++) {
                a[i] = (i % 5) + 1;
            }

            // 수포자 2 패턴
            int[] bPattern = {2, 1, 2, 3, 2, 4, 2, 5};
            for (int i = 0; i < cnt; i++) {
                b[i] = bPattern[i % bPattern.length];
            }

            // 수포자 3 패턴
            int[] cPattern = {3, 3, 1, 1, 2, 2, 4, 4, 5, 5};
            for (int i = 0; i < cnt; i++) {
                c[i] = cPattern[i % cPattern.length];
            }

            int cnt_a = 0, cnt_b = 0, cnt_c = 0;

            // 정답 비교
            for (int i = 0; i < cnt; i++) {
                if (answers[i] == a[i]) cnt_a++;
                if (answers[i] == b[i]) cnt_b++;
                if (answers[i] == c[i]) cnt_c++;
            }

            int max = Math.max(cnt_a, Math.max(cnt_b, cnt_c));

            ArrayList<Integer> list = new ArrayList<>();
            if (cnt_a == max) list.add(1);
            if (cnt_b == max) list.add(2);
            if (cnt_c == max) list.add(3);

            int[] answer = new int[list.size()];
            for (int i = 0; i < list.size(); i++) {
                answer[i] = list.get(i);
            }

            return answer;
        }
    }

}
```

## ⏱️ 시간 복잡도

- 패턴 비교: O(n) <br>
  n = answers.length일 때, 각 수포자와의 비교는 1회 순회로 끝나므로 선형 시간입니다.

- 총 시간 복잡도: O(n)

## ✅ 정리

- 각 수포자의 패턴은 배열로 직접 정의해두고 순환하며 채점합니다.
- 가장 많은 문제를 맞힌 사람을 모두 결과에 담기 위해 ArrayList 사용 후 배열로 변환합니다.
- 조건에 따라 패턴 비교 + 최댓값 계산으로 효율적인 방식입니다.
