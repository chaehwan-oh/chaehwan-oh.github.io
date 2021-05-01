---
layout: post
subclass: post
title: "그래프의 탐색 - DFS와 BFS"
date: 2019-04-08
tags: [problem-solving, graph, graph-traversal, data structure, dfs, bfs]
excerpt: "자료구조 그래프의 탐색에 대한 설명입니다."
disqus: True
---

## 그래프의 탐색

그래프 탐색의 목적은? 시작점에서 출발해서 모든 정점을 1번씩 방문하는 것이다.

이 목적을 달성하기 위해 크게 2가지 방법을 사용할 수 있는데 DFS(Depth First Search: 깊이 우선 탐색), BFS(Breadth First Search: 너비 우선 탐색)이다.

시간복잡도는 그래프 자료구조를 인접 리스트 형태로 표현했을 때 DFS와 BFS 모두 O(v+e)이다. 모든 정점과 간선을 방문하기 때문이다.

두 알고리즘 모두 중요한데 DFS는 다른 알고리즘의 기초가 되기 때문에 중요하다. 또, BFS가 중요한 이유는 특정 조건을 만족했을 때 최단거리 알고리즘이 되기 때문이다. 모든 가중치가 1일 때 BFS를 사용하면 최단거리를 구할 수 있다.

## DFS(Depth First Search: 깊이 우선 탐색)

깊이 우선 탐색은 시작 정점의 한 방향으로 갈 수 있는 경로가 있는 곳까지 깊이 탐색해 가다가 더 이상 갈 곳이 없으면 가장 마지막에 만났던 갈림길 간선이 있는 정점으로 되돌아와서 다른 방향의 간선으로 탐색을 계속하여 결국 모든 정점을 방문하는 순회 방법이다.

**가장 마지막에 만났던 갈림길 간선의 정점으로 되돌아와 탐색을 계속 진행해야 하므로 후입선출 구조의 스택을 사용한다.** 또, 재귀함수를 사용하는 것도 가능하다.

### 깊이 우선 탐색의 수행 순서

```
1) 시작 정점 v를 결정하여 방문한다.
2) 정점 v에 인접한 정점 중에서
    2-1) 방문하지 않은 정점 next가 있으면 정점 current를 스택에 push하고 next를 방문한다. 그리고 next를 current로 하여 다시 2)를 수행한다.
    2-2) 방문하지 않은 정점이 없으면 스택에서 pop하여 받은 가장 마지막 정점을 v로 설정한 뒤 다시 2)를 수행한다.
3) 스택이 빌 때까지 2)를 반복한다.
```

### 스택을 사용한 구현

```cpp
void dfs(vector<int> graph[], int vertex)
{
    bool visited[5] = { false, };
    stack<int> dfsStack;

    int current = vertex;
    visited[current] = true;
    dfsStack.push(current);
    cout << current << " ";
    while (!dfsStack.empty())
    {
        for (int i = 0; i < graph[current].size(); i++)
        {
            int adjacentVertex = graph[current][i];
            if (!visited[adjacentVertex])
            {
                current = adjacentVertex;
                visited[current] = true;
                dfsStack.push(current);
                cout << current << " ";
            }
        }
        dfsStack.pop();
    }
}
```

### 재귀함수를 사용한 구현

1. C++

```cpp
void dfs(vector<int> graph[], int vertex, bool visited[])
{
    visited[vertex] = true;
    cout << vertex << " ";
    for (int i = 0; i < graph[vertex].size(); i++)
    {
        int adjacentVertex = graph[vertex][i];
        if (!visited[adjacentVertex])
        {
            dfs(graph, adjacentVertex, visited);
        }
    }
}
```

2. node.js

```javascript
  depthFirstSearch(startingVertex) {
    let visited = [];
    for (let i = 0; i < this.numberOfVertex; i++) {
      visited[i] = false;
    }
    this.dfs(startingVertex, visited);
  }

  dfs(vertex, visited) {
    visited[vertex] = true;
    console.log(vertex);

    let connectedVertex = this.graph.get(vertex);
    for (let i in connectedVertex) {
      let adjacentVertex = connectedVertex[i];
      if (!visited[adjacentVertex]) {
        this.dfs(adjacentVertex, visited);
      }
    }
  }
```

## BFS(Breadth First Search: 너비 우선 탐색)

너비 우선 탐색은 시작 정점으로부터 인접한 정점들을 모두 차례로 방문하고 나서 방문했던 정점을 다시 시작으로 다시 인접한 정점들을 차례로 방문하여 가까운 정점들을 먼저 방문하고 멀리 있는 정점들은 나중에 방문하는 순회 방법이다.

**인접한 정점들에 대해서 차례로 다시 너비 우선 탐색을 반복해야 하므로 선입선출 구조를 갖는 큐를 사용한다.**

### 너비 우선 탐색의 수행 순서

```
1) 시작 정점 vertex를 결정하여 방문한다.
2) 정점 vertex에 인접한 정점들 중에서 방문하지 않은 정점을 차례로 방문하면서 큐에 enQueue한다.
3) 방문하지 않은 인접한 정점이 없으면 방문했던 정점에서 인접한 정점들을 다시 차례로 방문하기 위해 큐에서 deQueue하여 2)를 반복한다.
4) 큐가 공백이 될 때까지 2), 3)를 반복한다.
```

### 구현

1. C++

```cpp
void bfs(vector<int> graph[], int vertex)
{
    bool visited[VERTICES] = { false, };
    queue<int> bfsQueue;

    int current = vertex;
    visited[current] = true;
    bfsQueue.push(current);

    while (!bfsQueue.empty())
    {
        current = bfsQueue.front();
        bfsQueue.pop();
        cout << current << " ";

        for (int i = 0; i < graph[current].size(); i++)
        {
            int next = graph[current][i];
            if (!visited[next])
            {
                visited[next] = true;
                bfsQueue.push(next);
            }
        }
    }
}
```

2. javascript

```javascript
  breadFirstSearch(startingVertex) {
    let visited = [];
    for (let i = 0; i < this.numberOfVertex; i++) {
      visited[i] = false;
    }

    let bfsQueue = new Queue();

    visited[startingVertex] = true;
    bfsQueue.enqueue(startingVertex);

    while (!bfsQueue.isEmpty()) {
      let current = bfsQueue.dequeue();
      console.log(current);
      let connectedVertices = this.graph.get(current);
      for (let i in connectedVertices) {
        let next = connectedVertices[i];
        if (!visited[next]) {
          visited[next] = true;
          bfsQueue.enqueue(next);
        }
      }
    }
  }
```

## 마무리

다양한 언어로 쉽게 구현이 가능하다. 2가지를 기억하면 좋겠다.

1. 모든 정점을 한 번씩 방문하는 것이 '그래프 탐색'의 정의이므로, visited 리스트를 사용하여
   정점의 방문여부를 관리해야 한다.

2. 왜 깊이 우선 탐색에서 스택을 사용하고, 왜 너비 우선 탐색에서 큐를 사용하는지 기억한다. 깊이 우선 탐색에서는 마지막 갈림길에서 다시 탐색을 이어가기 위해 후입선출 구조의 스택을 사용한다. 너비 우선 탐색은 인접한 정점들에 대해 차례로 탐색하기 위해 선입선출 구조의 큐를 사용한다.
