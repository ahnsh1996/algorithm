# [2019 KAKAO BLIND RECRUITMENT] 길 찾기 게임
## 문제
### 문제 설명
([원본 문제](https://programmers.co.kr/learn/courses/30/lessons/42892)의 내용을 복사한 것입니다.)

### 길 찾기 게임

전무로 승진한 라이언은 기분이 너무 좋아 프렌즈를 이끌고 특별 휴가를 가기로 했다.
내친김에 여행 계획까지 구상하던 라이언은 재미있는 게임을 생각해냈고 역시 전무로 승진할만한 인재라고 스스로에게 감탄했다.

라이언이 구상한(그리고 아마도 라이언만 즐거울만한) 게임은, 카카오 프렌즈를 두 팀으로 나누고, 각 팀이 같은 곳을 다른 순서로 방문하도록 해서 먼저 순회를 마친 팀이 승리하는 것이다.

그냥 지도를 주고 게임을 시작하면 재미가 덜해지므로, 라이언은 방문할 곳의 2차원 좌표 값을 구하고 각 장소를 이진트리의 노드가 되도록 구성한 후, 순회 방법을 힌트로 주어 각 팀이 스스로 경로를 찾도록 할 계획이다.

라이언은 아래와 같은 특별한 규칙으로 트리 노드들을 구성한다.

* 트리를 구성하는 모든 노드의 x, y 좌표 값은 정수이다.
* 모든 노드는 서로 다른 x값을 가진다.
* 같은 레벨(level)에 있는 노드는 같은 y 좌표를 가진다.
* 자식 노드의 y 값은 항상 부모 노드보다 작다.
* 임의의 노드 V의 왼쪽 서브 트리(left subtree)에 있는 모든 노드의 x값은 V의 x값보다 작다.
* 임의의 노드 V의 오른쪽 서브 트리(right subtree)에 있는 모든 노드의 x값은 V의 x값보다 크다.

아래 예시를 확인해보자.

라이언의 규칙에 맞게 이진트리의 노드만 좌표 평면에 그리면 다음과 같다. (이진트리의 각 노드에는 1부터 N까지 순서대로 번호가 붙어있다.)

![예시1](https://user-images.githubusercontent.com/77680436/118399237-30348f80-b697-11eb-80dd-e0d02db48a07.png)

이제, 노드를 잇는 간선(edge)을 모두 그리면 아래와 같은 모양이 된다.

![예시2](https://user-images.githubusercontent.com/77680436/118399255-43dff600-b697-11eb-9efd-d4710440f86e.png)

위 이진트리에서 전위 순회(preorder), 후위 순회(postorder)를 한 결과는 다음과 같고, 이것은 각 팀이 방문해야 할 순서를 의미한다.

* 전위 순회 : 7, 4, 6, 9, 1, 8, 5, 2, 3
* 후위 순회 : 9, 6, 5, 8, 1, 4, 3, 2, 7

다행히 두 팀 모두 머리를 모아 분석한 끝에 라이언의 의도를 간신히 알아차렸다.

그러나 여전히 문제는 남아있다. 노드의 수가 예시처럼 적다면 쉽게 해결할 수 있겠지만, 예상대로 라이언은 그렇게 할 생각이 전혀 없었다.

이제 당신이 나설 때가 되었다.

곤경에 빠진 카카오 프렌즈를 위해 이진트리를 구성하는 노드들의 좌표가 담긴 배열 nodeinfo가 매개변수로 주어질 때,  
노드들로 구성된 이진트리를 전위 순회, 후위 순회한 결과를 2차원 배열에 순서대로 담아 return 하도록 solution 함수를 완성하자.

### 제한사항
* nodeinfo는 이진트리를 구성하는 각 노드의 좌표가 1번 노드부터 순서대로 들어있는 2차원 배열이다.
* nodeinfo의 길이는 `1` 이상 `10,000` 이하이다.
* nodeinfo[i] 는 i + 1번 노드의 좌표이며, [x축 좌표, y축 좌표] 순으로 들어있다.
* 모든 노드의 좌표 값은 `0` 이상 `100,000` 이하인 정수이다.
* 트리의 깊이가 `1,000` 이하인 경우만 입력으로 주어진다.
* 모든 노드의 좌표는 문제에 주어진 규칙을 따르며, 잘못된 노드 위치가 주어지는 경우는 없다.

___

### 입출력 예
|nodeinfo|result|
|:---|:---|
|[[5,3],[11,5],[13,3],[3,5],[6,1],[1,3],[8,6],[7,2],[2,2]]|[[7,4,6,9,1,8,5,2,3],[9,6,5,8,1,4,3,2,7]]|

### 입출력 예 설명
입출력 예 #1

문제에 주어진 예시와 같다.

## 제출답안
```python
import sys
sys.setrecursionlimit(1005) # 재귀의 제한을 늘려줌

class Node: # 트리의 노드 클래스
    def __init__(self, number, value):
        self.number = number # 노드의 번호
        self.value = value # 노드의 x 좌표
        self.left = self.right = None

class BST: # 이진 탐색 트리 클래스
    def __init__(self):
        self.root = None

    def insert(self, number, value): # 트리에 삽입하는 함수
        if not self.root:
            self.root = Node(number, value)
        else:
            p = self.root
            while True:
                if p.value > value:
                    if p.left: p = p.left
                    else:
                        p.left = Node(number, value)
                        break
                else:
                    if p.right: p = p.right
                    else:
                        p.right = Node(number, value)
                        break

    def preorder(self, node, result): # 전위 순회
        if node:
            result.append(node.number)
            self.preorder(node.left, result)
            self.preorder(node.right, result)

    def postorder(self, node, result): # 후위 순회
        if node:
            self.postorder(node.left, result)
            self.postorder(node.right, result)
            result.append(node.number)

def solution(nodeinfo):
    nodes = sorted(enumerate(nodeinfo), key=lambda x: -x[1][1]) # 노드를 y 좌표에 대해 내림차순 정렬

    bst = BST() # 이진 탐색 트리 생성
    for node in nodes: # 트리에 y좌표가 큰 노드부터 삽입
        bst.insert(node[0]+1, node[1][0])

    pre, post = [], []
    bst.preorder(bst.root, pre) # 전위 순회
    bst.postorder(bst.root, post) # 후위 순회

    return [pre, post]
```
### 설명
트리의 순회 같은 경우는 워낙 유명하기 때문에 사실상 문제로서의 의미가 없다. 본 문제에서 가장 핵심이 되는 것은 트리를 어떻게 구성할 것인가이다.

본 답안에서 사용한 방법은 간단하다. 각 노드를 y 좌표에 대해서 내림차순 정렬하고 y 좌표가 큰 순서대로 이진 탐색 트리의 삽입 알고리즘에 따라 트리에 삽입하는 것이다.

문제에서 제시하고 있는 트리의 구성 규칙은 사실상 이진 탐색 트리의 규칙이다. 따라서 이진 탐색 트리의 알고리즘을 사용한다면 문제에서 원하는 트리를 구성할 수 있다.

이진 탐색 트리의 삽입 알고리즘을 사용하더라도 아무런 순서없이 트리에 삽입하면 의도대로 트리를 만들 수 없다. 라이언이 의도했던 트리를 구성하기 위해서는 
트리의 레벨 순서대로 이진 탐색 트리에 노드를 삽입해야 한다. 이는 y 좌표가 큰 순서대로 삽입해야 한다는 의미이다.

따라서 y 좌표의 내림차순에 따라 이진 탐색 트리에 삽입하고 이를 전위 순회, 후위 순회하면 정답을 도출할 수 있다.

하지만 그냥 이대로 제출하게 되면 런타임 에러라는 결과를 몇 개 볼 수 있다. 아무리 생각해도 맞다고 생각해서 프로그래머스의 질문하기 란을 보니 같은 문제를 겪었던 사람이 많았다. 
문제의 원인은 런타임 에러가 나는 테스트 케이스의 트리를 순회하는 재귀 호출이 파이썬의 기본 재귀 제한을 초과하기 때문이었다. 이를 해결하기 위해서 재귀가 아닌 스택을 이용하여 풀이하거나 
다음과 같이 재귀 제한을 늘려주면 해결된다.
```python
import sys
sys.setrecursionlimit(1005)
```
