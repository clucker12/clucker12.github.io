---
title: "프로그래머스 피로도 문제 - DFS 완전탐색"
categories: [코딩테스트]
tags: [DFS, 완전탐색, Java]
date: 2025-07-01 10:00:00 +0900
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

당신은 피로도 `k`를 가지고 여러 개의 던전을 탐험해야 합니다.  
각 던전은 `최소 필요 피로도`와 `소모 피로도`를 가집니다.  
던전의 순서를 임의로 정할 수 있을 때, 최대 몇 개의 던전을 탐험할 수 있을까요?

---

## 💡 풀이 핵심

- **DFS(깊이 우선 탐색)**을 사용해 가능한 모든 탐험 순서를 탐색
- 방문 여부를 기록하는 `visited[]` 배열을 활용
- 피로도가 조건을 만족하는 던전만 탐험
- **백트래킹**을 통해 모든 경우의 수를 고려
- 탐험 가능한 던전 수의 최대값을 `answer`로 저장

---

## 📄 자바 풀이 코드

```java
package Programmers;

public class programmers5 {
    class Solution {
        private boolean[] visited;
        private int answer = 0;

        public int solution(int k, int[][] dungeons) {
            visited = new boolean[dungeons.length];
            dfs(0, k, dungeons);
            return answer;
        }

        private void dfs(int depth, int k, int[][] dungeons) {
            answer = Math.max(answer, depth);

            for (int i = 0; i < dungeons.length; i++) {
                if (!visited[i] && k >= dungeons[i][0]) {
                    visited[i] = true;
                    dfs(depth + 1, k - dungeons[i][1], dungeons);
                    visited[i] = false; // 백트래킹
                }
            }
        }
    }
}
```

## 🌳 DFS 탐색 트리 예시

초기 피로도 k = 80, 던전 정보:

```
0: [80, 20]
1: [50, 40]
2: [30, 10]
```

DFS 호출 흐름:

```
dfs(0, 80)
├─ 탐험 0 → dfs(1, 60)
│   ├─ 탐험 1 → dfs(2, 20)
│   └─ 탐험 2 → dfs(2, 50)
│       └─ 탐험 1 → dfs(3, 10)
├─ 탐험 1 → dfs(1, 40)
│   └─ 탐험 2 → dfs(2, 30)
├─ 탐험 2 → dfs(1, 70)
    └─ 탐험 1 → dfs(2, 30)
```

## ✅ 가능한 경로 요약

| 경로          | 탐험 수  |
| ------------- | -------- |
| 0 → 1         | 2        |
| **0 → 2 → 1** | **3 ✅** |
| 1 → 2         | 2        |
| 2 → 1         | 2        |

최대 탐험 가능 던전 수는 3개입니다.

## 📌 핵심 요약

| 항목        | 설명                                             |
| ----------- | ------------------------------------------------ |
| DFS         | 가능한 던전 조합을 재귀로 모두 탐색              |
| `visited[]` | 각 던전의 방문 여부를 확인하는 배열              |
| 백트래킹    | 탐험 후 이전 상태로 복원하여 다른 경로 탐색      |
| 조건        | 현재 피로도 ≥ 던전의 최소 필요 피로도            |
| 목표        | 탐험할 수 있는 **최대 던전 수**를 찾는 것이 목적 |

## ⏱️ 시간 복잡도

DFS 깊이: 최대 n (던전 수)

경우의 수: O(n!) (모든 순열 탐색)

피로도 조건 확인 및 백트래킹 포함

→ 총 시간 복잡도: O(n!)

작은 n (≤ 8)이므로 완전탐색이 가능함.
