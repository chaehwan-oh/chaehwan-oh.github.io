---
layout: post
subclass: post
title: "가장 긴 팰린드롬 구하기(프로그래머스)"
date: 2019-04-20
tags: [problem-solving, recursion, coding-test]
excerpt: "프로그래머스 가장 긴 팰린드롬 구하기 문제를 풀어봤습니다."
disqus: True
---

## 가장 긴 팰린드롬 구하기

## 문제 이해하기

앞뒤를 뒤집어도 똑같은 문자열을 팰린드롬이라고 합니다. 특정 문자열이 주어졌을 때 부분 문자열 중 가장 긴 팰린드롬의 길이를 구하는 문제입니다.

[가장 긴 팰린드롬](https://programmers.co.kr/learn/courses/30/lessons/12904)

## 계획 세우기

문제를 해결하는 단계는 다음과 같이 나눌 수 있습니다.

1. 가장 긴 길이의 팰린드롬을 구해야 하므로 후보가 되는 문자열의 모든 부분 문자열을 구합니다.

2. 각 부분 문자열에 대해 팰린드롬인지 아닌지를 판별합니다.

3. 기존의 가장 긴 팰린드롬의 길이와 비교하여 더 긴 길이의 팰린드롬의 길이가 있다면 값을 갱신해줍니다.

## 계획 실행하기

1. 모든 부분 문자열 구하기

```python
def get_substrings(input_string):
    substrings = []
    string_length = len(input_string)
    for start in range(string_length):
        for end in range(string_length):
            substring = input_string[start:end+1]
            substrings.append(substring)
    return substrings
```

가장 긴 팰린드롬을 구하는 문제이므로 빠짐없이 후보가 되는 문자열을 구합니다.

2. 팰린드롬 여부 식별하기

```python
def is_palindrome(substring):
    if len(substring) <= 1:
        return True
    else:
        return substring[0] == substring[-1] and is_palindrome(substring[1:-1])
```

해당 문제의 핵심입니다. 큰 문제와 작은 문제의 형태가 같다는 것을 생각하면 재귀 개념을 사용할 수 있습니다. 큰 문자열은 해당 문자열이 팰린드롬인지 판별하는 것입니다. 부분 문제는 양끝 두 문자를 제외한 문자열이 팰린드롬인지 식별하는 것입니다. 양끝 문자를 제외하면서 부분 문자열을 계속 확인했을 때 내부의 문자열이 팰린드롬이면 팰린드롬입니다. 위와 같이 재귀함수로 코드를 작성할 수 있습니다.

3. 가장 긴 팰린드롬의 길이 구하기

```python
def get_longest_palindrome_length(input_string):
    answer = 0
    substrings = get_substrings(input_string)
    for substring in substrings:
        substring_length = len(substring)
        if is_palindrome(substring) and answer < substring_length:
            answer = substring_length
    return answer
```

위와 같이 작성하면 가장 긴 팰린드롬의 길이를 구할 수 있습니다.

## 되돌아보기

문제의 핵심은 '팰린드롬을 판별할 때 큰 문제와 부분 문제의 형태가 같다는 것을 인식할 수 있는가?'였다고 생각합니다. 이러한 개념을 활용하면 다양한 문제를 쉽게 할 수 있을 것이라고 생각합니다.
