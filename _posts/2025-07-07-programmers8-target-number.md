---
title: "프로그래머스 타겟 넘버 DFS 문제 풀이"
categories: [코딩테스트]
tags: [DFS, Java, 전체 테스트]
date: 2025-07-07 14:00:00 +0900
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

배열 numbers가 주어지고, 각 숫자에 + 또는 -를 붙여 target 숫자를 만들 수 있는 경우의 수를 구하는 문제입니다.

예:

numbers = [1, 1, 1, 1, 1], target = 3

→ 만들 수 있는 방법은 총 5가지입니다.

## 💡 핵심 아이디어

모든 숫자에 대해 + 또는 -를 붙이는 모든 경우의 수를 탐색해야 합니다.

이는 완전탐색이며, 깊이 우선 탐색(DFS)을 통해 구현합니다.

각 단계마다 숫자를 더하거나 빼며, 최종 합이 target이면 성공 카운트를 1 증가시킵니다.

## 📄 자바 코드 풀이

```
package Programmers;

// 메인 클래스 (파일 이름 programmers8.java와 일치)
public class programmers8 {

    // 내부 클래스 Solution: 실제 문제 해결 로직
    class Solution {

        // 문제를 해결하는 main 함수
        public int solution(int[] numbers, int target) {
            // DFS(깊이 우선 탐색)를 시작: index 0부터 시작하고, 초기 합은 0
            return dfs(numbers, target, 0, 0);
        }

        /**
         * DFS 재귀 함수
         * @param numbers : 숫자 배열
         * @param target : 목표 숫자
         * @param index : 현재 탐색 중인 인덱스
         * @param sum : 현재까지의 합
         * @return 현재 경로에서 target을 만들 수 있는 경우의 수
         */
        private int dfs(int[] numbers, int target, int index, int sum) {
            // 모든 숫자를 다 사용한 경우 (기저 조건)
            if (index == numbers.length) {
                // 합이 target과 같으면 1 (성공적인 한 가지 방법)
                return sum == target ? 1 : 0;
            }

            // 현재 숫자를 더하는 경우
            int plus = dfs(numbers, target, index + 1, sum + numbers[index]);

            // 현재 숫자를 빼는 경우
            int minus = dfs(numbers, target, index + 1, sum - numbers[index]);

            // 두 경우의 수를 모두 합산하여 반환
            return plus + minus;
        }
    }
}

```

## 🧠 DFS 진행 예시

입력 예시

numbers = [1, 1, 1, 1, 1], target = 3

DFS 탐색 트리 구조 (일부)

                      0 (sum)
                   /        \
                 +1         -1
               /   \       /   \
            +1     -1   +1     -1
           / \     / \   / \     / \
         ... (총 2^5 = 32개의 경로)

각 단계마다 + 또는 -를 선택합니다.

깊이 5까지 도달했을 때, 합이 target인 경우만 카운트합니다.

최종적으로 5개의 경로에서 합이 3이 됩니다.

## ✅ 시간 복잡도

|| 항목 || 설명 ||
||------||------||
|| 시간 복잡도 || O(2^n) (n: numbers 길이) ||
|| 공간 복잡도 || O(n) (재귀 깊이 만큼 스택 사용) ||

최대 20개의 숫자가 주어지므로, 가능한 경우의 수는 2^20 ≈ 1,000,000입니다.

완전탐색이 가능하며, DFS로 충분히 해결 가능한 범위입니다.

## ✨ 정리

|| 항목 || 설명 ||
||------||------||
|| 탐색 방법 || DFS (깊이 우선 탐색) ||
|| 상태 표현 || index, sum ||
|| 종료 조건 || index == numbers.length ||
|| 성공 조건 || sum == target ||
|| 반환 값 || 조건 만족하는 경로 수의 합 ||

## 🔍 동작 요약

dfs 함수는 현재 인덱스의 숫자를 더하거나 빼는 두 가지 선택지를 재귀적으로 수행합니다.

모든 숫자를 사용한 시점에서 (index == numbers.length) 현재까지의 합이 target과 같다면 1을 반환해 정답을 하나 카운트합니다.

모든 가능한 경로를 탐색하여 target을 만들 수 있는 경우의 수를 반환합니다.

## 🔚 결론

이 문제는 DFS의 기초 개념과 재귀 함수의 구조를 이해하는 데 매우 좋은 예제입니다. <br>
모든 경우를 탐색하여 타겟을 만드는 완전탐색 문제이므로, 재귀/백트래킹 연습에 활용하면 좋습니다.<br>
