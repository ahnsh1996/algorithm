# [2018 KAKAO BLIND RECRUITMENT 1차] 캐시
## 문제
### 문제 설명
([원본 문제](https://programmers.co.kr/learn/courses/30/lessons/17680)의 내용을 복사한 것입니다.)

### 캐시
지도개발팀에서 근무하는 제이지는 지도에서 도시 이름을 검색하면 해당 도시와 관련된 맛집 게시물들을 데이터베이스에서 읽어 보여주는 서비스를 개발하고 있다.  
이 프로그램의 테스팅 업무를 담당하고 있는 어피치는 서비스를 오픈하기 전 각 로직에 대한 성능 측정을 수행하였는데, 제이지가 작성한 부분 중 데이터베이스에서 게시물을 가져오는 부분의 실행시간이 너무 오래 걸린다는 것을 알게 되었다.  
어피치는 제이지에게 해당 로직을 개선하라고 닦달하기 시작하였고, 제이지는 DB 캐시를 적용하여 성능 개선을 시도하고 있지만 캐시 크기를 얼마로 해야 효율적인지 몰라 난감한 상황이다.

어피치에게 시달리는 제이지를 도와, DB 캐시를 적용할 때 캐시 크기에 따른 실행시간 측정 프로그램을 작성하시오.

### 입력 형식
* 캐시 크기(```cacheSize```)와 도시이름 배열(```cities```)을 입력받는다.
* ```cacheSize```는 정수이며, 범위는 0 ≦ ```cacheSize``` ≦ 30 이다.
* ```cities```는 도시 이름으로 이뤄진 문자열 배열로, 최대 도시 수는 100,000개이다.
* 각 도시 이름은 공백, 숫자, 특수문자 등이 없는 영문자로 구성되며, 대소문자 구분을 하지 않는다. 도시 이름은 최대 20자로 이루어져 있다.

### 출력 형식
* 입력된 도시이름 배열을 순서대로 처리할 때, "총 실행시간"을 출력한다.

### 조건
* 캐시 교체 알고리즘은 ```LRU```(Least Recently Used)를 사용한다.
* ```cache hit```일 경우 실행시간은 ```1```이다.
* ```cache miss```일 경우 실행시간은 ```5```이다.

### 입출력 예제
|캐시크기(cacheSize)|도시이름(cities)|실행시간|
|:---|:---|:---|
|3|["Jeju", "Pangyo", "Seoul", "NewYork", "LA", "Jeju", "Pangyo", "Seoul", "NewYork", "LA"]|50|
|3|["Jeju", "Pangyo", "Seoul", "Jeju", "Pangyo", "Seoul", "Jeju", "Pangyo", "Seoul"]|21|
|2|["Jeju", "Pangyo", "Seoul", "NewYork", "LA", "SanFrancisco", "Seoul", "Rome", "Paris", "Jeju", "NewYork", "Rome"]|60|
|5|["Jeju", "Pangyo", "Seoul", "NewYork", "LA", "SanFrancisco", "Seoul", "Rome", "Paris", "Jeju", "NewYork", "Rome"]|52|
|2|["Jeju", "Pangyo", "NewYork", "newyork"]|16|
|0|["Jeju", "Pangyo", "Seoul", "NewYork", "LA"]|25|

## 제출답안
```python
def solution(cacheSize, cities):
    if cacheSize == 0: return len(cities) * 5 # 캐시 크기가 0일 때는 굳이 반복을 수행하지 않아도 됨
    answer = 0 # 총 실행시간
    cache = dict() # 캐시

    for time in range(len(cities)):
        city = cities[time].upper() # 대소문자 구분을 하지 않기 위함
        if city not in cache: # cache miss일 경우
            if cache and len(cache) >= cacheSize: # 캐시 교체가 필요한 경우
                del_city = min(cache.items(), key=lambda x: x[1])[0]
                del cache[del_city] # 가장 오랫동안 사용되지 않은 도시 삭제
            answer += 5
        else: answer += 1 # cache hit일 경우
        cache[city] = time # 캐시 속 도시의 접근 시간을 기록

    return answer
```
### 설명
본 문제의 경우 LRU 교체 알고리즘을 구현하는 문제이다.

LRU 알고리즘은 빈 공간이 없을 때 가장 오랫동안 사용되지 않은 부분을 삭제한다. 보통의 경우 LRU 알고리즘을 이중 연결 리스트로 구현한다고 하나, 본 문제의 경우 단순히 LRU 캐시를 시뮬레이션하는 
정도이기 때문에 간단하게 구현하였다.

먼저 캐시는 딕셔너리 타입을 사용하였다. 도시를 키로 갖는 딕셔너리의 값은 해당 부분을 접근한 시간을 나타낸다.  
이후 캐시가 가득차서 교체가 필요할 때, 딕셔너리의 값들을 비교해 접근한 시간이 가장 오래된 것을 찾아 삭제한다면 LRU 알고리즘을 구현할 수 있다.

하지만 만약 실제로 LRU 알고리즘의 기능을 구현해야 한다면, 인터넷에 잘 작성되어 있는 LRU 구현 코드를 찾아보길 바란다.

## 기타
본 문제에서 캐시를 딕셔너리로 구현한 이유는 캐시 안에 값이 존재하는지 확인하는 연산(in)과 캐시에서 값을 삭제하는 연산(del)이 O(1)이기 때문이었다.

하지만 제출 후 다른 사람들의 코드를 보면 리스트로 큐를 구현했음에도 본문의 제출 코드보다 빠르게 도는 것을 확인하였다.  
(in에서 O(n)이 걸리며, 캐시 안에 값이 존재하는 경우 해당 값을 지우고 다시 큐에 삽입하기 위한 remove 연산에서 O(n)이 걸리기 때문에 더 느릴 줄 알았다.)

아직 정확한 원인을 찾지 못 하였기 때문에 추후 더 알아볼 필요가 있다.
