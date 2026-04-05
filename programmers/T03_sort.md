오늘 풀어본 [Programmers 정렬 문제 세트](https://school.programmers.co.kr/learn/courses/30/parts/12198)의 풀이 정리입니다.

## 1. K번째수 (Level 1)

- **문제 링크**: [https://school.programmers.co.kr/learn/courses/30/lessons/42748](https://school.programmers.co.kr/learn/courses/30/lessons/42748)

```python
def solution(array, commands):
    answer = []
    for c in commands:
        tmp = compute(array, c)
        answer.append(tmp)

    return answer

def compute(array, command):
    idx1, idx2, k = command
    sliced = array[idx1 - 1: idx2]
    sliced.sort()
    return sliced[k - 1]
```

## 2. 가장 큰 수 (Level 2)

- **문제 링크**: [https://school.programmers.co.kr/learn/courses/30/lessons/42746](https://school.programmers.co.kr/learn/courses/30/lessons/42746)

```python
from functools import cmp_to_key

def solution(numbers):
    numbers = list(map(str, numbers))
    numbers.sort(key = cmp_to_key(cmp_))
    tmp = "".join(numbers)

    return str(int(tmp))

def cmp_(x, y):
    if x + y > y + x:
        return -1
    elif x + y == y + x:
        return 0
    else:
        return 1
```

## 3. H-Index (Level 2)

- **문제 링크**: [https://school.programmers.co.kr/learn/courses/30/lessons/42747](https://school.programmers.co.kr/learn/courses/30/lessons/42747)

```python
def solution(citations):
    n_citations = len(citations)
    for n in range(n_citations, -1, -1):
        if h_check(n, citations):
            return n
    return

def h_check(n, cits):
    count = 0
    for item in cits:
        if item >= n:
            count += 1
        if count >= n:
            return True
    return False
```
