---
layout: post
subclass: post
title: "BOJ 11048번: 이동하기"
date: 2018-08-12
tags: [problem-solving]
excerpt: "problem-solving"
disqus: True
---

# Understanding the problem

![move_in_the_maze1](/images/problem-solving/move_in_the_maze1.PNG)

![move_in_the_maze2](/images/problem-solving/move_in_the_maze2.PNG)

source:
[BOJ-이동하기](https://www.acmicpc.net/problem/11048)

## 1. 내가 알 수 있는 정보는?

(1, 1)에서 (N, M)으로 이동하면서 얻을 수 있는 사탕의 최대 개수를 파악하는 것이 큰 문제이고 (A, B) -> (C, D)로 이동하면서 얻을 수 있는 사탕의 최대 개수를 파악하는 것이 부분 문제이다. 세 방향 중 하나로 이동하면서 문제의 크기는 줄어든다. 즉 부분 문제를 해결해나가다 보면 큰 문제는 해결된다.

## 2. 내가 달성하려는 목표는?

(N, M)으로 이동할 때 가져올 수 있는 사탕의 개수를 구하는 프로그램

# Devising a plan

떠오르는 생각)

- 큰 문제는 (1, 1)에서 (N, M)으로 이동하면서 얻을 수 있는 사탕의 최대 개수를 파악하는 것이다. 여기서 부분 문제는 하나의 칸을 이동하면서 해당 칸의 사탕을 가져가는 것이다. 정확히는 해당 칸에 도착했을 때 얻을 수 있는 사탕의 최대 개수를 파악하는 것이다.
- 결국 구하려는 것은 (1, 1)에서 (N, M)으로 이동하면서 얻을 수 있는 사탕의 최대 개수이고, (row, col) 칸에 도착했을 때 얻을 수 있는 사탕의 개수를 캐시에 저장하면서 답을 파악할 수 있다.
- (row, col)에서 얻을 수 있는 사탕의 최대 개수는 다음을 비교해서 얻을 수 있다.(candy[row][col])

  - (row-1, col)까지 얻을 수 있는 사탕의 개수 + (row, col)에서 얻을 수 있는 사탕의 개수(candy[row-1][col] + maze[row][col])
  - (row, col-1)까지 얻을 수 있는 사탕의 개수 + (row, col)에서 얻을 수 있는 사탕의 개수(candy[row][col-1] + maze[row][col])
  - (row-1, col-1)까지 얻을 수 있는 사탕의 개수 + (row, col)에서 얻을 수 있는 사탕의 개수(candy[row-1][col-1] + maze[row][col])

# Carrying out the plan

## 절차 생각해보기)

### base case 설정하기

- (row, col)이 (1, 1)인 지점을 base case 로 둔다. (candy[1][1])

### double for loop 로 순회하면서 cache 에 정보를 하나씩 저장한다.

- row 또는 col 이 1 보다 작을 때와 row 가 N 보다 클 때, col 이 M 보다 클 때에 대해 예외 처리를 해준다.
- candy[row][col] = max(candy[row-1][col] + maze[row][col], candy[row][col-1] + maze[row][col], candy[row-1][col-1] + maze[row][col])

### (N, M)에서의 cache 값을 반환한다.

코드 작성)

```cpp
#include <cstdio>
#include <algorithm>

using namespace std;

int maze[1001][1001];
int candy[1001][1001];

int main()
{
	int n, m;
	scanf("%d %d", &n, &m);
	for (int row = 1; row <= n; row++){
		for (int col = 1; col <= m; col++){
			scanf(" %d", &maze[row][col]);
			candy[row][col] = maze[row][col];
		}
	}
	candy[1][1] = maze[1][1];
	int row_index[3] = {-1, -1, 0};
	int col_index[3] = {-1, 0, -1};
	for (int row = 1; row <= n; row++){
		for (int col = 1; col <= m; col++){
			for (int i = 0; i < 3; i++){
				int current_row = row + row_index[i];
				int current_col = col + col_index[i];
				if (current_row >= 1 && current_col >= 1 && current_row <= n && current_col <= m){
					if (candy[row][col] < candy[current_row][current_col] + maze[row][col]){
						candy[row][col] = candy[current_row][current_col] + maze[row][col];
					}
				}
			}
		}
	}
	printf("%d", candy[n][m]);
	return 0;
}
```

# Looking back

## 1. 핵심 아이디어 회상하기

내가 해결하려는 큰 문제와 그 속의 부분 문제들을 인식하고 그 부분 문제들이 동일한 구조임을 파악하는 것이 핵심인 문제였다. 이를 파악함으로써 다이나믹 프로그래밍 기법으로 문제를 해결할 수 있었다.

## 2. 더 넓은 문맥으로 확장하기

매트릭스 속에서의 이동이 아니더라도 큰 문제와 부분 문제의 관계와 그 각각의 구조를 파악하는 것이 핵심이다.
