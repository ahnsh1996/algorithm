# [2018 KAKAO BLIND RECRUITMENT 1차] 프렌즈4블록
## 문제
### 문제 설명
([원본 문제](https://programmers.co.kr/learn/courses/30/lessons/17679)의 내용을 복사한 것입니다.)

### 프렌즈4블록

블라인드 공채를 통과한 신입 사원 라이언은 신규 게임 개발 업무를 맡게 되었다. 이번에 출시할 게임 제목은 "프렌즈4블록".  
같은 모양의 카카오프렌즈 블록이 2×2 형태로 4개가 붙어있을 경우 사라지면서 점수를 얻는 게임이다.

![예시1](https://user-images.githubusercontent.com/77680436/112022675-dcfdff00-8b75-11eb-92a6-dffd09d49b4c.png)

만약 판이 위와 같이 주어질 경우, 라이언이 2×2로 배치된 7개 블록과 콘이 2×2로 배치된 4개 블록이 지워진다. 같은 블록은 여러 2×2에 포함될 수 있으며, 지워지는 조건에 만족하는 2×2 모양이 여러 개 있다면 한꺼번에 지워진다.

![예시2](https://user-images.githubusercontent.com/77680436/112022768-f1da9280-8b75-11eb-982c-543f54063567.png)

블록이 지워진 후에 위에 있는 블록이 아래로 떨어져 빈 공간을 채우게 된다.

![예시3](https://user-images.githubusercontent.com/77680436/112022828-fd2dbe00-8b75-11eb-9831-99d93f51ab4b.png)

만약 빈 공간을 채운 후에 다시 2×2 형태로 같은 모양의 블록이 모이면 다시 지워지고 떨어지고를 반복하게 된다.

![예시4](https://user-images.githubusercontent.com/77680436/112022894-0cad0700-8b76-11eb-8b29-de791b85cd27.png)

위 초기 배치를 문자로 표시하면 아래와 같다.

```
TTTANT
RRFACC
RRRFCC
TRRRAA
TTMMMF
TMMTTJ
```

각 문자는 라이언(R), 무지(M), 어피치(A), 프로도(F), 네오(N), 튜브(T), 제이지(J), 콘(C)을 의미한다

입력으로 블록의 첫 배치가 주어졌을 때, 지워지는 블록은 모두 몇 개인지 판단하는 프로그램을 제작하라.

### 입력 형식
* 입력으로 판의 높이 ```m```, 폭 ```n```과 판의 배치 정보 ```board```가 들어온다.
* 2 ≦ ```n```, ```m``` ≦ 30
* ```board```는 길이 ```n```인 문자열 ```m```개의 배열로 주어진다. 블록을 나타내는 문자는 대문자 A에서 Z가 사용된다.

### 출력 형식
입력으로 주어진 판 정보를 가지고 몇 개의 블록이 지워질지 출력하라.

### 입출력 예제
|m|n|board|answer|
|:---|:---|:---|:---|
|4|5|["CCBDE", "AAADE", "AAABF", "CCBBF"]|14|
|6|6|["TTTANT", "RRFACC", "RRRFCC", "TRRRAA", "TTMMMF", "TMMTTJ"]|15|

### 예제에 대한 설명
* 입출력 예제 1의 경우, 첫 번째에는 A 블록 6개가 지워지고, 두 번째에는 B 블록 4개와 C 블록 4개가 지워져, 모두 14개의 블록이 지워진다.
* 입출력 예제 2는 본문 설명에 있는 그림을 옮긴 것이다. 11개와 4개의 블록이 차례로 지워지며, 모두 15개의 블록이 지워진다.

## 제출답안
```python
def is4Block(x, y, board): # 현재 좌표를 기준으로 우하단 방향으로 4블록인지 판단하는 함수
    if board[y][x] and board[y][x] == board[y][x+1]:
        if board[y][x] == board[y+1][x]:
            if board[y+1][x] == board[y+1][x+1]:
                return True
    return False

def disappear(x, y, board): # 블록을 게임판에서 사라지게 하는 함수
    board[y][x] = board[y][x+1] = board[y+1][x] = board[y+1][x+1] = 0

def fall(m, n, board): # 빈 공간에 맞게 블록들을 떨어뜨리는 함수
    for x in range(n):
        temp = [board[i][x] for i in range(m) if board[i][x]]
        temp = [0]*(m-len(temp)) + temp
        for y in range(m): board[y][x] = temp[y]

def solution(m, n, board):
    board = [list(board[i]) for i in range(m)] # 게임판
    blocks = set() # 4블록이 된 블록들의 좌상단 좌표

    while True:
        for y in range(m-1):
            for x in range(n-1):
                if is4Block(x, y, board): # 게임판을 스캔하며 4블록을 찾음
                    blocks.add((x, y))

        if not blocks: break # 지워질 블록이 없다면 반복 종료

        while blocks: # 찾은 블록들을 사라지게 함
            block = blocks.pop()
            disappear(block[0], block[1], board)

        fall(m, n, board) # 사라져 생긴 빈 공간에 맞게 블록들을 떨어뜨림

    return sum(board[i].count(0) for i in range(m)) # 빈 공간의 수를 세어 반환(처음엔 빈 공간이 0이었으므로)
```
### 설명
본 문제의 경우 문제에서 문제에서 요구하는 그대로 구현하면 되는 문제이다. 아마도 프로그래밍을 배우다가 게임을 만들어보고자 테트리스와 같은 것들을 만들어보았다면 비교적 익숙하였을 것 같다.

먼저 게임판 매트릭스를 생성한다. 이후 4블록을 스캔하고 제거하는 과정을 더 이상 4블록을 찾을 수 없을 때까지 반복하는 과정을 수행하면 최종 결과가 게임판에 기록되어 있을 것이다.  
처음 게임판엔 빈 공간이 없었기 때문에, 게임판의 빈 공간의 수를 세어본다면 사라진 블록 자리의 수를 얻어낼 수 있다.

구현 문제의 경우 알고리즘을 얼마나 구현하기 어려운가에 따라 난이도가 결정된다. 본 문제의 경우 어려울 수 있는 부분이 스캔하는 부분과 빈 공간에 맞게 블록들을 떨어뜨리는 부분이라고 생각한다.  
사실 스캔하는 부분의 경우 인접한 블록의 수가 몇 개 이상이면 모두 지우는 실제 게임에 비해 본 문제의 경우 2x2 형태의 블록만 비교하면 되기 때문에 비교적 복잡하지 않았을 것이다. 
따라서 그나마 복잡한 부분은 블록들을 떨어뜨리는 부분이다.

```python
def fall(m, n, board):
    for x in range(n):
        temp = [board[i][x] for i in range(m) if board[i][x]]
        temp = [0]*(m-len(temp)) + temp
        for y in range(m): board[y][x] = temp[y]
```

위의 코드가 블록들을 떨어뜨리는 부분이다.  
먼저 temp 리스트의 경우 게임판의 위에서 아래 방향으로 스캔하며 0이 아닌(빈 공간이 아닌) 블록들을 순서대로 저장한다.  
예를 들어 게임판의 첫 번째 세로줄이 다음과 같다고 하자.
```
|T|
|0|
|0|
|T|
|T|
|T|
```
이 경우 위에서부터 순서대로 이러한 과정을 거친다.
```
|T| → temp = [T]
|0| → temp = [T]
|0| → temp = [T]
|T| → temp = [T, T]
|T| → temp = [T, T, T]
|T| → temp = [T, T, T, T]
```
따라서 temp는 ```[T, T, T, T]```가 된다. 이를 세로로 살펴보면 떨어진 이후의 블록이다.

게임판의 나머지만큼 빈공간을 왼쪽에 추가하여 주면 ```[0, 0, T, T, T, T]```가 되며, 이를 세로 방향으로 순차적으로 게임판에 넣어주면 블록의 떨어짐을 구현할 수 있다.

```python
# 블록의 떨어짐 구현 예시2
def fall(m, n, board):
    temp = []
    for x in range(n):
        for y in range(m):
            if board[y][x]: temp.append(board[y][x])
        for y in range(m-1, -1, -1):
            board[y][x] = temp.pop() if temp else 0
```

참고로 위와 같이 temp를 스택처럼 생각하여 아래서부터 pop()을 하여주어도 된다. 또한 temp를 만드는 것은 위나 아래나 같은 방식이지만 예시2에서는 조금 더 직관적으로 나타내보았다.

떨어지는 부분을 위와 같이 구현하였다면, 나머지는 이들을 적용하여 반복문을 만들고 결과에서 빈 공간의 수를 찾아 반환하면 답이 된다.

## 개선사항
제출 후 다른 사람들의 방법을 살펴보던 중 블록들의 떨어짐을 조금 더 최적화하기 위하여 애초부터 게임판을 전치하여 만든 경우도 존재하였다.

이 경우 설명의 temp 부분을 실제 게임판에 적용함에 있어 반복문을 수행하지 않고 대입 연산만으로도 가능하기 때문에 더 빠르게 구현할 수 있다.
