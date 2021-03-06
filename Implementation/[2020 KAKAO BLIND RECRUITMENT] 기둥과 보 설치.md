# [2020 KAKAO BLIND RECRUITMENT] 기둥과 보 설치
## 문제
### 문제 설명
([원본 문제](https://programmers.co.kr/learn/courses/30/lessons/60061)의 내용을 복사한 것입니다.)

빙하가 깨지면서 스노우타운에 떠내려 온 <b>"죠르디"</b>는 인생 2막을 위해 주택 건축사업에 뛰어들기로 결심하였습니다. "죠르디"는 <b>기둥과 보</b>를 이용하여 벽면 구조물을 자동으로 세우는 로봇을 개발할 계획인데, 그에 앞서 로봇의 동작을 시뮬레이션 할 수 있는 프로그램을 만들고 있습니다.
프로그램은 <b>2차원 가상 벽면</b>에 기둥과 보를 이용한 구조물을 설치할 수 있는데, 기둥과 보는 <b>길이가 1인 선분</b>으로 표현되며 다음과 같은 규칙을 가지고 있습니다.

* 기둥은 바닥 위에 있거나 보의 한쪽 끝 부분 위에 있거나, 또는 다른 기둥 위에 있어야 합니다.
* 보는 한쪽 끝 부분이 기둥 위에 있거나, 또는 양쪽 끝 부분이 다른 보와 동시에 연결되어 있어야 합니다.

단, 바닥은 벽면의 맨 아래 지면을 말합니다.

2차원 벽면은 `n x n` 크기 정사각 격자 형태이며, 각 격자는 `1 x 1` 크기입니다. 맨 처음 벽면은 비어있는 상태입니다. 기둥과 보는 격자선의 교차점에 걸치지 않고, 격자 칸의 각 변에 정확히 일치하도록 설치할 수 있습니다. 다음은 기둥과 보를 설치해 구조물을 만든 예시입니다.

<img width="420" alt="예시" src="https://user-images.githubusercontent.com/77680436/117630083-cbb89280-b1b5-11eb-8e25-8b38b342a96a.png">

예를 들어, 위 그림은 다음 순서에 따라 구조물을 만들었습니다.

* (1, 0)에서 위쪽으로 기둥을 하나 설치 후, (1, 1)에서 오른쪽으로 보를 하나 만듭니다.
* (2, 1)에서 위쪽으로 기둥을 하나 설치 후, (2, 2)에서 오른쪽으로 보를 하나 만듭니다.
* (5, 0)에서 위쪽으로 기둥을 하나 설치 후, (5, 1)에서 위쪽으로 기둥을 하나 더 설치합니다.
* (4, 2)에서 오른쪽으로 보를 설치 후, (3, 2)에서 오른쪽으로 보를 설치합니다.

만약 (4, 2)에서 오른쪽으로 보를 먼저 설치하지 않고, (3, 2)에서 오른쪽으로 보를 설치하려 한다면 2번 규칙에 맞지 않으므로 설치가 되지 않습니다. 기둥과 보를 삭제하는 기능도 있는데 기둥과 보를 삭제한 후에 남은 기둥과 보들 또한 위 규칙을 만족해야 합니다. 만약, 작업을 수행한 결과가 조건을 만족하지 않는다면 해당 작업은 무시됩니다.

벽면의 크기 n, 기둥과 보를 설치하거나 삭제하는 작업이 순서대로 담긴 2차원 배열 build_frame이 매개변수로 주어질 때, 모든 명령어를 수행한 후 구조물의 상태를 return 하도록 solution 함수를 완성해주세요.

### 제한사항
* n은 5 이상 100 이하인 자연수입니다.
* build_frame의 세로(행) 길이는 1 이상 1,000 이하입니다.
* build_frame의 가로(열) 길이는 4입니다.
* build_frame의 원소는 [x, y, a, b]형태입니다.
  * x, y는 기둥, 보를 설치 또는 삭제할 교차점의 좌표이며, [가로 좌표, 세로 좌표] 형태입니다.
  * a는 설치 또는 삭제할 구조물의 종류를 나타내며, 0은 기둥, 1은 보를 나타냅니다.
  * b는 구조물을 설치할 지, 혹은 삭제할 지를 나타내며 0은 삭제, 1은 설치를 나타냅니다.
  * 벽면을 벗어나게 기둥, 보를 설치하는 경우는 없습니다.
  * 바닥에 보를 설치 하는 경우는 없습니다.
* 구조물은 교차점 좌표를 기준으로 보는 오른쪽, 기둥은 위쪽 방향으로 설치 또는 삭제합니다.
* 구조물이 겹치도록 설치하는 경우와, 없는 구조물을 삭제하는 경우는 입력으로 주어지지 않습니다.
* 최종 구조물의 상태는 아래 규칙에 맞춰 return 해주세요.
  * return 하는 배열은 가로(열) 길이가 3인 2차원 배열로, 각 구조물의 좌표를 담고있어야 합니다.
  * return 하는 배열의 원소는 [x, y, a] 형식입니다.
  * x, y는 기둥, 보의 교차점 좌표이며, [가로 좌표, 세로 좌표] 형태입니다.
  * 기둥, 보는 교차점 좌표를 기준으로 오른쪽, 또는 위쪽 방향으로 설치되어 있음을 나타냅니다.
  * a는 구조물의 종류를 나타내며, 0은 기둥, 1은 보를 나타냅니다.
  * return 하는 배열은 x좌표 기준으로 오름차순 정렬하며, x좌표가 같을 경우 y좌표 기준으로 오름차순 정렬해주세요.
  * x, y좌표가 모두 같은 경우 기둥이 보보다 앞에 오면 됩니다.

### 입출력 예
|n|build_frame|result|
|:---|:---|:---|
|5|[[1,0,0,1],[1,1,1,1],[2,1,0,1],[2,2,1,1],[5,0,0,1],[5,1,0,1],[4,2,1,1],[3,2,1,1]]|[[1,0,0],[1,1,1],[2,1,0],[2,2,1],[3,2,1],[4,2,1],[5,0,0],[5,1,0]]|
|5|[[0,0,0,1],[2,0,0,1],[4,0,0,1],[0,1,1,1],[1,1,1,1],[2,1,1,1],[3,1,1,1],[2,0,0,0],[1,1,1,0],[2,2,0,1]]|[[0,0,0],[0,1,1],[1,1,1],[2,1,1],[3,1,1],[4,0,0]]|

### 입출력 예에 대한 설명
**입출력 예 #1**

문제의 예시와 같습니다.

**입출력 예 #2**

여덟 번째 작업을 수행 후 아래와 같은 구조물 만들어집니다.

<img width="420" alt="예시2" src="https://user-images.githubusercontent.com/77680436/117631332-171f7080-b1b7-11eb-8949-21f4f0f7506e.png">

아홉 번째 작업의 경우, (1, 1)에서 오른쪽에 있는 보를 삭제하면 (2, 1)에서 오른쪽에 있는 보는 조건을 만족하지 않으므로 무시됩니다.

열 번째 작업의 경우, (2, 2)에서 위쪽 방향으로 기둥을 세울 경우 조건을 만족하지 않으므로 무시됩니다.

## 제출답안
```python
def buildable(x, y, column, beam, buildType): # 구조물이 설치 가능한지 판단하는 함수
    if buildType: # 보
        if (x, y-1) in column or (x+1, y-1) in column: return True # 기둥 위일 때
        if (x-1, y) in beam and (x+1, y) in beam: return True # 양쪽 끝이 다른 보일 때
        
    else: # 기둥
        if y == 0: return True # 바닥 위일 때
        if (x, y-1) in column: return True # 다른 기둥 위일 때
        if (x, y) in beam or (x-1, y) in beam: return True # 보의 끝일 때
    
    return False

def solution(n, build_frame):
    column, beam = set(), set() # 기둥, 보의 좌표
    
    for build in build_frame:
        x, y = build[0], build[1]
        buildType = build[2]
        if build[3]: # 현재 작업이 설치인 경우
            if buildable(x, y, column, beam, buildType): # 설치 가능하면 설치
                if buildType: beam.add((x, y))
                else: column.add((x, y))
        else: # 현재 작업이 삭제인 경우
            toCheck = None # 삭제로 인해 영향을 끼치는 건축물
            _type = beam if buildType else column
            _type.remove((x, y)) # 주변 영향을 생각하지 않고 일단 주어진 작업대로 건축물 삭제
            if buildType: # 보
                toCheck = [(x, y, 0), (x+1, y, 0), (x-1, y, 1), (x+1, y, 1)]
            else: # 기둥
                toCheck = [(x, y+1, 0), (x-1, y+1, 1), (x, y+1, 1)]
            for cx, cy, ct in toCheck: # 건축물을 삭제한 이후 영향을 끼치는 주변 건출물이 설치 가능한지를 확인
                checkType = beam if ct else column
                if (cx, cy) in checkType and not buildable(cx, cy, column, beam, ct): # 불가능하다면 작업 취소
                    _type.add((x, y))
                    break
                
    answer = [[x, y, 0] for x, y in column] + [[x, y, 1] for x, y in beam]
    
    return sorted(answer)
```
### 설명
본 문제의 경우 문제에서 제시하고 있는 시뮬레이션 구현하는 문제이다.

이를 위해서 먼저 주어진 규칙을 생각해보자.

* 기둥은 바닥 위에 있거나 보의 한쪽 끝 부분 위에 있거나, 또는 다른 기둥 위에 있어야 합니다.
* 보는 한쪽 끝 부분이 기둥 위에 있거나, 또는 양쪽 끝 부분이 다른 보와 동시에 연결되어 있어야 합니다.

주어진 규칙은 기둥과 보의 설치 가능 여부를 판단하는 기준이 된다.
따라서 이를 이용하여 아래와 같이 설치가 가능한지 확인하는 함수를 만드는 것은 어렵지 않다.
```python
def buildable(x, y, column, beam, buildType): # 구조물이 설치 가능한지 판단하는 함수
    if buildType: # 보
        if (x, y-1) in column or (x+1, y-1) in column: return True # 기둥 위일 때
        if (x-1, y) in beam and (x+1, y) in beam: return True # 양쪽 끝이 다른 보일 때
        
    else: # 기둥
        if y == 0: return True # 바닥 위일 때
        if (x, y-1) in column: return True # 다른 기둥 위일 때
        if (x, y) in beam or (x-1, y) in beam: return True # 보의 끝일 때
    
    return False
```

하지만 삭제가 가능한지를 판단하는 함수의 구현은 생각보다 까다로워질 수 있다. 자신의 삭제로 인해 주변의 다른 건축물이 주어진 규칙을 만족하지 못 할 수 있기 때문이다.

여기서 생각을 조금만 달리하면 굳이 삭제 가능 여부를 판단하는 함수를 구현하지 않고도 문제를 해결할 수 있다.  
이는 일단 작업대로 건축물을 삭제하고 주변에 영향을 끼칠 수 있는 건축물에 대해 다시 한 번 설치가 가능한지를 판단하는 방법이다.  
만약 주변의 모든 건축물이 삭제하려는 건축물을 삭제하고 나서도 모두 설치가 가능하다면 이는 주어진 규칙을 만족한다는 것을 의미하므로 삭제할 수 있다는 것이다. 
반대로 하나라도 설치가 불가능 하다면 주어진 규칙을 만족하지 않는 작업이므로 이는 무시하면 된다.

그렇다면 영향을 끼칠 수 있는 것들은 어떤 것이 있을까. (하단을 읽기 전에 참고로 기둥은 기둥의 밑 부분 좌표를, 보는 보의 왼쪽 부분 좌표를 기준으로 저장한다.)

먼저 기둥을 삭제하려는 경우이다.  
주어진 규칙을 살펴보면 어떠한 건축물을 설치하려고 할 때 <b>기둥</b>에 의해 규칙이 만족되는 경우는 다음과 같다.
1. <ins>기둥</ins>을 설치하려고 할 때, 다른 <b>기둥</b> 위에 있는 경우
2. <ins>보</ins>를 설치하려고 할 때, 한쪽 끝 부분이 <b>기둥</b> 위에 있는 경우

1번의 경우 현재 기둥을 삭제함에 따라 자신의 위에 있던 기둥이 규칙을 만족하지 못 할 수 있다. 즉 삭제하는 건축물의 좌표가 (x, y)일 때, `(x, y+1)에 있는 기둥`에 영향을 끼칠 수 있다.

2번의 경우 현재 기둥을 삭제함에 따라 자신을 한쪽 끝 부분으로 삼아 설치하였던 보가 규칙을 만족하지 못 할 수 있다. 즉 `(x-1, y+1), (x, y+1)에 있는 보`에 영향을 끼칠 수 있다.

따라서 기둥을 삭제할 때에는 `(x, y+1, 기둥), (x-1, y+1, 보), (x, y+1, 보)`에 대해 다시 설치 가능한지 확인해주면 된다.

이번에는 보를 삭제하려는 경우이다.
주어진 규칙을 살펴보면 어떠한 건축물을 설치하려고 할 때 <b>보</b>에 의해 규칙이 만족되는 경우는 다음과 같다.
1. <ins>기둥</ins>을 설치하려고 할 때, <b>보</b>의 끝 부분 위에 있는 경우
2. <ins>보</ins>를 설치하려고 할 때, 양쪽 끝 부분이 다른 <b>보</b>와 동시에 연결되어 있는 경우

1번의 경우 현재 보를 삭제함에 따라 자신의 양쪽 끝 부분에 있던 기둥이 규칙을 만족하지 못 할 수 있다. 즉 `(x, y), (x+1, y)에 있는 기둥`에 영향을 끼칠 수 있다.

2번의 경우 현재 보를 삭제함에 따라 자신의 양쪽 끝 부분에 있던 보가 규칙을 만족하지 못 할 수 있다. 즉 `(x-1, y), (x+1, y)에 있는 보`에 영향을 끼칠 수 있다.

따라서 보를 삭제할 때에는 `(x, y, 기둥), (x+1, y, 기둥), (x-1, y, 보), (x+1, y, 보)`에 대해 다시 설치 가능한지 확인해주면 된다.

이처럼 설치 가능 여부를 판단하는 함수만을 통해 삭제 여부까지 판단할 수 있으며, 이를 이용해 시뮬레이션을 하고나면 나머지는 정렬하여 반환해주면 된다.
