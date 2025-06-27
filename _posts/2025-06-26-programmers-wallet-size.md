---
title: "프로그래머스 최소 지갑 크기 문제"
categories: [코딩테스트]
tags: [Java, 완전 탐색]
toc: true
date: 2025-06-26 10:00:00 +0900
layout: single
author_profile: true
read_time: true
share: true
show_tags: true
show_categories: true
sidebar:
  nav: "counts"
---

## 📘 문제 설명

프로그래머스 **"최소 지갑 크기"** 문제는 모든 명함을 수납할 수 있는 지갑의 최소 크기를 구하는 문제입니다.  
각 명함은 가로, 세로가 주어지고 **회전이 가능**합니다.  
즉, 명함을 세로 ↔ 가로로 돌릴 수 있다는 점을 고려해야 합니다.

---

## 💡 풀이 아이디어

- 각 명함을 가로 ≥ 세로 형태로 회전시킵니다.
- 모든 명함 중 **가로 길이의 최대값** × **세로 길이의 최대값**을 구하면 지갑의 최소 크기입니다.

---

## 📄 자바 풀이 코드

```java
package Programmers;

public class programmers1 {
    class Solution {
        public int solution(int[][] sizes) {
            int maxw = 0; // 최대 가로 길이
            int maxh = 0; // 최대 세로 길이

            for (int i = 0; i < sizes.length; i++) {
                int w = sizes[i][0];
                int h = sizes[i][1];

                // 가로가 항상 더 크도록 회전
                if (w < h) {
                    sizes[i][0] = h;
                    sizes[i][1] = w;
                }

                // 최대값 갱신
                maxw = Math.max(maxw, sizes[i][0]);
                maxh = Math.max(maxh, sizes[i][1]);
            }

            return maxw * maxh;
        }
    }
}
```

## ✏️ 정리 요약

1️⃣ 모든 명함을 가로 ≥ 세로 형태로 회전 <br>
2️⃣ 회전된 명함 중 가장 긴 가로 길이와 가장 긴 세로 길이 구함<br>
3️⃣ 최소 지갑 크기 = 최대 가로 \* 최대 세로<br>
4️⃣ 시간 복잡도: O(N)<br>
  → 명함 수만큼 한 번씩 순회하며 최대값만 계산하므로 선형 시간입니다.<br>

## ✅ 예시

```
입력: [[60, 50], [30, 70], [60, 30], [80, 40]]
회전 후: [[60, 50], [70, 30], [60, 30], [80, 40]]

최대 가로 = 80
최대 세로 = 50

정답: 80 * 50 = 4000
```
