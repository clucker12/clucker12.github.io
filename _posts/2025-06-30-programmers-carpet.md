---
title: "프로그래머스 카펫 문제"
categories: [코딩테스트]
tags: [완전탐색, 구현, Java]
date: 2025-06-30 10:00:00 +0900
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

## 🧩 문제 설명

갈색 타일과 노란색 타일이 조합된 카펫의 크기를 추측하는 문제입니다.  
가장 바깥 테두리는 **갈색 타일**, 내부는 **노란색 타일**로 구성되어 있으며,  
이때 가능한 카펫의 가로, 세로 크기를 구하는 문제입니다.

- 갈색 타일 개수: `brown`
- 노란색 타일 개수: `yellow`
- 반환값: `[가로, 세로]` 형태의 정수 배열

---

## 💡 풀이 아이디어

- 전체 타일 수 = `brown + yellow`
- 가능한 세로 길이(height)를 3 이상부터 순회하며,
- `(전체 / 세로)`를 이용해 가로(width)를 구함
- `(width - 2) * (height - 2)`가 `yellow`와 같으면 정답!

---

## 📄 자바 풀이 코드

```java
package Programmers;

public class programmers4 {
    class Solution {
        public int[] solution(int brown, int yellow) {
            // 카펫의 전체 타일 수는 갈색 타일과 노란색 타일의 합
            int total = brown + yellow;

            // height는 최소 3부터 시작 (테두리를 만들기 위해 최소한의 높이 필요)
            // total / 3까지 반복 -> width >= height 이라는 조건에서 충분
            for (int height = 3; height <= total / 3; height++) {
                // 전체 타일 수가 해당 높이로 나누어떨어지지 않으면 해당 높이는 불가능
                if (total % height != 0) continue;

                // 가능한 너비 계산 (전체 타일 수 / 현재 높이)
                int width = total / height;

                // 내부의 노란색 타일 개수 확인
                // 갈색 테두리를 제외한 내부 면적이 노란색 타일 수와 같아야 함
                if ((width - 2) * (height - 2) == yellow) {
                    // 조건이 맞으면 [너비, 높이] 형태로 반환
                    return new int[]{width, height};
                }
            }

            // 조건을 만족하는 경우가 없으면 null 반환 (문제 조건상 이런 경우는 없음)
            return null;
        }
    }
}

```

## ⏱️ 시간 복잡도

가능한 세로 길이를 total/3까지 순회하므로 O(n) <br>
(실제로는 입력 크기가 작아 완전탐색으로 충분)

## ✅ 정리

테두리는 항상 갈색, 내부는 노란색으로 구성됨.

(width - 2) \* (height - 2) == yellow 를 만족하는 조합을 찾는 것이 핵심.

조건을 만족하면 [width, height] 형태로 반환.

완전탐색이지만 범위가 작아 효율적인 구현이 가능함.
