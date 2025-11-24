---
title: "[프로그래머스] 게임 맵 최단거리 (BFS 탐색)"
date: 2025-11-24
categories: [Algorithm, Programmers]
tags: [BFS, 그래프, 네트워크, 탐색, Java]
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

# 🎮 프로그래머스 - 게임 맵 최단거리

> **문제 설명:**  
> N x M 크기의 맵에서 캐릭터가 (0, 0)에서 출발하여  
> 목적지 (N-1, M-1)까지 도달할 때 **최단 거리를 구하는 문제**입니다.  
> 벽은 `0`, 이동 가능한 길은 `1`로 표시됩니다.  
> BFS를 사용해야 최단거리를 구할 수 있습니다.

---

## 🧩 문제 접근

이 문제는 **그래프 탐색 중 BFS**를 사용해 해결할 수 있습니다.

- 격자(grid)의 각 위치는 **노드**
- 상하좌우로 연결된 칸은 **간선**
- BFS는 ‘가까운 노드부터 탐색’하기 때문에 **최단거리 문제에 최적**

---

## 🧠 접근 방법

1. (0, 0)에서 BFS 시작
2. 상·하·좌·우 4방향으로 이동
3. 이동 가능한 칸(=1)만 탐색
4. 방문한 칸은 `distances[][]` 배열에 **현재 거리 + 1**로 기록
5. 목적지에 도달하는 순간 그 값이 최단거리

도달할 수 없다면 `-1` 반환.

---

## ✅ 전체 코드

```java
package Programmers;

import java.util.Arrays;
import java.util.LinkedList;
import java.util.Queue;

public class programmers12 {

    // 4방향 이동을 위한 배열 (동, 서, 남, 북)
    public static int[] dx = {0, 0, 1, -1};  // x 좌표 (가로 방향)
    public static int[] dy = {1, -1, 0, 0};  // y 좌표 (세로 방향)

    public int solution(int[][] maps) {
        // 최단 경로를 저장할 변수
        int answer = 0;

        // 맵의 크기 (n은 세로, m은 가로)
        int n = maps.length;
        int m = maps[0].length;

        // 큐 초기화 (BFS에 사용할 큐)
        Queue<int[]> queue = new LinkedList<>();

        // 각 칸까지의 최단 거리를 기록하는 배열
        int[][] distances = new int[n][m];

        // distances 배열을 0으로 초기화
        for(int[] row : distances){
            Arrays.fill(row, 0);  // 모든 칸을 0으로 설정 (0은 아직 방문하지 않은 칸)
        }

        // 시작점 (0, 0)을 큐에 넣고, 해당 위치의 거리를 1로 설정
        queue.offer(new int[]{0, 0});
        distances[0][0] = 1;  // 시작점은 1칸이므로 1로 설정

        // BFS 탐색 시작
        while (!queue.isEmpty()) {
            // 큐에서 하나의 위치를 꺼냄
            int[] current = queue.poll();
            int x = current[0];  // 현재 위치의 x 좌표
            int y = current[1];  // 현재 위치의 y 좌표

            // 목표 지점 (n-1, m-1)에 도달하면 그때까지의 거리 반환
            if (x == n - 1 && y == m - 1) {
                return distances[x][y];  // 목표 지점까지의 최단 거리 반환
            }

            // 4방향으로 인접한 칸을 탐색
            for (int i = 0; i < 4; i++) {
                int nx = x + dx[i];  // x 방향으로 이동
                int ny = y + dy[i];  // y 방향으로 이동

                // 이동한 위치가 유효한 범위 내에 있고, 벽이 아니며, 아직 방문하지 않은 곳이면
                if (nx >= 0 && nx < n && ny >= 0 && ny < m && maps[nx][ny] == 1 && distances[nx][ny] == 0) {
                    // 해당 칸의 거리를 갱신
                    distances[nx][ny] = distances[x][y] + 1;
                    // 큐에 새 위치를 추가
                    queue.offer(new int[]{nx, ny});
                }
            }
        }

        // 목표 지점에 도달할 수 없는 경우에는 -1을 반환
        return -1;
    }
}

```

---

## 💡 사용한 알고리즘 및 자료구조

| 항목     | 설명                                   |
| -------- | -------------------------------------- |
| 알고리즘 | BFS (너비 우선 탐색)                   |
| 자료구조 | Queue (탐색 순서를 관리)               |
| 보조배열 | `distances[][]` — 각 칸까지의 최단거리 |
| 입력     | 2차원 배열 (맵 정보)                   |
| 출력     | 최단 거리 (정수)                       |

---

## 🔍 핵심 로직 설명

```java
if (nx >= 0 && nx < n && ny >= 0 && ny < m
    && maps[nx][ny] == 1 && distances[nx][ny] == 0) {

    distances[nx][ny] = distances[x][y] + 1;
    queue.offer(new int[]{nx, ny});
}
```

- maps[nx][ny] == 1 → 이동 가능한 길
- distances[nx][ny] == 0 → 아직 방문하지 않은 칸
- 방문 시 거리 = 이전 거리 + 1
- BFS이기 때문에 가장 먼저 도착하는 값이 최단거리

---

## 🧮 시간 복잡도

| 구분       | 복잡도   |
| ---------- | -------- |
| BFS 탐색   | O(N × M) |
| 공간복잡도 | O(N × M) |

- 격자 전체를 최대 한 번씩만 방문합니다.

---

## ✨ 마무리 & 느낀점

이 문제를 통해 BFS 기반의 최단거리 탐색의 기본 구조를 다시 한번 정리할 수 있었습니다.

- 거리 기록 배열(distances)의 필요성
- BFS가 최단경로 문제에서 항상 먼저 고려되는 이유
- DFS와의 차이점 명확화

특히 이동 가능한 좌표 체크와 방문 체크가 복습에 큰 도움이 되었습니다.
