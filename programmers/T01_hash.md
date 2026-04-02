프로그래머스 고득점 Kit: [해시](https://school.programmers.co.kr/learn/courses/30/parts/12077) 문제 풀이 정리입니다.

## 1. 완주하지 못한 선수

- **문제 링크**: [완주하지 못한 선수](https://school.programmers.co.kr/learn/courses/30/lessons/42576)
- **풀이 설명**: 해시 테이블(딕셔너리)을 사용하여 참가자 명단의 이름을 카운트하고, 완주자 명단에 있는 이름을 감산하여 남은 이름(값이 0이 아닌 것)을 반환합니다.

```python
def solution(participant, completion):
    ta = {}

    for p in participant:
        if p not in ta:
            ta[p] = 1
        else:
            ta[p] += 1

    for c in completion:
        if c in ta:
            ta[c] -= 1

    for key, value in ta.items():
        if value != 0:
            return key

    return
```

## 2. 폰켓몬

- **문제 링크**: [폰켓몬](https://school.programmers.co.kr/learn/courses/30/lessons/1845)
- **풀이 설명**: 가질 수 있는 폰켓몬의 최대 수(`N/2`)와 중복을 제거한 폰켓몬 종류의 수(`len(ta)`)를 비교하여 더 작은 값을 반환합니다.

```python
def solution(nums):
    ta = {}

    for n in nums:
        if n not in ta:
            ta[n] = 1
        else:
            ta[n] += 1

    if len(nums) // 2 <= len(ta):
        return len(nums) // 2
    return len(ta.keys())
```

## 3. 전화번호 목록

- **문제 링크**: [전화번호 목록](https://school.programmers.co.kr/learn/courses/30/lessons/42577)
- **풀이 설명**: 전화번호부를 정렬한 뒤, 현재 번호가 바로 다음 번호의 접두어인지 확인하여 효율적으로 비교합니다.

```python
def solution(phone_book):
    phone_book.sort()

    for idx1, item1 in enumerate(phone_book):
        if idx1 + 1 < len(phone_book) and phone_book[idx1 + 1].startswith(item1):
            return False
    else:
        return True
```

## 4. 의상

- **문제 링크**: [의상](https://school.programmers.co.kr/learn/courses/30/lessons/42578)
- **풀이 설명**: 각 의상 종류별로 (의상 수 + 1)을 모두 곱한 뒤, 아무것도 입지 않은 경우인 1을 빼서 가능한 모든 조합의 수를 구합니다.

```python
def solution(clothes):
    ta = {}

    for item, category in clothes:
        if category not in ta:
            ta[category] = 1
        else:
            ta[category] += 1

    rv = 1
    for k, v in ta.items():
        rv *= (v + 1)

    return (rv - 1)
```

## 5. 베스트앨범

- **문제 링크**: [베스트앨범](https://school.programmers.co.kr/learn/courses/30/lessons/42579)
- **풀이 설명**: 장르별 총 재생 횟수와 장르 내 개별 곡 정보를 저장한 뒤, 조건(장르별 재생 수 내림차순 -> 장르 내 곡 재생 수 내림차순 -> 곡 ID 오름차순)에 맞춰 정렬하여 상위 2곡씩 선정합니다.

```python
def solution(genres, plays):
    d_items = {} # (id, count)
    d_sum = {}
    tid = -1

    for genre, count in zip(genres, plays):
        tid += 1

        if genre not in d_items:
            d_items[genre] = [(tid, count)]
            d_sum[genre] = count
        else:
            d_items[genre].append((tid, count))
            d_sum[genre] += count

    for genre in d_items.keys():
        d_items[genre].sort(key = lambda x: x[1], reverse = True)

    temp_rv = []
    for genre in d_items.keys():
        # d_sum을 기준으로 정렬하기 위해 데이터를 변환
        # (id, genre_sum, play_count)
        temp_rv.append((d_items[genre][0][0], d_sum[genre], d_items[genre][0][1]))
        if len(d_items[genre]) >= 2:
            temp_rv.append((d_items[genre][1][0], d_sum[genre], d_items[genre][1][1]))

    # 장르 총합(x[1]) 내림차순, 장르 내 곡 재생수(x[2]) 내림차순 정렬
    temp_rv.sort(key = lambda x: (x[1], x[2]), reverse = True)

    return [x[0] for x in temp_rv]
```
