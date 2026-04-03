오늘 풀어본 [Programmers 스택/큐 문제 세트](https://school.programmers.co.kr/learn/courses/30/parts/12081)의 풀이 정리입니다.

## 1. 같은 숫자는 싫어 (Level 1)

- **문제 링크**: [https://school.programmers.co.kr/learn/courses/30/lessons/12906](https://school.programmers.co.kr/learn/courses/30/lessons/12906)

```python
def solution(arr):
    t_stack = []

    t_stack.append(arr[0])
    for idx in range(1, len(arr)):
        if t_stack[-1] != arr[idx]:
            t_stack.append(arr[idx])

    return t_stack
```

## 2. 기능개발 (Level 2)

- **문제 링크**: [https://school.programmers.co.kr/learn/courses/30/lessons/42586](https://school.programmers.co.kr/learn/courses/30/lessons/42586)

```python
def solution(progresses, speeds):
    n_task = len(progresses)
    days_required = []

    for idx in range(n_task):
        temp = (100 - progresses[idx]) // speeds[idx]
        if (100 - progresses[idx]) % speeds[idx] != 0:
            temp += 1
        days_required.append(temp)

    release = []
    previous = days_required[0]
    count = 1
    for idx in range(1, n_task):
        if previous < days_required[idx]:
            release.append(count)
            previous = days_required[idx]
            count = 1
        else:
            count += 1
    release.append(count)

    return release
```

## 3. 올바른 괄호 (Level 2)

- **문제 링크**: [https://school.programmers.co.kr/learn/courses/30/lessons/12909](https://school.programmers.co.kr/learn/courses/30/lessons/12909)

```python
def solution(s):
    letter_stack = []
    for letter in s:
        if letter == "(":
            letter_stack.append(letter)
        elif letter == ")":
            if letter_stack and letter_stack[-1] == "(":
                letter_stack.pop()
            else:
                return False

    if letter_stack:
        return False
    return True
```

## 4. 프로세스 (Level 2)

- **문제 링크**: [https://school.programmers.co.kr/learn/courses/30/lessons/42587](https://school.programmers.co.kr/learn/courses/30/lessons/42587)

```python
from collections import deque

def solution(priorities, location):
    task_stack = []
    for idx, p in enumerate(priorities):
        task_stack.append([idx, p])
    task_queue = deque(task_stack)

    priorities.sort()
    sequence = 0
    while priorities:
        current_task = task_queue.popleft()
        if current_task[1] == priorities[-1]:
            priorities.pop(-1)
            sequence += 1
            if current_task[0] == location:
                return sequence
        else:
            task_queue.append(current_task)

    return
```

## 5. 다리를 지나는 트럭 (Level 2)

- **문제 링크**: [https://school.programmers.co.kr/learn/courses/30/lessons/42583](https://school.programmers.co.kr/learn/courses/30/lessons/42583)

```python
from collections import deque

def solution(bridge_length, capacity, truck_weights):
    queue_wait = deque(truck_weights)
    queue_bridge = deque([]) # [[weight, position], [weight, position], ...]

    time_count = 0
    while queue_bridge or queue_wait:
        time_count += 1
        n_pass = 0

        # update the position of truck
        for truck in queue_bridge:
            if truck[1] == bridge_length: # count up passing ones
                n_pass += 1
            else:
                truck[1] += 1

        # process passing ones
        for _ in range(n_pass):
            queue_bridge.popleft()

        # process new enter
        total_weight = get_total_weight(queue_bridge)
        if queue_wait and total_weight + queue_wait[0] <= capacity:
            queue_bridge.append([queue_wait.popleft(), 1])

    return time_count

def get_total_weight(queue_bridge):
    rv = 0
    for weight, _ in queue_bridge:
        rv += weight

    return rv
```

## 6. 주식가격 (Level 2)

- **문제 링크**: [https://school.programmers.co.kr/learn/courses/30/lessons/42584](https://school.programmers.co.kr/learn/courses/30/lessons/42584)

```python
def solution(prices):
    tmp_stack = [] # [(idx, price), (idx, price), (idx, price), ...]
    tmp_answer = {} # idx: answer

    for idx, price in enumerate(prices):
        if not tmp_stack:
            tmp_stack.append((idx, price))
        elif tmp_stack and tmp_stack[-1][1] <= price:
            tmp_stack.append((idx, price))
        else:
            while tmp_stack and tmp_stack[-1][1] > price:
                pop_idx, _ = tmp_stack.pop()
                tmp_answer[pop_idx] = idx - pop_idx
            tmp_stack.append((idx, price))

    while tmp_stack:
        pop_idx, _ = tmp_stack.pop()
        tmp_answer[pop_idx] = len(prices) - 1 - pop_idx

    return [tmp_answer[idx] for idx in range(len(prices))]
```
