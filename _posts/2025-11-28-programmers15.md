---
title: "[프로그래머스] 여행경로 (DFS + 백트래킹)"
date: 2025-11-28
categories: [Algorithm, Programmers]
tags: [DFS, 백트래킹, 그래프, Java]
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

# ✈️ Programmers - 여행경로 (DFS + 백트래킹)

## 📌 문제 개요

주어진 항공권을 모두 사용하여 **ICN**에서 시작하는 여행 경로를 찾아야 한다.  
가능한 경로가 여러 개인 경우 **사전순으로 가장 빠른 경로**를 반환해야 한다.

---

# 🧠 접근 방법

이 문제의 핵심은 다음 두 가지다:

1. **모든 티켓을 사용해야 한다 → 완전탐색 / DFS**
2. **여러 경로 중 사전순으로 가장 앞서는 경로 선택 → 티켓을 목적지 기준 정렬**

따라서 DFS + 백트래킹을 사용하여 모든 경로를 탐색하되,  
**정렬을 먼저 수행해 가장 사전순 앞서는 경로부터 탐색**한다.

---

# 📂 사용된 자료구조 및 핵심 로직

### 🔸 `visited[]`

- 티켓 사용 여부 저장
- 같은 출발/도착지를 가진 중복 티켓을 정확하게 구분하기 위해 필요

### 🔸 `answer`

- 실제 경로를 저장하는 리스트
- DFS를 돌면서 도착지를 하나씩 push하고 실패 시 pop하는 방식(백트래킹)

### 🔸 DFS 핵심 포인트

- 모든 티켓을 사용했다면 종료 조건 만족 → true 반환
- 가능 티켓을 하나씩 선택하며 DFS 재귀 호출
- 재귀가 성공하면 상위 호출까지 계속 true 리턴되어 탐색 종료
- 실패하면 방문 취소 후 경로 제거 → 백트래킹

---

# 🧩 전체 코드

```java
package Programmers;

import java.util.*;

public class programmers15 {
    // 티켓 사용 여부를 기록하는 배열
    static boolean[] visited;

    // 여행 경로를 저장하는 리스트
    static List<String> answer = new ArrayList<>();

    public String[] solution(String[][] tickets) {
        // 1. 도착지를 기준으로 사전순 정렬
        //    -> DFS에서 먼저 선택된 티켓이 알파벳 순서가 앞서도록 보장
        Arrays.sort(tickets,(a,b) -> a[1].compareTo(b[1]));

        // 2. 방문 배열 초기화
        visited = new boolean[tickets.length];

        // 3. 항상 "ICN"에서 출발
        answer.add("ICN");

        // 4. DFS 호출: 현재 위치 "ICN", depth 0
        dfs("ICN", 0, tickets);

        // 5. List를 배열로 변환하여 반환
        return answer.toArray(new String[0]);
    }

    // DFS 함수: 현재 위치(ticket), 현재 사용한 티켓 수(depth), 전체 티켓 배열
    static boolean dfs(String ticket, int depth, String[][] tickets){
        // 6. 모든 티켓을 다 사용했으면 성공
        if(depth == tickets.length){
            return true;
        }

        // 7. 모든 티켓을 순회하며 선택 가능한 티켓 탐색
        for (int i = 0; i < tickets.length; i++) {
            // 8. 조건: 아직 사용하지 않았고, 출발지가 현재 위치와 일치하는 티켓
            if(!visited[i] && tickets[i][0].equals(ticket)){
                // 9. 티켓 사용 체크
                visited[i] = true;
                // 10. 경로에 목적지 추가
                answer.add(tickets[i][1]);

                // 11. 재귀 DFS 호출: 다음 위치, 사용 티켓 +1
                if(dfs(tickets[i][1], depth + 1, tickets)){
                    // 12. 성공 경로 발견 시 바로 true 반환하여 상위 DFS 종료
                    return true;
                }

                // 13. DFS 실패 시 백트래킹: 방문 체크 원복, 경로 제거
                visited[i] = false;
                answer.remove(answer.size() - 1);
            }
        }

        // 14. 모든 티켓을 시도해도 실패하면 false 반환
        return false;
    }
}

```

---

# ⏱ 시간 복잡도 분석

| 항목         | 설명                                    |
| ------------ | --------------------------------------- |
| 정렬         | O(N log N)                              |
| DFS 백트래킹 | 최악의 경우 모든 티켓 조합 탐색 → O(N!) |

### 🔎 최종 복잡도

**O(N log N + N!)**

사전순 앞서는 경로를 먼저 탐색하므로 실제 수행 시간은 크게 단축되는 편이다.

---

# 💡 마무리 & 느낀 점

이 문제의 가장 중요한 포인트는 DFS 구조 자체보다  
**정렬을 통해 탐색 순서를 조정하는 접근 방식**이었다.

초기에는 모든 경로를 탐색한 뒤 정렬하려고 했지만,  
탐색 순서를 바꿔버리는 것이 훨씬 효율적이었다.

- 목적지를 기준으로 정렬
- 정렬된 순서로 DFS 진행

이 두 가지만으로 "사전순 최소 경로"가 자연스럽게 먼저 발견되어  
백트래킹이 깔끔하게 작동한다는 점이 흥미로웠다.

---

# 📎 결론

DFS + 백트래킹 문제 중에서도  
정렬을 통한 탐색 순서 제어가 얼마나 강력한지 다시 깨닫게 되는 문제였다.
