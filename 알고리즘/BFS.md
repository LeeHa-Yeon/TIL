# BFS

## BFS (Breadth-First Search) 의 특징

- 루트노드에서 시작해서 인적한 노드를 먼저 탐색하는 방법
- 시작정점으로부터 가까운 정점을 먼저 방문하고 멀리 떨어져 있는 정점을 나중에 방문하는 순회 방법
- 깊게 < 넓게
- 사용 경우 : 두 노드 사이의 최단 경로 / 임의의 경로를 찾고 싶을 때
- DFS보다 조금 더 복잡하다.
- 재귀적으로 동작하지 않음
- DFS와의 큰 차이점 : 어떤 노드를 방문했었는지 여부 검사
- Queue를 사용 - 선입선출 원칙
- Prim / Dijkstra 알고리즘과 유사

- 순서 : A - B - C - D - E - F - G - H - I - J - K

![BFS%202f711e8bc71d4471aeb6f762c766b20f/_2021-05-02__12.43.25.png](BFS%202f711e8bc71d4471aeb6f762c766b20f/_2021-05-02__12.43.25.png)

## Queue를 사용

- BFS에서는 Dequeue / Expand 두 가지 일을 동시에 처리
    - Dequeue : 큐에서 맨 앞에 있는 노드를 꺼냄
    - expand : dequeue로 지운 노드의 자식을 모두 큐에 넣음

                        : 만약 자식이 없다면 패스

    ![BFS%202f711e8bc71d4471aeb6f762c766b20f/_2021-05-02__12.47.44.png](BFS%202f711e8bc71d4471aeb6f762c766b20f/_2021-05-02__12.47.44.png)

- 설명

1. 루트 노드 (시작점) 인 `A` 를 큐에 넣습니다.
2. `A` 를 `Dequeue` 하면서 `Expand` 합니다. 즉, `A` 는 지우고 `A` 의 바로 다음 자식인 `B`, `C`, `D`를 큐의 오른쪽에 넣습니다.
3. 큐의 맨 왼쪽에 있는 B 를 `Dequeue` and `Expand` 합니다. 즉, B 는 지우고 B 의 바로 다음 자식인 E 만 큐에 넣습니다.
4. 큐의 맨 왼쪽에 있는 `C` 를 `Dequeue` and `Expand` 합니다. 즉, `C` 는 지우고 `C` 의 바로 다음 자식인 `F` 를 큐에 넣습니다.
5. 큐의 맨 왼쪽에 있는 `D` 를 `Dequeue` and `Expand` 합니다. 즉, `D` 는 지우고 `C` 의 바로 다음 자식인 `G`, `H` 를 큐에 넣습니다.
6. 큐의 맨 왼쪽에 있는 `E` 를 `Dequeue` and `Expand` 합니다. 즉, `E` 는 지우고 `E` 의 바로 다음 자식인 `I`, `J` 를 큐에 넣습니다.
7. 큐의 맨 왼쪽에 있는 `F` 를 `Dequeue` and `Expand` 합니다. 이 때, `F` 는 자식이 없으므로 큐에 넣을 것이 없습니다.
8. 큐의 맨 왼쪽에 있는 `G` 를 `Dequeue` and `Expand` 합니다. 이 때, `G` 는 자식이 없으므로 큐에 넣을 것이 없습니다.
9. 큐의 맨 왼쪽에 있는 `H` 를 `Dequeue` and `Expand` 합니다. 즉, `H` 는 지우고 `H` 의 바로 다음 자식인 `K` 를 큐에 넣습니다.
10. 큐의 맨 왼쪽에 있는 `I` 를 `Dequeue` and `Expand` 합니다. 이 때, `I` 는 자식이 없으므로 큐에 넣을 것이 없습니다.
11. 큐의 맨 왼쪽에 있는 `J` 를 `Dequeue` and `Expand` 합니다. 이 때, `J` 는 자식이 없으므로 큐에 넣을 것이 없습니다.
12. 큐의 맨 왼쪽에 있는 `K` 를 `Dequeue` and `Expand` 합니다. 이 때, `K` 는 자식이 없으므로 큐에 넣을 것이 없습니다.
13. 큐가 비었습니다. 모든 노드를 탐색했다는 뜻

## 파이썬 코드

```python
# queue 사용
def BFS(graph, start_node) :
    visit = list()
    queue = list()

    queue.append(start_node)

    while queue :
        node = queue.pop(0)
        if node not in visit :
            visit.append(node)
            queue.extend(graph[node])

    return visit

graph = {
    'A': ['B'],
    'B': ['A', 'C', 'H'],
    'C': ['B', 'D'],
    'D': ['C', 'E', 'G'],
    'E': ['D', 'F'],
    'F': ['E'],
    'G': ['D'],
    'H': ['B', 'I', 'J', 'M'],
    'I': ['H'],
    'J': ['H', 'K'],
    'K': ['J', 'L'],
    'L': ['K'],
    'M': ['H']
}

print(BFS(graph,'A'))
```
