# [2021 KAKAO BLIND RECRUITMENT] 순위 검색
## 문제
### 문제 설명
([원본 문제](https://programmers.co.kr/learn/courses/30/lessons/72412)의 내용을 복사한 것입니다.)

카카오는 하반기 경력 개발자 공개채용을 진행 중에 있으며 현재 지원서 접수와 코딩테스트가 종료되었습니다. 이번 채용에서 지원자는 지원서 작성 시 아래와 같이 4가지 항목을 반드시 선택하도록 하였습니다.

* 코딩테스트 참여 개발언어 항목에 cpp, java, python 중 하나를 선택해야 합니다.
* 지원 직군 항목에 backend와 frontend 중 하나를 선택해야 합니다.
* 지원 경력구분 항목에 junior와 senior 중 하나를 선택해야 합니다.
* 선호하는 소울푸드로 chicken과 pizza 중 하나를 선택해야 합니다.

인재영입팀에 근무하고 있는 `니니즈`는 코딩테스트 결과를 분석하여 채용에 참여한 개발팀들에 제공하기 위해 지원자들의 지원 조건을 선택하면 해당 조건에 맞는 지원자가 몇 명인 지 쉽게 알 수 있는 도구를 만들고 있습니다.  
예를 들어, 개발팀에서 궁금해하는 문의사항은 다음과 같은 형태가 될 수 있습니다.  
`코딩테스트에 java로 참여했으며, backend 직군을 선택했고, junior 경력이면서, 소울푸드로 pizza를 선택한 사람 중 코딩테스트 점수를 50점 이상 받은 지원자는 몇 명인가?`

물론 이 외에도 각 개발팀의 상황에 따라 아래와 같이 다양한 형태의 문의가 있을 수 있습니다.

* 코딩테스트에 python으로 참여했으며, frontend 직군을 선택했고, senior 경력이면서, 소울푸드로 chicken을 선택한 사람 중 코딩테스트 점수를 100점 이상 받은 사람은 모두 몇 명인가?
* 코딩테스트에 cpp로 참여했으며, senior 경력이면서, 소울푸드로 pizza를 선택한 사람 중 코딩테스트 점수를 100점 이상 받은 사람은 모두 몇 명인가?
* backend 직군을 선택했고, senior 경력이면서 코딩테스트 점수를 200점 이상 받은 사람은 모두 몇 명인가?
* 소울푸드로 chicken을 선택한 사람 중 코딩테스트 점수를 250점 이상 받은 사람은 모두 몇 명인가?
* 코딩테스트 점수를 150점 이상 받은 사람은 모두 몇 명인가?

즉, 개발팀에서 궁금해하는 내용은 다음과 같은 형태를 갖습니다.

```
* [조건]을 만족하는 사람 중 코딩테스트 점수를 X점 이상 받은 사람은 모두 몇 명인가?
```

___

### [문제]
지원자가 지원서에 입력한 4가지의 정보와 획득한 코딩테스트 점수를 하나의 문자열로 구성한 값의 배열 info, 개발팀이 궁금해하는 문의조건이 문자열 형태로 담긴 배열 query가 매개변수로 주어질 때,  
각 문의조건에 해당하는 사람들의 숫자를 순서대로 배열에 담아 return 하도록 solution 함수를 완성해 주세요.

### [제한사항]
* info 배열의 크기는 1 이상 50,000 이하입니다.
* info 배열 각 원소의 값은 지원자가 지원서에 입력한 4가지 값과 코딩테스트 점수를 합친 "개발언어 직군 경력 소울푸드 점수" 형식입니다.
  * 개발언어는 cpp, java, python 중 하나입니다.
  * 직군은 backend, frontend 중 하나입니다.
  * 경력은 junior, senior 중 하나입니다.
  * 소울푸드는 chicken, pizza 중 하나입니다.
  * 점수는 코딩테스트 점수를 의미하며, 1 이상 100,000 이하인 자연수입니다.
  * 각 단어는 공백문자(스페이스 바) 하나로 구분되어 있습니다.
* query 배열의 크기는 1 이상 100,000 이하입니다.
* query의 각 문자열은 "[조건] X" 형식입니다.
  * [조건]은 "개발언어 and 직군 and 경력 and 소울푸드" 형식의 문자열입니다.
  * 언어는 cpp, java, python, - 중 하나입니다.
  * 직군은 backend, frontend, - 중 하나입니다.
  * 경력은 junior, senior, - 중 하나입니다.
  * 소울푸드는 chicken, pizza, - 중 하나입니다.
  * '-' 표시는 해당 조건을 고려하지 않겠다는 의미입니다.
  * X는 코딩테스트 점수를 의미하며 조건을 만족하는 사람 중 X점 이상 받은 사람은 모두 몇 명인 지를 의미합니다.
  * 각 단어는 공백문자(스페이스 바) 하나로 구분되어 있습니다.
  * 예를 들면, "cpp and - and senior and pizza 500"은 "cpp로 코딩테스트를 봤으며, 경력은 senior 이면서 소울푸드로 pizza를 선택한 지원자 중 코딩테스트 점수를 500점 이상 받은 사람은 모두 몇 명인가?"를 의미합니다.

___

### [입출력 예]
|info|query|result|
|:---|:---|:---|
|`["java backend junior pizza 150","python frontend senior chicken 210","python frontend senior chicken 150","cpp backend senior pizza 260","java backend junior chicken 80","python backend senior chicken 50"]`|`["java and backend and junior and pizza 100","python and frontend and senior and chicken 200","cpp and - and senior and pizza 250","- and backend and senior and - 150","- and - and - and chicken 100","- and - and - and - 150"]`|[1,1,1,1,2,4]|

___

### 입출력 예에 대한 설명
지원자 정보를 표로 나타내면 다음과 같습니다.

|언어|직군|경력|소울 푸드|점수|
|:---|:---|:---|:---|:---|
|java|backend|junior|pizza|150|
|python|frontend|senior|chicken|210|
|python|frontend|senior|chicken|150|
|cpp|backend|senior|pizza|260|
|java|backend|junior|chicken|80|
|python|backend|senior|chicken|50|

* `"java and backend and junior and pizza 100"` : java로 코딩테스트를 봤으며, backend 직군을 선택했고 junior 경력이면서 소울푸드로 pizza를 선택한 지원자 중 코딩테스트 점수를 100점 이상 받은 지원자는 1명 입니다.
* `"python and frontend and senior and chicken 200"` : python으로 코딩테스트를 봤으며, frontend 직군을 선택했고, senior 경력이면서 소울 푸드로 chicken을 선택한 지원자 중 코딩테스트 점수를 200점 이상 받은 지원자는 1명 입니다.
* `"cpp and - and senior and pizza 250"` : cpp로 코딩테스트를 봤으며, senior 경력이면서 소울푸드로 pizza를 선택한 지원자 중 코딩테스트 점수를 250점 이상 받은 지원자는 1명 입니다.
* `"- and backend and senior and - 150"` : backend 직군을 선택했고, senior 경력인 지원자 중 코딩테스트 점수를 150점 이상 받은 지원자는 1명 입니다.
* `"- and - and - and chicken 100"` : 소울푸드로 chicken을 선택한 지원자 중 코딩테스트 점수를 100점 이상을 받은 지원자는 2명 입니다.
* `"- and - and - and - 150"` : 코딩테스트 점수를 150점 이상 받은 지원자는 4명 입니다.

## 제출답안
```python
from collections import deque
from bisect import bisect_left
def bfs(query, trie):
    q_score = int(query[-1])
    queue = deque([(trie, 0)])
    count = 0

    while queue:
        now, idx = queue.popleft()
        if idx == 4: # 모든 조건을 만족하는 경우
            count += len(now['score']) - bisect_left(now['score'], q_score) # 이진 탐색을 통해 개수를 세어 추가
            continue

        if query[idx] == '-': # 해당 조건을 고려하지 않으면 모든 자식을 큐에 추가
            for key in now:
                queue.append((now[key], idx+1))
        elif query[idx] in now: # 조건을 고려한다면 만족하는 해당 조건을 키로하는 자식을 큐에 추가
            queue.append((now[query[idx]], idx+1))

    return count

def solution(info, query):
    answer = []
    trie = dict() # 조건을 키로하는 점수를 저장하는 트라이
    info.sort(key=lambda x: int(x.rsplit(' ', 1)[1])) # 점수에 대하여 오름차순 정렬

    for i in info: # 키를 따라가며 트라이에 점수 삽입
        i = i.split()
        score = int(i[-1])
        now = trie
        for j in i[:-1]:
            if j not in now: now[j] = dict()
            now = now[j]
        if 'score' in now: now['score'].append(score)
        else: now['score'] = [score]
    
    for q in query: # 트라이를 BFS로 탐색하여 조건을 만족하는 개수 탐색
        q = q.replace(' and', '').split()
        answer.append(bfs(q, trie))

    return answer
```
### 설명
본 문제의 경우 탐색을 빠르게 할 수 있는 방법을 적용하여 해결할 수 있다.

문제를 해결하기 위해서 다음의 3가지 방법을 시도해보았다.

1. 점수를 키로하는 인덱스를 만들어 이진 탐색을 이용해 접근하는 방법
2. 트라이 자료구조를 사용하는 방법
3. 해시 테이블을 만들어 경우의 수를 낮추는 방법

사실 이 세 가지의 방법 모두 단독적으로 사용하였을 때는 효율성 검사에서 실패하였다. 그래서 다른 방법이 있나 고민하던 중 이들을 조합할 수 있을 것 같다는 생각이 들었다.
실제로 가장 빠를 것 같은 2번과 1번을 조합하여 본 문제를 해결할 수 있었고, 3번과 1번을 조합하여 푼 경우도 더 느리기는 하지만 통과할 수 있었다.

먼저 2번에 대해서 살펴보자. 본 문제의 경우 쿼리 조건의 순서는 항상 동일하다. 즉 `언어 → 직군 → 경력 → 소울 푸드 → 점수`의 순서를 유지한다. 
이는 트라이 자료구조를 이용하기에 매우 적합한 형태이다. 따라서 `언어 → 직군 → 경력 → 소울 푸드` 순서로 트리 구조를 만들고 마지막에는 `점수`를 저장한다.

이후 BFS를 통해서 조건에 따라 트라이를 탐색하며 단말 노드에 저장된 점수를 비교하며 개수를 세면 된다. 하지만 이렇게만 한다면 조건을 탐색하는 과정을 최적화하긴 하였지만, 
점수를 비교하는 과정은 여전히 단순 반복문이 되게 되므로 효율성 검사에서 실패한다.

점수를 비교하는 과정 역시 최적화하기 위해선 어떻게 해야할까? 바로 여기서 1번을 조합할 수 있다. 단말 노드에 저장된 점수를 오름차순으로 정렬하고 이진 탐색으로 해당 점수 이상인 것의 개수를 
바로 구하는 방법이다. 이렇게 한다면 트라이를 통한 조건 탐색 최적화 + 이진 탐색을 이용한 점수 비교 최적화로 탐색을 빠르게 수행할 수 있고, 제출된 답안이 이러한 형태이다.  
(트라이와 이진 탐색을 이용한 범위 내의 개수 확인에 대해서는 이미 작성한 적이 있어 기술하지 않는다.)

참고로 트라이를 만들고 단말 노드인 점수들을 오름차순으로 정렬하기 위해서 트라이를 만들고 단말을 탐색해 하나하나 정렬하는 방법도 있겠지만, 생각해보면 애초부터 info를 점수에 대해 정렬한 후로 그 순서에 따라 
트라이에 삽입한다면 자동으로 단말 노드는 정렬된 상태일 것이다.

이번에는 3번에 대해서 생각해보자. 위에서도 언급하였지만 본 문제의 경우 조건의 수와 순서는 고정되어 있다. 따라서 해시 테이블로 조건을 키로하여 점수를 저장한다면 해시 테이블을 모두 탐색하는 경우의 수는 
확연하게 줄어들 것이다.

그렇다면 가능한 경우의 수를 생각해보자.

* 개발언어는 cpp, java, python로 가능한 경우의 수 3
* 직군은 backend, frontend로 가능한 경우의 수 2
* 경력은 junior, senior로 가능한 경우의 수 2
* 소울푸드는 chicken, pizza로 가능한 경우의 수 2

이므로 해시 테이블의 키로 나올 수 있는 경우의 수는 3 × 2 × 2 × 2 = 24가지 이다. 따라서 쿼리의 조건을 탐색하기 위해서 info를 탐색하는 대신 
조건과 해시 테이블의 키가 매칭되는지를 최대 24번만 수행하여 조건을 탐색할 수 있다.

이러한 해시를 이용한 최적화 방법 역시 조건 탐색에 대한 최적화는 이루어졌지만 트라이 때와 같이 점수 비교에 대한 최적화가 이루어지지 않는다면 여전히 효율성 검사는 실패한다. 따라서 트라이 때와 마찬가지로 이진 탐색을 이용해서 점수 비교 최적화를 적용하여 조합한다면 효율성 검사를 통과할 수 있다.

이렇게 구현한 방법은 다음과 같다.
```python
import re
from bisect import bisect_left
def solution(info, query):
    answer = []
    dic = dict()
    
    for i in sorted(info, key=lambda x: int(x.rsplit(' ', 1)[1])):
        i = i.rsplit(' ', 1)
        if i[0] in dic: dic[i[0]].append(int(i[1]))
        else: dic[i[0]] = [int(i[1])]
            
    for q in query:
        q = q.rsplit(' ', 1)
        count = 0
        for key in dic:
            if re.match(q[0].replace(' and', '').replace('-', '.+'), key):
                count += len(dic[key]) - bisect_left(dic[key], int(q[1]))
        answer.append(count)
    
    return answer
```
