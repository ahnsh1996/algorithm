# [2021 KAKAO BLIND RECRUITMENT] 카드 짝 맞추기
## 문제
### 문제 설명
([원본 문제](https://programmers.co.kr/learn/courses/30/lessons/72415)의 내용을 복사한 것입니다.)

게임 개발자인 `베로니`는 개발 연습을 위해 다음과 같은 간단한 카드 짝맞추기 보드 게임을 개발해 보려고 합니다.  
게임이 시작되면 화면에는 카드 16장이 뒷면을 위로하여 `4 x 4` 크기의 격자 형태로 표시되어 있습니다. 각 카드의 앞면에는 카카오프렌즈 캐릭터 그림이 그려져 있으며, 8가지의 캐릭터 그림이 그려진 카드가 각기 2장씩 화면에 무작위로 배치되어 있습니다.  
유저가 카드를 2장 선택하여 앞면으로 뒤집었을 때 같은 그림이 그려진 카드면 해당 카드는 게임 화면에서 사라지며, 같은 그림이 아니라면 원래 상태로 뒷면이 보이도록 뒤집힙니다. 이와 같은 방법으로 모든 카드를 화면에서 사라지게 하면 게임이 종료됩니다.

게임에서 카드를 선택하는 방법은 다음과 같습니다.

* 카드는 `커서`를 이용해서 선택할 수 있습니다.
  * 커서는 4 x 4 화면에서 유저가 선택한 현재 위치를 표시하는 "굵고 빨간 테두리 상자"를 의미합니다.
* 커서는 [Ctrl] 키와 방향키에 의해 이동되며 키 조작법은 다음과 같습니다.
  * 방향키 ←, ↑, ↓, → 중 하나를 누르면, 커서가 누른 키 방향으로 1칸 이동합니다.
  * [Ctrl] 키를 누른 상태에서 방향키 ←, ↑, ↓, → 중 하나를 누르면, 누른 키 방향에 있는 가장 가까운 카드로 한번에 이동합니다.
    * 만약, 해당 방향에 카드가 하나도 없다면 그 방향의 가장 마지막 칸으로 이동합니다.
  * 만약, 누른 키 방향으로 이동 가능한 카드 또는 빈 공간이 없어 이동할 수 없다면 커서는 움직이지 않습니다.
* 커서가 위치한 카드를 뒤집기 위해서는 [Enter] 키를 입력합니다.
  * [Enter] 키를 입력해서 카드를 뒤집었을 때
    * 앞면이 보이는 카드가 1장 뿐이라면 그림을 맞출 수 없으므로 두번째 카드를 뒤집을 때 까지 앞면을 유지합니다.
    * 앞면이 보이는 카드가 2장이 된 경우, 두개의 카드에 그려진 그림이 같으면 해당 카드들이 화면에서 사라지며, 그림이 다르다면 두 카드 모두 뒷면이 보이도록 다시 뒤집힙니다.

"베로니"는 게임 진행 중 카드의 짝을 맞춰 몇 장 제거된 상태에서 카드 앞면의 그림을 알고 있다면, 남은 카드를 모두 제거하는데 필요한 키 조작 횟수의 최솟값을 구해 보려고 합니다. 키 조작 횟수는 방향키와 [Enter] 키를 누르는 동작을 각각 조작 횟수 `1`로 계산하며, [Ctrl] 키와 방향키를 함께 누르는 동작 또한 조작 횟수 `1`로 계산합니다.

다음은 카드가 몇 장 제거된 상태의 게임 화면에서 커서를 이동하는 예시입니다.  
아래 그림에서 빈 칸은 이미 카드가 제거되어 없어진 칸을 의미하며, 그림이 그려진 칸은 카드 앞 면에 그려진 그림을 나타냅니다.

<img width="420" alt="예시1" src="https://user-images.githubusercontent.com/77680436/117810049-e5330a80-b299-11eb-8f22-a3270fa70cd5.png">

예시에서 커서는 두번째 행, 첫번째 열 위치에서 시작하였습니다.

<img width="420" alt="예시2" src="https://user-images.githubusercontent.com/77680436/117810175-04ca3300-b29a-11eb-87de-3ccb6d85f8f6.png">

[Enter] 입력, ↓ 이동, [Ctrl]+→ 이동, [Enter] 입력 = 키 조작 4회

<img width="420" alt="예시3" src="https://user-images.githubusercontent.com/77680436/117810258-1d3a4d80-b29a-11eb-876d-689a1cad7537.png">

[Ctrl]+↑ 이동, [Enter] 입력, [Ctrl]+← 이동, [Ctrl]+↓ 이동, [Enter] 입력 = 키 조작 5회

<img width="420" alt="예시4" src="https://user-images.githubusercontent.com/77680436/117810329-35aa6800-b29a-11eb-9c71-7ca5c9962cb7.png">

[Ctrl]+→ 이동, [Enter] 입력, [Ctrl]+↑ 이동, [Ctrl]+← 이동, [Enter] 입력 = 키 조작 5회

위와 같은 방법으로 커서를 이동하여 카드를 선택하고 그림을 맞추어 카드를 모두 제거하기 위해서는 총 14번(방향 이동 8번, [Enter] 키 입력 6번)의 키 조작 횟수가 필요합니다.

___

### [문제]
현재 카드가 놓인 상태를 나타내는 2차원 배열 board와 커서의 처음 위치 r, c가 매개변수로 주어질 때, 모든 카드를 제거하기 위한 키 조작 횟수의 최솟값을 return 하도록 solution 함수를 완성해 주세요.

### [제한사항]
* board는 4 x 4 크기의 2차원 배열입니다.
* board 배열의 각 원소는 0 이상 6 이하인 자연수입니다.
  * 0은 카드가 제거된 빈 칸을 나타냅니다.
  * 1 부터 6까지의 자연수는 2개씩 들어있으며 같은 숫자는 같은 그림의 카드를 의미합니다.
  * 뒤집을 카드가 없는 경우(board의 모든 원소가 0인 경우)는 입력으로 주어지지 않습니다.
* r은 커서의 최초 세로(행) 위치를 의미합니다.
* c는 커서의 최초 가로(열) 위치를 의미합니다.
* r과 c는 0 이상 3 이하인 정수입니다.
* 게임 화면의 좌측 상단이 (0, 0), 우측 하단이 (3, 3) 입니다.

___

### [입출력 예]
|board|r|c|result|
|:---|:---|:---|:---|
|[[1,0,0,3],[2,0,0,0],[0,0,0,2],[3,0,1,0]]|1|0|14|
|[[3,0,0,2],[0,0,1,0],[0,1,0,0],[2,0,0,3]]|0|1|16|

___

### 입출력 예에 대한 설명
**입출력 예 #1**

문제의 예시와 같습니다.

**입출력 예 #2**

입력으로 주어진 게임 화면은 아래 그림과 같습니다.

<img width="420" alt="예시5" src="https://user-images.githubusercontent.com/77680436/117810873-e0228b00-b29a-11eb-8649-3afdd4e52ea5.png">

위 게임 화면에서 모든 카드를 제거하기 위한 키 조작 횟수의 최솟값은 16번 입니다.

## 제출답안
```python
from collections import deque
from itertools import permutations
def move(r, c, direction, board, ctrl): # 이동한 결과 좌표 반환
    if not ctrl: # Ctrl을 누르지 않은 경우
        if direction == 0: return (r-1, c) # 상
        elif direction == 1: return (r+1, c) # 하
        elif direction == 2: return (r, c-1) # 좌
        else: return (r, c+1) # 우
    else: # Ctrl을 누른 경우
        if direction == 0: # 상
            r -= 1
            while r > 0 and not board[r][c]: r -= 1
        elif direction == 1: # 하
            r += 1
            while r < 3 and not board[r][c]: r += 1
        elif direction == 2: # 좌
            c -= 1
            while c > 0 and not board[r][c]: c -= 1
        else: # 우
            c += 1
            while c < 3 and not board[r][c]: c += 1
    return (r, c)

def rangeCheck(r, c): # 좌표가 게임판의 범위 내인지 확인
    return r >= 0 and r < 4 and c >= 0 and c < 4

def bfs(r, c, board, target, first): # 현 위치에서 목표하는 카드로 최단 비용으로 이동하는 경우를 
    visited = {(r, c)}
    queue = deque([(r, c, 0)])

    while queue:
        _r, _c, count = queue.popleft()

        if board[_r][_c] == target:
            if (not first and (r, c) != (_r, _c)) or first:
                return (_r, _c, count)

        for direction in range(4):
            mr, mc = move(_r, _c, direction, board, False)
            if rangeCheck(mr, mc) and (mr, mc) not in visited:
                visited.add((mr, mc))
                queue.append((mr, mc, count+1))
            mr, mc = move(_r, _c, direction, board, True)
            if rangeCheck(mr, mc) and (mr, mc) not in visited:
                visited.add((mr, mc))
                queue.append((mr, mc, count+1))

def solution(board, r, c):
    card = dict()

    for x in range(4):
        for y in range(4):
            if board[y][x]:
                if board[y][x] in card: card[board[y][x]].append((x, y))
                else: card[board[y][x]] = [(x, y)]

    minCount = 9999999
    for order in permutations(card.keys(), len(card.keys())): # 카드 제거 순서의 모든 경우의 수 탐색
        count = 0
        _r, _c = r, c
        for num in order: # 현재 순서에 따라 카드 제거
            _r, _c, cnt = bfs(_r, _c, board, num, True) # 카드를 처음 선택하는 경우
            count += cnt + 1 # 카드로 이동하는 조작횟수 + Enter
            _r, _c, cnt = bfs(_r, _c, board, num, False) # 현재 카드와 같은 두 번째 카드를 선택하는 경우
            count += cnt + 1

            for x, y in card[num]: board[y][x] = 0 # 현재 순서의 카드를 게임판에서 제거

        if minCount > count: minCount = count # 최소 조작 횟수 갱신

        for num in card: # 한 번의 순서를 모두 탐색한 후 게임판 다시 리셋
            for x, y in card[num]: board[y][x] = num

    return minCount
```
### 설명
**반례가 존재할 수도 있는 답안입니다. 더 정확한 답안은 개선사항을 확인해주시길 바랍니다.**

본 문제의 경우 처음에는 단순히 그리디하게 접근하여 문제를 해결하려고 시도했었다.
즉, BFS를 통해서 현재 커서와 제일 가까운 카드로 이동해 선택하고, 다시 BFS를 통해서 현재 선택한 카드의 두 번째 카드로 이동해 선택하는 방법을 반복하는 방법을 사용하였다.

하지만 이러한 접근은 통과하지 못 하였고, 접근 방법을 수정해야겠다는 생각이 들었다. 그러다가 제한사항을 생각해보니 경우의 수가 생각보다 적다는 것을 보고 완전탐색을 생각하게 되었다. 
게임판도 `4 × 4`로 고정되어 있을 뿐만 아니라 카드도 최대 6쌍 밖에 존재하지 않기 때문이다.

그래서 다음과 같이 접근을 수정하였다.
1. 순열함수를 이용해 카드를 제거하는 모든 순서를 얻는다.
2. 순서에 따라 커서와 가장 가까운 현재 차례의 카드를 BFS로 찾아 선택한다.
3. 현재 차례의 다른 카드를 BFS로 찾아 선택한다.
4. 현재 순서대로 모든 카드를 제거할 때까지 2번과 3번을 반복한다.
5. 하나의 순서가 끝나면 게임판과 좌표를 초기화한 후 다른 순서에서도 2번부터 4번을 반복한다.
6. 위의 과정을 통하여 최소 조작 횟수를 얻는다.

**(틀릴 수도 있는 접근입니다. 자세한 내용은 개선사항을 참고하여 주시길 바랍니다.)**

위와 같은 알고리즘을 답안과 같이 구현하여 제출한 결과 검사를 통과할 수 있었다.

## 개선사항
제출한 답안의 알고리즘에서는 카드를 제거하는 순서의 경우 모든 경우의 수를 고려한 완전 탐색이지만, 하나의 순서 안에서 카드를 선택하는 경우는 그리디하게 진행하였다.

즉, 2번 카드가 `(1, 1)`과 `(2, 3)` 위치 두 곳에 있다면 `(1, 1)`을 먼저 선택할지, `(2, 3)`을 먼저 선택할지는 현재 커서와 가까운 것부터 선택하도록 진행하였다.

하지만 카카오의 해설을 보았을 때 이것까지 모든 경우를 고려해준 것을 보았다. 즉 `(1, 1) → (2, 3)`과 `(2, 3) → (1, 1)`, 이렇게 순서 안에서 카드를 선택하는 경우 역시 모든 경우를 탐색한 것이다.

사실 나의 방법이 반례가 존재하는지는 검증하지 않았으나, 처음 문제를 풀 때 그리디하게 풀었다가 실패한 것을 보면 충분히 반례가 존재할 수도 있다고 생각이 되긴 하였다.

그래서 정확히 제출답안의 코드가 틀렸는지는 모르겠지만 그래도 더 정확하다고 할 수 있는 풀이로도 다시 풀이하였다.
```python
from collections import deque
from itertools import permutations
def move(r, c, direction, board, ctrl):
    if not ctrl:
        if direction == 0: return (r-1, c) # 상
        elif direction == 1: return (r+1, c) # 하
        elif direction == 2: return (r, c-1) # 좌
        else: return (r, c+1) # 우
    else:
        if direction == 0: # 상
            r -= 1
            while r > 0 and not board[r][c]: r -= 1
        elif direction == 1: # 하
            r += 1
            while r < 3 and not board[r][c]: r += 1
        elif direction == 2: # 좌
            c -= 1
            while c > 0 and not board[r][c]: c -= 1
        else: # 우
            c += 1
            while c < 3 and not board[r][c]: c += 1
    return (r, c)
            
def rangeCheck(r, c):
    return r >= 0 and r < 4 and c >= 0 and c < 4

def bfs(start, board, target):
    visited = {start}
    queue = deque([(start[0], start[1], 0)])
    
    while queue:
        _r, _c, count = queue.popleft()
        
        if (_r, _c) == target: return count
        
        for direction in range(4):
            mr, mc = move(_r, _c, direction, board, False)
            if rangeCheck(mr, mc) and (mr, mc) not in visited:
                visited.add((mr, mc))
                queue.append((mr, mc, count+1))
            mr, mc = move(_r, _c, direction, board, True)
            if rangeCheck(mr, mc) and (mr, mc) not in visited:
                visited.add((mr, mc))
                queue.append((mr, mc, count+1))

def solution(board, r, c):
    card = dict()
    
    for x in range(4):
        for y in range(4):
            if board[y][x]:
                if board[y][x] in card: card[board[y][x]].append((y, x))
                else: card[board[y][x]] = [(y, x)]
    
    minCount = 9999999
    for order in permutations(card.keys(), len(card.keys())):
        count = 0
        _r, _c = r, c
        for num in order:
            cnt1 = bfs((_r, _c), board, card[num][0])
            cnt1 += bfs(card[num][0], board, card[num][1])
            cnt2 = bfs((_r, _c), board, card[num][1])
            cnt2 += bfs(card[num][1], board, card[num][0])
            if cnt1 < cnt2: # 카드를 선택하는 과정 역시 완전 탐색하여 비교
                count += cnt1 + 2
                _r, _c = card[num][1]
            else:
                count += cnt2 + 2
                _r, _c = card[num][0]
            
            for y, x in card[num]: board[y][x] = 0
                
        if minCount > count: minCount = count
            
        for num in card:
            for y, x in card[num]: board[y][x] = num
    
    return minCount
```
