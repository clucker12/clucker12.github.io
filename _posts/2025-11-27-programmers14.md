---
title: "[프로그래머스] 아이템 줍기 (BFS 탐색)"
date: 2025-11-27
categories: [Algorithm, Programmers]
tags: [BFS, 그래프, 좌표확장, 탐색, Java]
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

# 🎒 프로그래머스 - 아이템 줍기 (BFS 최단 거리)

이 문제는 **캐릭터가 사각형(장애물) 외곽선을 따라 이동하여 아이템까지의 최단거리**를 구하는 문제이다.

사각형이 겹치는 경우가 있기 때문에,  
단순한 BFS로는 **모서리 겹침(코너 끼워들기 문제)** 때문에  
잘못된 경로가 생길 수 있다.

이를 해결하기 위해 **좌표 전체를 2배로 확장**하고,  
사각형의 **외곽선만을 남긴 뒤 BFS로 탐색**하는 방식으로 해결한다.

---

## 🚀 핵심 접근 방법

### ✔ 1. 좌표를 2배로 확장하는 이유

- 사각형이 겹칠 때 외곽선이 붙어버리는 문제 해결
- 정확한 경로 판별
- 반 칸(0.5칸) 단위 이동을 정수로 표현 가능

### ✔ 2. 확장된 좌표에서 직사각형 테두리를 1로, 내부는 0으로

- 외곽선만 이동 가능하도록 설계
- BFS로 최단거리 탐색

### ✔ 3. BFS로 네 방향 탐색하며 최소 거리 계산

- 목표 지점 도달 시 거리 반환
- 2배 확장했기 때문에 실제 이동 거리 = dist / 2

---

## 📌 전체 코드 (Java)

```java
package Programmers;
import java.util.*;

public class programmers14 {
    // 지도 배열과 방문 여부 배열 선언
    static int[][] map;
    static boolean[][] visited;
    // 상하좌우 이동 방향
    static int[] dx = {0, 0, 1, -1};
    static int[] dy = {1, -1, 0, 0};

    public int solution(int[][] rectangle, int characterX, int characterY, int itemX, int itemY) {
        map = new int[102][102]; // 좌표를 2배로 확장해 모서리 겹침 방지
        visited = new boolean[102][102]; // 방문 체크 배열

        // 직사각형을 2배 확장 좌표로 map에 표시 (1로)
        for (int[] rec : rectangle) {
            int x1 = rec[0]*2;
            int y1 = rec[1]*2;
            int x2 = rec[2]*2;
            int y2 = rec[3]*2;

            for (int i = x1; i <= x2; i++) {
                for (int j = y1; j <= y2; j++) {
                    map[i][j] = 1; // 직사각형 영역 전체를 1로 표시
                }
            }
        }

        // 직사각형 내부를 제거하여 외곽선만 남김
        for (int[] rec : rectangle) {
            int x1 = rec[0]*2;
            int y1 = rec[1]*2;
            int x2 = rec[2]*2;
            int y2 = rec[3]*2;

            for (int i = x1+1; i < x2; i++) {
                for (int j = y1+1; j < y2; j++) {
                    map[i][j] = 0; // 내부는 0으로
                }
            }
        }

        // BFS를 위한 큐 초기화, 시작 위치 넣기
        Queue<int[]> queue = new LinkedList<>();
        queue.offer(new int[]{characterX*2, characterY*2, 0}); // x, y, 거리
        visited[characterX*2][characterY*2] = true;

        // BFS 시작
        while (!queue.isEmpty()) {
            int[] curr = queue.poll();
            int x = curr[0];
            int y = curr[1];
            int dist = curr[2];

            // 아이템 위치에 도달하면 거리 반환 (2배 확장했으므로 2로 나눔)
            if (x == itemX*2 && y == itemY*2) {
                return dist / 2;
            }

            // 상하좌우 탐색
            for (int i = 0; i < 4; i++) {
                int nx = x + dx[i];
                int ny = y + dy[i];

                // 지도 범위 내이며, 방문하지 않았고 외곽선이면 이동
                if (nx >= 0 && nx <= 101 && ny >= 0 && ny <= 101) {
                    if (!visited[nx][ny] && map[nx][ny] == 1) {
                        visited[nx][ny] = true;
                        queue.offer(new int[]{nx, ny, dist + 1}); // 거리 +1
                    }
                }
            }
        }

        return 0; // 도달 불가 시
    }
}

```

---

## 🔍 주요 로직 설명

🔸 직사각형 전체를 1로 채우기

```
map[i][j] = 1;
```

좌표 확장 후, 사각형 해당 구역을 전부 1로 채워 기본 형태를 만든다.

🔸 내부를 0으로 지워 테두리만 남기는 과정

```
if (i > x1 && i < x2 && j > y1 && j < y2)
    map[i][j] = 0;

```

이 과정을 거쳐 오직 테두리만 BFS의 경로로 남는다.

🔸 BFS 탐색 (최단 거리)

```
queue.offer(new int[]{nx, ny, dist + 1});
```

- bfs 특성상 첫 도달 = 최단거리
- 좌표 확장했으므로 마지막에 / 2 한 값이 실제 정답

---

## 🧬 사용된 자료구조

| 자료구조              | 역할                         |
| --------------------- | ---------------------------- |
| `int[][] map`         | 확장된 좌표 기반 외곽선 표시 |
| `boolean[][] visited` | 중복 탐색 방지               |
| `Queue<int[]>`        | BFS용                        |
| `dx, dy`              | 상하좌우 이동                |

---

## ⏱ 시간 복잡도

사각형 표시는 최대 100×100 크기의 확장된 좌표에서 수행됨.

- 전체 grid 크기 ≈ 100×100
- BFS 탐색: O(N × M)
- 직사각형 개수를 R이라 하면,
  - 테두리 생성: O(R × 100²)

➡ 전체 시간 복잡도: O(100² + BFS) = O(10,000)
실제로는 매우 빠르게 수행된다.

---

## 💡 느낀점 / 문제 접근

이 문제는 처음 보면 BFS 문제처럼 보이지만,
실제로는 좌표 전처리(geometry 처리) 가 핵심인 문제였다.

가장 중요했던 포인트는:

- 그냥 BFS 하면 사각형 모서리에서 잘못된 경로가 생김
- 좌표를 2배 확장하는 아이디어가 없으면 풀기 어렵다
- 테두리만 남기는 로직이 전체 문제의 절반을 차지한다고 느꼈다

문제 접근 자체는 BFS였지만,
정확한 맵 전처리 기반의 BFS라는 점에서
단순 그래프 탐색과는 다른 느낌이었다.

복잡한 좌표 문제에서 “확장해서 단순화하는 전략”을
앞으로도 활용할 수 있을 것 같다는 생각이 들었다.
