---
layout: post
subclass: post
title: "숫자야구(프로그래머스)"
date: 2019-04-20
tags: [problem-solving, brute-force, coding-test]
excerpt: "프로그래머스 숫자야구 문제를 풀어봤습니다."
disqus: True
---

## 숫자야구(프로그래머스)

## 문제 이해하기

[숫자야구](https://programmers.co.kr/learn/courses/30/lessons/42841)

한 사람이 서로 다른 1 ~ 9까지 3자리의 임의의 숫자를 정하고 다른 한 사람이 3자리 숫자를 불러서 판정결과를 확인합니다. 그 결과를 토대로 다시 예측해 상대가 정한 숫자를 맞추는 게임입니다. 이 문제의 경우 컴퓨터가 하나의 숫자를 정했다고 가정하고 주어진 결과가 나왔을 때 어떤 숫자들이 가능하겠는가를 묻는 문제입니다.

주어진 정보 : 각 예측수와 그에 따른 판정결과
목표 : 컴퓨터가 정한 숫자로 가능한 모든 숫자

## 계획 세우기

판정결과를 어떻게 조합하면 방법이 나올까 했는데 쉽지 않습니다. 그래서 생각한 방법은 각 자리가 서로 다른 모든 3자리 숫자를 하나씩 예측수와 비교했을 때 주어진 판정결과가 나오는지 확인하는 것입니다.

단계를 하나씩 나누어보면 다음과 같습니다.

1. 모든 세 자리 수를 하나씩 확인한다.

- 세 자리 수를 각 자리에 중복되는 수가 있으면 건너뛴다.

- 1 ~ 9 사이의 숫자가 아닌 수가 포함되어 있으면 건너뛴다.

2. 하나씩 확인하며 예측수와 비교하여 스트라이크와 볼을 구한다.

3. 주어진 입력의 스트라이크와 볼과 일치하는지를 비교한다.

4. 모든 각 자리의 수가 다른 세자리 수에 대해 2번, 3번 작업을 수행해준다.

- 주어진 스트라이크와 볼 입력과 모두 일치하면 정답에 추가한다.

5. 추가된 정답의 개수를 반환한다.

## 계획 실행하기

위의 계획한 단계를 하나의 역할을 함수로 바꾸며 작업을 진행했습니다. 첫 단계 조건에 속하는 모든 세 자리 수를 구하는 작업을 진행했습니다.

1. 조건에 속하는 모든 세 자리 수 구하기

```python
def get_three_digit_numbers():
    numbers = []
    for number in range(100, 1000):
        if has_repeated_number(number) or not is_in_range(number):
            continue
        numbers.append(number)
    return numbers

def has_repeated_number(number):
    digits = number_to_digits(number)
    return len(set(digits)) != len(digits)

def is_in_range(number):
    digits = number_to_digits(number)
    for digit in digits:
        if digit < 1 or digit > 9:
            return False
    return True

def number_to_digits(number):
    return [int(digit) for digit in str(number)]
```

조금 길어지긴 했지만 함수 하나가 각 단계의 역할을 나타내도록 하려고 해봤습니다. itertools 모듈의 permutations 함수를 사용했다면 훨씬 간단하고 간결하게 위의 작업을 할 수 있었을 것입니다.

이렇게 구한 숫자에 대해 판정결과를 확인하는 큰 틀은 다음과 같이 될 것입니다..

2. 결과 판정하기

```python
def get_answer_count(hints):
    answers = []
    three_digit_numbers = get_three_digit_numbers()
    for number in three_digit_numbers:
        if is_answer(number, hints):
            answers.append(number)
    return len(answers)

def is_answer(target_number, hints):
    for hint in hints:
        hint_number, strike, ball = hint
        strike_count, ball_count = get_score(target_number, hint_number)
        if not (strike == strike_count and ball == ball_count):
            return False
    return True
```

판정결과를 비교해서 해당 숫자가 정답수인지 하나씩 확인합니다. 이제 점수 판별만 하면 될 것입니다.

3. 점수 구하기

```python
def get_score(target_number, hint_number):
    strike_count = 0
    ball_count = 0
    target = number_to_digits(target_number)
    hint = number_to_digits(hint_number)
    for i, target_value in enumerate(target):
        for j, hint_value in enumerate(hint):
            if (i == j) and (target_value == hint_value):
                strike_count += 1
            if (i != j) and (target_value == hint_value):
                ball_count += 1
    return (strike_count, ball_count)
```

간단하게 작성하고 싶었는데 쉽지 않군요. 이렇게 하면 간단히 문제를 해결할 수 있습니다.

## 되돌아보기

문제의 핵심은 "'모든 3자리 수에 대해 하나씩 확인한다.'라는 생각을 할 수 있는가?"였다고 생각합니다. 컴퓨터는 상당히 빠르기 때문에 문제의 제한을 확인하고 그 제약사항을 어기지 않는다면 간단하게 해결하는 것이 현명하다고 생각합니다. 제한을 어기지 않는 선에서 최대한 문제를 간단한 문제로 바꾸어 해결하는 연습을 해나갈 생각입니다.
