# [2020 KAKAO BLIND RECRUITMENT] 가사 검색
## 문제
### 문제 설명
([원본 문제](https://programmers.co.kr/learn/courses/30/lessons/60060)의 내용을 복사한 것입니다.)

**[본 문제는 정확성과 효율성 테스트 각각 점수가 있는 문제입니다.]**

친구들로부터 천재 프로그래머로 불리는 <b>"프로도"</b>는 음악을 하는 친구로부터 자신이 좋아하는 노래 가사에 사용된 단어들 중에 특정 키워드가 몇 개 포함되어 있는지 궁금하니 프로그램으로 개발해 달라는 제안을 받았습니다.  
그 제안 사항 중, 키워드는 와일드카드 문자중 하나인 '?'가 포함된 패턴 형태의 문자열을 뜻합니다. 와일드카드 문자인 '?'는 글자 하나를 의미하며, 어떤 문자에도 매치된다고 가정합니다. 예를 들어 ```"fro??"```는 ```"frodo"```, ```"front"```, ```"frost"``` 등에 매치되지만 ```"frame"```, ```"frozen"```에는 매치되지 않습니다.

가사에 사용된 모든 단어들이 담긴 배열 ```words```와 찾고자 하는 키워드가 담긴 배열 ```queries```가 주어질 때, 각 키워드 별로 매치된 단어가 몇 개인지 **순서대로** 배열에 담아 반환하도록 ```solution``` 함수를 완성해 주세요.

### 가사 단어 제한사항
* ```words```의 길이(가사 단어의 개수)는 2 이상 100,000 이하입니다.
* 각 가사 단어의 길이는 1 이상 10,000 이하로 빈 문자열인 경우는 없습니다.
* 전체 가사 단어 길이의 합은 2 이상 1,000,000 이하입니다.
* 가사에 동일 단어가 여러 번 나올 경우 중복을 제거하고 ```words```에는 하나로만 제공됩니다.
* 각 가사 단어는 오직 알파벳 소문자로만 구성되어 있으며, 특수문자나 숫자는 포함하지 않는 것으로 가정합니다.

### 검색 키워드 제한사항
* ```queries```의 길이(검색 키워드 개수)는 2 이상 100,000 이하입니다.
* 각 검색 키워드의 길이는 1 이상 10,000 이하로 빈 문자열인 경우는 없습니다.
* 전체 검색 키워드 길이의 합은 2 이상 1,000,000 이하입니다.
* 검색 키워드는 중복될 수도 있습니다.
* 각 검색 키워드는 오직 알파벳 소문자와 와일드카드 문자인 ```'?'``` 로만 구성되어 있으며, 특수문자나 숫자는 포함하지 않는 것으로 가정합니다.
* 검색 키워드는 와일드카드 문자인 ```'?'```가 하나 이상 포함돼 있으며, ```'?'```는 각 검색 키워드의 접두사 아니면 접미사 중 하나로만 주어집니다.
  * 예를 들어 ```"??odo"```, ```"fro??"```, ```"?????"```는 가능한 키워드입니다.
  * 반면에 ```"frodo"```(```'?'```가 없음), ```"fr?do"```(```'?'```가 중간에 있음), ```"?ro??"```(```'?'```가 양쪽에 있음)는 불가능한 키워드입니다.

### 입출력 예
|words|queries|result|
|:---|:---|:---|
|```["frodo", "front", "frost", "frozen", "frame", "kakao"]```|```["fro??", "????o", "fr???", "fro???", "pro?"]```|```[3, 2, 4, 1, 0]```|

### 입출력 예에 대한 설명
* ```"fro??"```는 ```"frodo"```, ```"front"```, ```"frost"```에 매치되므로 3입니다.
* ```"????o"```는 ```"frodo"```, ```"kakao"```에 매치되므로 2입니다.
* ```"fr???"```는 ```"frodo"```, ```"front"```, ```"frost"```, ```"frame"```에 매치되므로 4입니다.
* ```"fro???"```는 ```"frozen"```에 매치되므로 1입니다.
* ```"pro?"```는 매치되는 가사 단어가 없으므로 0 입니다.

## 제출답안
```python
from collections import deque
def bfs(trie, query, isPrefix): # 트라이를 탐색하며 검색 키워드에 매치되는 가사 단어의 수 반환
    idx = len(query) - 1 if isPrefix else 0 # 검색 키워드의 와일드 카드가 접두사인지 여부에 따라 탐색 방향 결정
    add = -1 if isPrefix else 1
    end = -1 if isPrefix else len(query)
    queue = deque([(trie[key], idx+add) for key in trie if query[idx] == key or query[idx] == '?'])
    count = 0

    while queue:
        now, idx = queue.popleft()
        if idx == end and not now: count += 1 # 검색 키워드를 모두 탐색했을 때 가사 단어 역시 마지막 문자라면(단말이라면) 매치
        elif (isPrefix and idx > end) or (not isPrefix and idx < end): # 검색 키워드를 모두 검색하지 않았다면 다음 문자 검색
            for key in now:
                if query[idx] == key or query[idx] == '?':
                    queue.append((now[key], idx+add))
    return count

def solution(words, queries):
    answer = []
    trie = dict() # 가사 길이에 따른 트라이
    result = dict() # 검색 키워드는 중복이 허용되기 때문에 불필요한 중복 탐색을 피하기 위해 결과 저장

    # 가사 단어 길이에 따른 트라이 저장
    for word in words:
        length = len(word)
        if len(word) not in trie: trie[length] = [dict(), dict()]
        forward = trie[length][0] # 단어의 앞에서부터 저장
        backward = trie[length][1] # 단어의 뒤에서부터 저장
        for i in range(length):
            if word[i] not in forward:
                forward[word[i]] = dict()
            forward = forward[word[i]]
            if word[length-i-1] not in backward:
                backward[word[length-i-1]] = dict()
            backward = backward[word[length-i-1]]

    # BFS를 수행하며 검색 키워드에 매치되는 단어의 수 측정
    for query in queries:
        count = 0
        if query in result: count = result[query] # 이미 탐색했던 적이 있는 키워드면 저장한 결과 사용
        else:
            length = len(query)
            if length in trie:
                isPrefix = query[0] == '?' # 와일드 카드 문자열이 접두사인지 여부
                dic = trie[length][1] if isPrefix else trie[length][0]
                count = result[query] = bfs(dic, query, isPrefix)
        answer.append(count)

    return answer
```
### 설명
본 문제의 경우 트라이 자료구조를 이용하여 풀 수 있는 문제이다.

처음에는 직관적으로 생각하여 다음과 같이 구현하였다.

```python
def compare(word, query):
    if len(word) != len(query): return False
    
    start = len(query)-1 if query[0] == '?' else 0
    end = -1 if query[0] == '?' else len(query)
    add = -1 if query[0] == '?' else 1
    
    for i in range(start, end, add):
        if query[i] == '?': break
        if word[i] != query[i]: return False
    return True

def solution(words, queries):
    answer = []
    dic = dict()
    
    for query in queries:
        count = 0
        if query in dic:
            count = dic[query]
        else:
            for word in words:
                if compare(word, query): count += 1
            dic[query] = count
        answer.append(count)
    
    return answer
```

단어와 검색 키워드가 매치되는지 비교하는 compare 함수는 와일드 카드의 위치에 따라, 즉 '?'가 앞에 있다면 뒤에서부터, '?'가 뒤에 있다면 앞에서부터 단어와 키워드를 비교한다. 
그러다가 '?'를 만날 때까지 다른 문자가 없다면 매치, 그 이전에 다른 문자를 만난다면 매치하지 않는다는 결과를 반환한다.  
이렇게 하면 '?' 부분의 탐색은 피할 수 있어 단순한 방법 중에서는 그래도 빠른 편에 속할 것이다.

하지만 이런 정도로는 정확도는 통과하였지만, 효율성에 있어서 통과하지 못 하였다. 그래서 검색을 통해 문자열과 관련된 자료구조나 알고리즘이 있는지 확인하였더니 찾을 수 있었던 것이 트라이 자료구조 였다. 

![트라이 예시](https://user-images.githubusercontent.com/77680436/112762464-c4408e00-903a-11eb-9d30-75342c59d3ba.png)

위 사진은 [위키백과](https://ko.wikipedia.org/wiki/%ED%8A%B8%EB%9D%BC%EC%9D%B4_(%EC%BB%B4%ED%93%A8%ED%8C%85))에 나와있는 트라이의 예이다.  

위와 같이 트라이는 문자열을 저장할 때 한 문자 한 문자가 다음 노드를 결정한다. 따라서 문자열의 문자 자체가 노드를 가리키는 포인터 역할을 하기 때문에, 시간복잡도는 문자열의 길이만큼이 걸린다.(문자열의 길이가 L이라고 한다면 O(L)만에 검색을 할 수 있다.)  
이러한 이유는 포인터가 가리키는 내용을 조회하는 것은 O(1) 연산으로 가능하고, 트라이를 탐색하는 과정은 각 문자가 가리키는 포인터를 따라가는 과정이기 때문이다.  
(트라이는 영어로 trie인데, 이는 단어 retrieval의 중간 음절에서 따왔다고 한다. 이처럼 이름에서부터 검색을 위한 자료구조임이 느껴진다.)

원래의 경우라면 노드 클래스를 만들고 트라이 클래스를 만들고 등등 복잡하게 구현해야 할 수도 있겠지만, 본 답안에서는 간단하게 트라이 자료구조 역할을 할 수 있도록 딕셔너리를 중첩한 구조를 이용하였다.
즉, 문자열의 각 문자는 키로 사용되고, 해당 키에 대응되는 값은 다음 문자의 딕셔너리들을 가지고 있다.

예를 들어 "fro" 문자열이 있다면 다음과 같은 순서로 만들 수 있다.
1. {'f': {}}
2. {'f': {'r': {}}}
3. {'f': {'r': {'o': {}}}}

이렇게 만든다면 트라이와 같이 각 문자가 다음 문자들을 가리키는 키가 되며, 트라이처럼 이용할 수 있다. 잘 이해가 되지 않을 수 있겠지만, 
실제 답안의 코드를 유심히 살펴보면 C언어의 포인터를 따라가는 과정과 유사하다고 느껴질 것이다.~~(아마?)~~  
본 문제에서는 '?'가 앞에서부터 있을 수 있으므로 이 경우 더 효율적으로 탐색하기 위해서 단어의 역순으로도 트라이를 생성해야 한다.
즉, ```{'o': {'r': {'f': {}}}}```와 같이 말이다.

이러한 트라이를 단어의 길이에 따라 저장한다. 이후 검색 키워드의 길이에 따라 대응되는 트라이를 탐색하여 매치되는 단어의 수를 빠르게 셀 수 있다.

여기서 트라이를 탐색하는 과정은 다양한 방법이 존재하겠지만, 본 답안에서는 단순히 트리를 탐색하듯이 BFS를 이용하였다.  
BFS에서는 검색 키워드의 문자를 따라 해당 문자의 문자를 키로 가지고 있거나 검색 키워드의 문자가 '?'일 때 방문 가능으로 판단하여 방문하였다. 
예를 들어 검색 키워드가 "f?"라면 처음에는 키를 "f"로 갖는 딕셔너리가 가리키는 다음 딕셔너리를 방문한다. 다음에는 와일드 카드이므로, "f"가 가리켰던 다음 문자들을 모두 방문한다. 이러한 식으로 BFS를 진행할 수 있다.

BFS를 통해 검색 키워드에 매치되는 단어의 수를 얻어냈다면, 이제 해당 결과를 저장할 필요가 있다. 이는 제한사항의 ```검색 키워드는 중복될 수도 있습니다.```이 불필요한 중복 탐색이 발생할 수 있음을 알려주기 때문이다.

이처럼 본 답안에서는 딕셔너리, 즉 해시 테이블을 이용하여 트라이 자료구조를 흉내냈고 이를 탐색하는 과정을 통해 문제를 해결하였다.

## 개선사항
카카오 블로그에서는 다음과 같은 두 가지 방법을 소개하고 있다.

1. 트라이 자료구조에서 문자별로 카운트 값을 두는 방법

카카오에서도 트라이 자료구조를 사용함은 동일하나 단어를 삽입할 때, 문자 노드를 거칠 때마다 카운트 값을 증가시키는 방법을 소개하고 있다.  
예를 들어서 다음과 같다.

![카운트 예시](https://user-images.githubusercontent.com/77680436/112807781-3d7bc780-90b3-11eb-8ec0-910bb5f751c0.png)

위와 같이 같은 길이 단어들을 하나의 트라이에 저장하며, 각 문자를 삽입할 때 거치는 노드마다 카운트 값을 저장한다고 하자. 
이렇게 한다면 와일드 카드 '?'에 대한 탐색을 거치지 않고 바로 카운트를 반환할 수 있다는 장점이 있다. 
즉 위의 트라이 예에서는 길이가 2인 문자열이 4개가 있으며 그 중 'f'로 시작하는 문자가 2개, 'k'로 시작하는 문자열이 2개가 있다. 이를 삽입 시 저장하고 있기 때문에, 예를 들어 "f?"라는 
검색 키워드를 검색할 때 'f'가 가리키는 노드에 카운트가 2가 저장되어 있어 단말 노드까지 탐색하지 않아도 된다는 장점이 있다. 이는 단어의 길이가 길며 와일드 카드'?'가 많을 때 특히 많은 시간 차이를 보일 것으로 생각된다.

2. 이분탐색

카카오에서는 이분탐색을 활용할 수도 있다고 설명하고 있다. 이는 이분탐색을 응용한 파라메트릭 서치를 이용하여 풀 수 있다는 것을 말한다.  
(파라메트릭 서치에 대한 내용은 [여기](https://github.com/ahnsh1996/algorithm/blob/master/BinarySearch/%5B%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4%5D%20%EC%9E%85%EA%B5%AD%EC%8B%AC%EC%82%AC.md#%EC%84%A4%EB%AA%85)에서 
확인할 수 있다.)

파이썬에서는 파라메트릭 서치를 위해서 ```bisect``` 모듈의 ```bisect_left```와 ```bisect_right```를 사용할 수 있다. 이는 각각 반복 가능한 객체 속에서 특정 값이 삽입될 왼쪽 인덱스와 오른쪽 인덱스를 반환한다.

본 문제에서 이를 이용하여 알고리즘을 작성할 수 있다.  
먼저 파라메트릭 서치를 위해 단어 리스트를 길이별로 정렬한다. 이후 검색 키워드를 치환하여 ```bisect_left```와 ```bisect_right```를 사용하여 답을 구할 수 있다.

이에 대한 예는 다음과 같다.  
예를 들어 ```fro??```라는 검색 키워드가 매치되는 단어의 수를 계산한다고 하자. 여기서 와일드 카드를 모두 'a'로 변환한 후("froaa") ```bisect_left```를 통해 삽입될 인덱스를 구한다. 
다음과 같이 길이가 5인 정렬된 단어 리스트 ```['frame', 'frodo', 'front', 'frost', 'kakao']```에서는 1이 될 것이다.
이번에는 와일드 카드를 'z'로 변환하여("frozz") ```bisect_right```를 통해 삽입될 인덱스를 구한다. 마찬가지로 위의 경우에서는 4가 될 것이다. 그러면 해당 결과의 사이에 있는 단어들은 
"fro??" 키워드에 매치되는 단어의 수(4-1=3개)가 될 것이다.  

즉 이러한 느낌으로 단어의 수를 카운트 한다.  
['frame', <ins>'froaa'</ins>, <b>'frodo', 'front', 'frost'</b>, <ins>'frozz'</ins>, 'kakao']

참고로 'a'와 'z'로 변환하는 것은 제한사항에서 ```각 가사 단어는 오직 알파벳 소문자로만 구성```된다고 설명하고 있기 때문이다.

와일드 카드가 앞에 온 경우 이러한 방법을 그대로 사용할 경우 제대로 된 결과가 출력되지 않는다. 예를 들어 "????o"를 검색할 때에는 "aaaao", "zzzzo"가 되어 기본적인 단어 리스트에서는 
의도치 않은 결과를 반환할 것이기 때문이다.  
(삽입 시 [<ins>'aaaao'</ins>, <b>'frame', 'frodo', 'front', 'frost', 'kakao'</b>, <ins>'zzzzo'</ins>]가 될 것이다.)

따라서 이러한 경우에도 사용할 수 있도록 단어를 뒤집은 결과도 저장하고 검색 키워드 역시 뒤집어서 검색한다면 해결할 수 있다.

이러한 내용들을 적용하여 구현하면 다음과 같이 구현할 수 있다.
``` python
from bisect import bisect_left, bisect_right
def solution(words, queries):
    answer = []
    word = [dict(), dict()]
    result = dict()
    
    for w in words:
        length = len(w)
        if length not in word[0]:
            word[0][length] = []
            word[1][length] = [] # 역순
        word[0][length].append(w)
        word[1][length].append(''.join(reversed(w)))
        
    for length in word[0]:
        word[0][length].sort()
        word[1][length].sort()
        
    for query in queries:
        count = 0
        if query in result: count = result[query]
        else:
            length = len(query)
            if length in word[0]:
                isPrefix = query[0] == '?'
                query = ''.join(reversed(query)) if isPrefix else query
                left = bisect_left(word[isPrefix][length], query.replace('?', 'a'))
                right = bisect_right(word[isPrefix][length], query.replace('?', 'z'))
                count = right - left
        answer.append(count)

    return answer
```
본 문제에서는 파라메트릭 서치를 이용한 방법이 훨씬 빠른 결과를 보이는 것을 확인할 수 있었다.
