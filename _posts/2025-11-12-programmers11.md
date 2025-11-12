---
title: "[프로그래머스] 네트워크 (DFS 탐색)"
date: 2025-11-12
categories: [Algorithm, Programmers]
tags: [DFS, 그래프, 네트워크, 탐색, Java]
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

# 💻 프로그래머스 - 네트워크

> **문제 설명:**  
> 네트워크 상에서 서로 연결되어 있는 컴퓨터들의 개수를 구하는 문제입니다.  
> 컴퓨터의 개수 `n`과 각 컴퓨터 간의 연결 여부가 담긴 `computers` 2차원 배열이 주어집니다.

---

## 🧩 문제 접근

이 문제는 **그래프 탐색(DFS)** 알고리즘의 기본적인 응용 문제입니다.  
각 컴퓨터는 노드(node), 연결 관계는 간선(edge)으로 생각할 수 있습니다.

- `computers[i][j] == 1` → i번 컴퓨터와 j번 컴퓨터가 연결되어 있음
- 연결된 컴퓨터 그룹을 하나의 네트워크로 간주
- 방문하지 않은 컴퓨터를 기준으로 DFS를 수행하여 **하나의 네트워크 전체를 탐색**

---

## 🧠 접근 방법

1. 모든 컴퓨터를 순회하며, 방문하지 않은 컴퓨터를 찾는다.
2. 해당 컴퓨터에서 DFS를 시작하여 연결된 모든 컴퓨터를 방문 처리한다.
3. DFS가 한 번 끝날 때마다 `answer++` (하나의 네트워크 완성)

---

## ✅ 전체 코드

```java
package Programmers;

public class programmers11 {
    // 문제 해결 메서드
    public int solution(int n, int[][] computers) {
        int answer = 0;                   // 네트워크 개수를 세기 위한 변수
        boolean[] visited = new boolean[n]; // 각 컴퓨터의 방문 여부를 저장하는 배열

        // 모든 컴퓨터를 한 번씩 확인
        for (int i = 0; i < n; i++) {
            // 아직 방문하지 않은 컴퓨터라면 (새로운 네트워크 발견)
            if (!visited[i]) {
                dfs(i, computers, visited, n); // 해당 컴퓨터와 연결된 모든 컴퓨터 탐색
                answer++;                      // 하나의 네트워크 탐색이 끝났으므로 +1
            }
        }

        // 모든 컴퓨터를 확인한 후, 네트워크의 개수 반환
        return answer;
    }

    // 깊이 우선 탐색(DFS) 메서드
    private void dfs(int node, int[][] computers, boolean[] visited, int n) {
        visited[node] = true; // 현재 컴퓨터(node)를 방문 처리

        // 현재 컴퓨터와 연결된 다른 컴퓨터들을 탐색
        for (int i = 0; i < n; i++) {
            // computers[node][i] == 1 : 연결되어 있음
            // !visited[i] : 아직 방문하지 않은 컴퓨터
            if (computers[node][i] == 1 && !visited[i]) {
                dfs(i, computers, visited, n); // 연결된 컴퓨터로 재귀 탐색
            }
        }
    }
}
```

---

## 💡 사용한 알고리즘 및 자료구조

| 항목     | 설명                                                  |
| -------- | ----------------------------------------------------- |
| 알고리즘 | DFS (깊이 우선 탐색)                                  |
| 자료구조 | boolean 배열 (`visited[]`) - 각 노드의 방문 여부 관리 |
| 입력     | 2차원 배열 (연결 관계)                                |
| 출력     | 네트워크의 개수 (정수)                                |

---

## 🔍 주요 메서드 & 핵심 로직

```java
private void dfs(int node, int[][] computers, boolean[] visited, int n) {
    visited[node] = true; // 현재 컴퓨터 방문 처리

    for (int i = 0; i < n; i++) {
        if (computers[node][i] == 1 && !visited[i]) {
            dfs(i, computers, visited, n); // 연결된 컴퓨터 재귀 탐색
        }
    }
}
```

- visited[node] : 현재 노드를 방문 처리
- computers[node][i] == 1 : 두 노드가 연결되어 있는지 확인
- 재귀적으로 연결된 모든 노드를 방문하여 한 네트워크를 완성

---

## 🧮 시간 복잡도

| 구분           | 복잡도 |
| -------------- | ------ |
| DFS 한 번 수행 | O(n)   |
| 전체 탐색      | O(n²)  |

- 이유: 모든 노드에 대해 연결 여부를 확인해야 하므로, computers 배열 전체를 한 번씩 탐색하게 됩니다.

---

## ✨ 마무리 & 느낀점

이번 문제는 DFS의 전형적인 응용 문제로,
연결 요소(Connected Component) 개수를 찾는 전형적인 그래프 탐색 패턴을 복습할 수 있었습니다.

특히 방문 배열(visited)의 사용이 매우 중요하다는 점,
그리고 한 번의 DFS로 하나의 네트워크를 모두 탐색할 수 있다는 점이 핵심이었습니다.

이 문제를 통해 그래프 탐색 구조를 이해하고, 재귀 기반 DFS의 흐름을 다시 점검할 수 있었습니다.
