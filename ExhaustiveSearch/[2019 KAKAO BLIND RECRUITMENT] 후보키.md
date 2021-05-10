# [2019 KAKAO BLIND RECRUITMENT] 후보키
## 문제
### 문제 설명
([원본 문제](https://programmers.co.kr/learn/courses/30/lessons/42890)의 내용을 복사한 것입니다.)

### 후보키

프렌즈대학교 컴퓨터공학과 조교인 제이지는 네오 학과장님의 지시로, 학생들의 인적사항을 정리하는 업무를 담당하게 되었다.

그의 학부 시절 프로그래밍 경험을 되살려, 모든 인적사항을 데이터베이스에 넣기로 하였고, 이를 위해 정리를 하던 중에 후보키(Candidate Key)에 대한 고민이 필요하게 되었다.

후보키에 대한 내용이 잘 기억나지 않던 제이지는, 정확한 내용을 파악하기 위해 데이터베이스 관련 서적을 확인하여 아래와 같은 내용을 확인하였다.

* 관계 데이터베이스에서 릴레이션(Relation)의 튜플(Tuple)을 유일하게 식별할 수 있는 속성(Attribute) 또는 속성의 집합 중, 다음 두 성질을 만족하는 것을 후보 키(Candidate Key)라고 한다.
  * 유일성(uniqueness) : 릴레이션에 있는 모든 튜플에 대해 유일하게 식별되어야 한다.
  * 최소성(minimality) : 유일성을 가진 키를 구성하는 속성(Attribute) 중 하나라도 제외하는 경우 유일성이 깨지는 것을 의미한다. 즉, 릴레이션의 모든 튜플을 유일하게 식별하는 데 꼭 필요한 속성들로만 구성되어야 한다.

제이지를 위해, 아래와 같은 학생들의 인적사항이 주어졌을 때, 후보 키의 최대 개수를 구하라.

![예시](https://user-images.githubusercontent.com/77680436/117615681-05cd6880-b1a5-11eb-8c3b-9222815042b8.png)

위의 예를 설명하면, 학생의 인적사항 릴레이션에서 모든 학생은 각자 유일한 "학번"을 가지고 있다. 따라서 "학번"은 릴레이션의 후보 키가 될 수 있다.  
그다음 "이름"에 대해서는 같은 이름("apeach")을 사용하는 학생이 있기 때문에, "이름"은 후보 키가 될 수 없다. 그러나, 만약 ["이름", "전공"]을 함께 사용한다면 릴레이션의 모든 튜플을 유일하게 식별 가능하므로 후보 키가 될 수 있게 된다.  
물론 ["이름", "전공", "학년"]을 함께 사용해도 릴레이션의 모든 튜플을 유일하게 식별할 수 있지만, 최소성을 만족하지 못하기 때문에 후보 키가 될 수 없다.  
따라서, 위의 학생 인적사항의 후보키는 "학번", ["이름", "전공"] 두 개가 된다.

릴레이션을 나타내는 문자열 배열 relation이 매개변수로 주어질 때, 이 릴레이션에서 후보 키의 개수를 return 하도록 solution 함수를 완성하라.

### 제한사항
* relation은 2차원 문자열 배열이다.
* relation의 컬럼(column)의 길이는 `1` 이상 `8` 이하이며, 각각의 컬럼은 릴레이션의 속성을 나타낸다.
* relation의 로우(row)의 길이는 `1` 이상 `20` 이하이며, 각각의 로우는 릴레이션의 튜플을 나타낸다.
* relation의 모든 문자열의 길이는 `1` 이상 `8` 이하이며, 알파벳 소문자와 숫자로만 이루어져 있다.
* relation의 모든 튜플은 유일하게 식별 가능하다.(즉, 중복되는 튜플은 없다.)

### 입출력 예
|relation|result|
|:---|:---|
|`[["100","ryan","music","2"],["200","apeach","math","2"],["300","tube","computer","3"],["400","con","computer","4"],["500","muzi","music","3"],["600","apeach","music","2"]]`|2|

### 입출력 예 설명
**입출력 예 #1**  
문제에 주어진 릴레이션과 같으며, 후보 키는 2개이다.

## 제출답안
```python
from itertools import combinations
def solution(relation):
    attribute = [i for i in range(len(relation[0]))]
    candidateKey = []
    
    for num in range(1, len(attribute)):
        for comb in combinations(attribute, num): # 후보키 검사를 할 속성 조합 추출
            temp = set()
            for row in range(len(relation)): # 유일성을 확인하는 과정
                value = tuple(relation[row][column] for column in comb)
                if value in temp: break
                temp.add(value)
            else: # 유일성이 확인 되었다면 최소성을 확인
                comb = set(comb)
                for key in candidateKey:
                    if not key-comb: break
                else: candidateKey.append(comb)

    return len(candidateKey) if candidateKey else 1
```
### 설명
본 문제의 경우 제한사항을 봤을 때 입력 값의 범위가 매우 작은 것을 볼 수 있다. 이러한 경우 완전 탐색 문제일 가능성이 매우 높다.

완전 탐색을 이용하여 문제를 해결하기 위해서 후보키가 될 수 있는 가능한 모든 키 조합, 즉 속성들의 조합을 얻어야 한다. 이는 파이썬의 combinations 모듈을 이용하여 어렵지 않게 가능하다.

여기서 모든 속성이 뽑히는 경우는 굳이 추출하지 않은 것을 확인할 수 있다.(`for num in range(1, len(attribute)+1)이 아닌 for num in range(1, len(attribute))`)

왜냐하면 제한사항에서 이미 `'relation의 모든 튜플은 유일하게 식별 가능하다.'`라고 하였기 때문에 적어도 하나의 후보키는 존재한다는 것이다. 따라서 반복문을 한 번 더 수행하기 보다는 
마지막에 조건문(`len(candidateKey) if candidateKey else 1`)을 통하여 이를 확인하는 방법을 사용하였다.

아무튼 속성 조합을 추출하였다면 먼저 유일성을 만족하는지 확인하여야 한다. 이는 데이터베이스에서 해당 속성들의 값들로 이루어진 튜플을 만들고 중복이 존재하는지 확인하면 어렵지 않게 수행할 수 있다.

키의 유일성이 확인 되었다면 이제 최소성에 대한 검사를 수행하여야 한다. 속성 조합을 추출할 때 뽑는 개수는 오름차순으로 진행된다. 
따라서 나중에 만들어진 슈퍼키(유일성을 만족하는 키)는 먼저 확인된 후보키의 superset이 되지 않아야(슈퍼키<sub>나중</sub> ⊃ 후보키<sub>기존</sub>이면 안됨) 최소성을 만족한다고 볼 수 있다.

이는 기존에 만들어진 후보키를 순회하면서 후보키에 대한 현재 슈퍼키의 차집합(답안의 `key-comb`)이 공집합인지 아닌지 확인하여 검증할 수 있다.

이처럼 완전 탐색을 통해 유일성과 최소성을 검증하여 본 문제를 해결할 수 있다.
