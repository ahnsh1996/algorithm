# [2020 KAKAO BLIND RECRUITMENT] 블록 이동하기
## 문제
### 문제 설명
([원본 문제](https://programmers.co.kr/learn/courses/30/lessons/60063)의 내용을 복사한 것입니다.)

로봇개발자 <b>"무지"</b>는 한 달 앞으로 다가온 "카카오배 로봇경진대회"에 출품할 <b>로봇</b>을 준비하고 있습니다. 
준비 중인 로봇은 ```2 x 1``` 크기의 로봇으로 "무지"는 "0"과 "1"로 이루어진 ```N x N``` 크기의 지도에서 ```2 x 1``` 크기인 로봇을 움직여 <b>(N, N)</b> 위치까지 이동 할 수 있도록 프로그래밍을 하려고 합니다. 
로봇이 이동하는 지도는 가장 왼쪽, 상단의 좌표를 <b>(1, 1)</b>로 하며 지도 내에 표시된 숫자 <b>"0"</b>은 빈칸을 <b>"1"</b>은 벽을 나타냅니다. 
로봇은 벽이 있는 칸 또는 지도 밖으로는 이동할 수 없습니다. 
로봇은 처음에 아래 그림과 같이 좌표 <b>(1, 1)</b> 위치에서 가로방향으로 놓여있는 상태로 시작하며, 앞뒤 구분없이 움직일 수 있습니다.

<img width="420" alt="예시1" src="https://user-images.githubusercontent.com/77680436/112272386-7ab11580-8cbf-11eb-888d-5eefed908800.png">

로봇이 움직일 때는 현재 놓여있는 상태를 유지하면서 이동합니다. 
예를 들어, 위 그림에서 오른쪽으로 한 칸 이동한다면 <b>(1, 2), (1, 3)</b> 두 칸을 차지하게 되며, 아래로 이동한다면 <b>(2, 1), (2, 2)</b> 두 칸을 차지하게 됩니다. 
로봇이 차지하는 두 칸 중 어느 한 칸이라도 <b>(N, N)</b> 위치에 도착하면 됩니다.

로봇은 다음과 같이 조건에 따라 회전이 가능합니다.

<img width="420" alt="예시2" src="https://user-images.githubusercontent.com/77680436/112272492-9ddbc500-8cbf-11eb-9e95-5f13be210e6a.png">

위 그림과 같이 로봇은 90도씩 회전할 수 있습니다. 단, 로봇이 차지하는 두 칸 중, 어느 칸이든 축이 될 수 있지만, 회전하는 방향(축이 되는 칸으로부터 대각선 방향에 있는 칸)에는 벽이 없어야 합니다. 로봇이 한 칸 이동하거나 90도 회전하는 데는 걸리는 시간은 정확히 1초 입니다.

<b>"0"</b>과 <b>"1"</b>로 이루어진 지도인 board가 주어질 때, 로봇이 <b>(N, N)</b> 위치까지 이동하는데 필요한 최소 시간을 return 하도록 solution 함수를 완성해주세요.

### 제한사항
* board의 한 변의 길이는 5 이상 100 이하입니다.
* board의 원소는 0 또는 1입니다.
* 로봇이 처음에 놓여 있는 칸 (1, 1), (1, 2)는 항상 0으로 주어집니다.
* 로봇이 항상 목적지에 도착할 수 있는 경우만 입력으로 주어집니다.

___

### 입출력 예
|board|result|
|:---|:---|
|[[0, 0, 0, 1, 1],[0, 0, 0, 1, 0],[0, 1, 0, 1, 1],[1, 1, 0, 0, 1],[0, 0, 0, 0, 0]]|7|

### 입출력 예에 대한 설명
문제에 주어진 예시와 같습니다.  
로봇이 오른쪽으로 한 칸 이동 후, (1, 3) 칸을 축으로 반시계 방향으로 90도 회전합니다. 다시, 아래쪽으로 3칸 이동하면 로봇은 (4, 3), (5, 3) 두 칸을 차지하게 됩니다. 이제 (5, 3)을 축으로 시계 방향으로 90도 회전 후, 오른쪽으로 한 칸 이동하면 (N, N)에 도착합니다. 따라서 목적지에 도달하기까지 최소 7초가 걸립니다.

## 제출답안
```python
from collections import deque
def rangeCheck(now, n): # 지도의 범위를 벗어나는지 체크하는 함수
    return now[0] < 0 or now[0] > n or now[1] < 0 or now[1] > n

def boardCheck(now, board): # 지도에서 벽이 있는지 체크하는 함수
    return board[now[1]][now[0]] == 1

def move(now, direction, board): # 상하좌우로 이동하는 함수
    result = [[now[0][0], now[0][1]], [now[1][0], now[1][1]]]
    idx = [[0, 1], [1, 1]] if direction == 'up' or direction == 'down' else [[0, 0], [1, 0]]
    add = -1 if direction == 'up' or direction == 'left' else 1
    result[idx[0][0]][idx[0][1]] += add
    result[idx[1][0]][idx[1][1]] += add

    # 이동 결과 범위를 벗어나거나 벽이 있는 곳이라면 제자리 반환
    for r in result:
        if rangeCheck(r, len(board)-1) or boardCheck(r, board):
            return now

    return (tuple(result[0]), tuple(result[1]))

def rotation(now, axis, direction, board): # 축과 방향을 정해 회전하는 함수
    result = [[now[0][0], now[0][1]], [now[1][0], now[1][1]]]
    mode = now[0][0] == now[1][0] # True = 세로모드
    idx = [[1, 0], [1, 1]] if axis == 'left' else [[0, 0], [0, 1]]
    l_add = -1 if (not mode and axis == 'left') or (mode and direction == 'down') else 1
    r_add = -1 if (not mode and direction == 'up') or (mode and axis == 'left') else 1
    result[idx[0][0]][idx[0][1]] += l_add
    result[idx[1][0]][idx[1][1]] += r_add
    temp = None
    temp_add = -1 if (not mode and direction == 'up') or (mode and direction == 'down') else 1
    if mode: # 세로모드
        temp = (now[idx[0][0]][idx[0][1]]+temp_add, now[idx[1][0]][idx[1][1]])
    else:
        temp = (now[idx[0][0]][idx[0][1]], now[idx[1][0]][idx[1][1]]+temp_add)
    
    # 회전 결과 범위를 벗어나거나 벽이 있는 곳이라면 제자리 반환
    for r in result:
        if rangeCheck(r, len(board)-1) or boardCheck(r, board):
            return now
    
    # 회전하는 방향에 벽이 있으면 제자리 반환
    if rangeCheck(temp, len(board)-1) or boardCheck(temp, board):
        return now

    result.sort() # 항상 왼쪽 좌표가 더 작은 좌표로 만들기 위해 정렬
    return (tuple(result[0]), tuple(result[1]))

def bfs(start, board, visited): # 시간에 따라 이동할 수 있는 모든 경우 탐색
    visited.add(start)
    queue = deque([(start, 0)])
    n = len(board) - 1

    while queue:
        now, time = queue.popleft()
        if (now[0][0] == n and now[0][1] == n) or (now[1][0] == n and now[1][1] == n):
            return time # 탐색 도중 도착했다면 현재 시간 반환

        for direction in ['left', 'right', 'up', 'down']: # 상하좌우 중 이동할 수 있는 경우 탐색
            temp = move(now, direction, board) # move의 결과 이동할 수 없어 제자리가 반환되었다면 어차피 visited 안에 있어서 이동 X
            if temp not in visited: # 아직 방문하지 않았다면 방문
                visited.add(temp)
                queue.append((temp, time+1))

        for axis in ['left', 'right']: # 회전축과 회전방향을 모두 고려하여 회전할 수 있는 경우 탐색
            for direction in ['up', 'down']:
                temp = rotation(now, axis, direction, board)
                if temp not in visited:
                    queue.append((temp, time+1))

def solution(board):
    return bfs(((0, 0), (1, 0)), board, set()) # 처음 시작 위치에서 탐색 시작
```
### 설명
본 문제의 경우 로봇의 이동을 구현하고 BFS를 통하여 이동 가능한 경우를 탐색하여 해결할 수 있다.

BFS를 작성하는 것보단 로봇이 ```1 x 1```이 아닌 ```2 x 1``` 크기에다가 회전까지 가능하기 때문에 생각보다 구현이 까다로운 문제였다.  
~~(그래서 그런지 시간 제한이 있는 실제 코딩 테스트에서는 정답률이 1.4%라고 한다.)~~

회전이나 이동을 구현하는 경우 하나하나 모든 경우를 손수 코딩해주는 방법도 있고, 본 제출처럼 일부 공통점을 찾아 구현할 수도 있고 등등 다양한 방법이 있기 때문에 특별히 설명하지 않는다.  
사실 이해가 어렵다기 보단 모든 경우를 하나하나 생각해보는 것이 귀찮고 헷갈리기 때문에 직접 구현해보는 것이 제일 나을 것이다.  
(카카오 블로그에 따르면 회전 충돌 확인을 위해서 맨해튼 거리를 이용할 수도 있다고 한다. 혹시 이를 안다면 이런 것들을 사용해서 구현할 수도 있다.)

BFS의 경우 시간이 지남에 따라 이동할 수 있는 모든 경우의 수를 탐색한다.  
사실 처음에는 BFS보다 DFS가 익숙하여 다음과 같이 구현하였었다.

```python
min_time = 0
def dfs(now, board, visited, time):
    global min_time
    if min_time and time >= min_time: return
    
    n = len(board) - 1
    if (now[0][0] == n and now[0][1] == n) or (now[1][0] == n and now[1][1] == n):
        if not min_time or time < min_time:
            min_time = time
            
    visited.add(now)
    
    for direction in ['left', 'right', 'up', 'down']:
        temp = move(now, direction, board)
        if temp not in visited:
            dfs(temp, board, visited, time+1)
            
    for axis in ['left', 'right']:
        for direction in ['up', 'down']:
            temp = rotation(now, axis, direction, board)
            if temp not in visited:
                dfs(temp, board, visited, time+1)
    
    visited.remove(now)
```
위와 같이 min_time을 구한 이후에는 해당 min_time을 넘어가는 탐색을 종료하도록 구현하였음에도 시간 초과가 나서 성공하지 못 하였다.

하지만 BFS를 통해 구현함으로써 해결할 수 있었는데, BFS는 너비 우선으로 탐색하기 때문에 항상 시간 순으로 탐색을 한다. 
따라서 BFS를 수행하는 도중 이미 visited에 있다면 해당 위치로 이동하는 방법이 더 빠른 방법이 있다는 것을 의미하고 불필요한 탐색을 수행하지 않을 수 있다.  
또한 시간 순으로 탐색하기 때문에 가장 먼저 도착한 경우가 있다면 그것이 바로 정답이 된다. 따라서 DFS는 더 오래 걸리는 이동 경로가 먼저 발견될 수도 있지만, BFS는 최적의 시간을 먼저 찾아 
함수를 종료할 수 있다. 그리고 재귀가 아닌 큐를 이용하기 때문에 이 점에서도 어느 정도 더 빠를 것이다.

아무튼 결론적으로 위와 같이 이동과 회전을 구현하고, BFS를 통해 가장 빠른 시간을 탐색한다면 본 문제를 해결할 수 있다.
