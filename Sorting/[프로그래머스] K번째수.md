# [프로그래머스] K번째수
## 문제
### 문제 설명
([원본 문제](https://programmers.co.kr/learn/courses/30/lessons/42748)의 내용을 복사한 것입니다.)

배열 array의 i번째 숫자부터 j번째 숫자까지 자르고 정렬했을 때, k번째에 있는 수를 구하려 합니다.

예를 들어 array가 [1, 5, 2, 6, 3, 7, 4], i = 2, j = 5, k = 3이라면

1. array의 2번째부터 5번째까지 자르면 [5, 2, 6, 3]입니다.
2. 1에서 나온 배열을 정렬하면 [2, 3, 5, 6]입니다.
3. 2에서 나온 배열의 3번째 숫자는 5입니다.

배열 array, [i, j, k]를 원소로 가진 2차원 배열 commands가 매개변수로 주어질 때, commands의 모든 원소에 대해 앞서 설명한 연산을 적용했을 때 나온 결과를 배열에 담아 return 하도록 solution 함수를 작성해주세요.

### 제한사항
* array의 길이는 1 이상 100 이하입니다.
* array의 각 원소는 1 이상 100 이하입니다.
* commands의 길이는 1 이상 50 이하입니다.
* commands의 각 원소는 길이가 3입니다.

### 입출력 예
|array|commands|return|
|:---:|:---:|:---:|
|[1, 5, 2, 6, 3, 7, 4]|[[2, 5, 3], [4, 4, 1], [1, 7, 3]]|[5, 6, 3]|

### 입출력 예 설명
[1, 5, 2, 6, 3, 7, 4]를 2번째부터 5번째까지 자른 후 정렬합니다. [2, 3, 5, 6]의 세 번째 숫자는 5입니다.  
[1, 5, 2, 6, 3, 7, 4]를 4번째부터 4번째까지 자른 후 정렬합니다. [6]의 첫 번째 숫자는 6입니다.  
[1, 5, 2, 6, 3, 7, 4]를 1번째부터 7번째까지 자릅니다. [1, 2, 3, 4, 5, 6, 7]의 세 번째 숫자는 3입니다.

## 제출답안
```python
def solution(array, commands):
    answer = []
    
    for i, j, k in commands: # i 번째부터 j 번째까지 잘라 정렬한 리스트의 k 번째 숫자를 answer 리스트에 추가
        answer.append(sorted(array[i-1:j])[k-1])
    
    return answer
```
### 설명
본 문제의 경우 어렵지 않은, 특히 파이썬을 이용한다면 매우 쉬운 문제이다. solution 함수는 다음과 같이 동작한다.  
1. commands 리스트로부터 i, j, k를 가져온다. → for i, j, k in commands:
2. i 번째부터 j 번째까지 자른다. → array[i-1:j]
3. 자른 리스트를 정렬한다. → sorted(array[i-1:j])
4. 정렬한 리스트의 k 번째 숫자를 정답 리스트에 추가한다. → answer.append(sorted(array[i-1:j])[k-1])
5. commands 리스트가 끝날 때까지 1-4를 반복한다.

## 개선사항
본 문제의 경우 워낙 간단하여 크게 개선할 사항이 없으나, 가독성 측면에서 약간 손해를 보더라도 map과 lambda를 이용하여 코드의 수를 더 간략하게 줄일 수는 있다.
```python
def solution(array, commands):
    return list(map(lambda x: sorted(array[x[0]-1:x[1]])[x[2]-1], commands))
```
