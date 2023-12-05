# CHAPTER05 DFS/BFS

## 4. 실전 문제 - 미로 탈출

#### `idea`

- 돌아가지 않는(움직일 수 있는 모든 칸을 카운팅X) 최소 이동거리 칸 개수 구하기?  
  → 가까운 노드부터 탐색하는 BFS

  > 📌 BFS의 동작 방식  
  > _1. 시작 노드 큐에 삽입 하고 해당 노드 방문처리.  
  > 2. 큐에서 노드를 꺼내 해당 노드의 인접 노드 중 방문하지 않은 노드를 모두 큐에 삽입하고 방문 처리.  
  > 3. 2번의 과정을 더 이상 수행할 수 없을 때까지 반복한다._

- 최소 거리로 (n,m)까지 가려면 가능하면 x, y값을 - 가 아닌 + 값으로 이동하기
- 큐에는 모든 칸 넣기X, 값이 `1` 인 칸만 넣기 O
- def bfs(): 책에서 배운 BFS 구현 예제의 graph는 노드 간의 연결정보인데, 여기서 입력받은 graph의 값은 그냥 맵의 정보이다…어떻게 하면 될까?  
  → 시작노드 (1,1) 에서 칸 카운팅(이동 거리 카운트)하고 주변 상,하,좌,우 노드 확인하기.  
   → 맵 크기 벗어나는 경우: continue  
   → 주변 노드가 1인 경우: 이동하고 카운팅  
   → 주변 노드가 0인 경우: continue

⇒ BFS를 코드로 어떻게 적용해야 할지 잘 모르겠다. 2차원 배열과 queue를 같이 사용할 때 어떤식으로 소스 코드를 작성해야 할지 헷갈림.

#### `작성한 코드`

```python
from collections import deque

# n, m 크기의 미로 입력받기
n, m = map(int, input().split())

# 미로 맵 정보 입력받기
graph = []
for i in range(n):
	graph.append(list(int, input())

def bfs(...)
	queue = deque()

...
```

#### `답안 예시`

```python
from collections import deque

# N, M을 공백을 기준으로 구분하여 입력 받기
n, m = map(int, input().split())
# 2차원 리스트의 맵 정보 입력 받기
graph = []
for i in range(n):
    graph.append(list(map(int, input())))

# 이동할 네 가지 방향 정의 (상, 하, 좌, 우)
dx = [-1, 1, 0, 0]
dy = [0, 0, -1, 1]

# BFS 소스코드 구현
def bfs(x, y):
    # 큐(Queue) 구현을 위해 deque 라이브러리 사용
    queue = deque()
    queue.append((x, y))
    # 큐가 빌 때까지 반복하기
    while queue:
        x, y = queue.popleft()
        # 현재 위치에서 4가지 방향으로의 위치 확인
        for i in range(4):
            nx = x + dx[i]
            ny = y + dy[i]
            # 미로 찾기 공간을 벗어난 경우 무시
            if nx < 0 or nx >= n or ny < 0 or ny >= m:
                continue
            # 벽인 경우 무시
            if graph[nx][ny] == 0:
                continue
            # 해당 노드를 처음 방문하는 경우에만 최단 거리 기록
            if graph[nx][ny] == 1:
                graph[nx][ny] = graph[x][y] + 1
                queue.append((nx, ny))
    # 가장 오른쪽 아래까지의 최단 거리 반환
    return graph[n - 1][m - 1]

# BFS를 수행한 결과 출력
print(bfs(0, 0))
```

1. BFS를 사용해 시작 지점부터 가까운 노드부터 차례대로 탐색하기.
2. (1,1) 부터 BFS를 수행해 **모든 노드의 값을 거리 정보로 넣기**.
   - (1,1)의 값은 항상 1이다. 상하좌우 탐색을 진행하기
   - 옆 노드인 (1,2)를 방문하게 되고, 새로 방문한 (1,2) 노드의 값을 2로 바꾸기
   - BFS를 계속 수행하면 **최단 경로의 값들이 1씩 증가하는 형태로 변경**된다.
