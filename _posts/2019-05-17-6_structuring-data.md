---
layout: post
subclass: post
title: "입문자를 위한 파이썬 프로그래밍 입문 (6) 구조형 자료(Structuring data)"
date: 2019-05-17 00:00:06
tags: [python-programming] 
excerpt: "파이썬 프로그래밍 입문, 구조형 자료에 대한 설명입니다."
disqus: True
---

## 구조형 자료(Structuring data)

구조형 자료는 딕셔너리, 튜플, 리스트 등 단일값이 아닌 그들의 묶음을 저장하는 자료형을 말합니다. 몇 가지 유의할 점을 짚고 넘어가도록 하겠습니다.

- 튜플과 리스트의 차이
- 시퀀스와 다중 대입문
- 함수와 구조형 자료의 사용
- 딕셔너리의 특징

## 튜플과 리스트의 차이

튜플과 리스트에는 중요한 차이가 있습니다. 리스트는 변경이 가능하지만 튜플은 변경과 삭제가 불가능하다는 것입니다.

```python
fruits = ('apple', 'grape')
del fruits[0]
```

위와 같이 작성하면 아래와 같이 오류가 출력됩니다.

```
del fruits[0]
TypeError: 'tuple' object doesn't support item deletion
```

다음 코드도 마찬가지입니다. 삭제를 시도하면 오류가 출력됩니다.

```python
fruits = ('apple', 'grape')
fruits[1] = 'strawberry'
```

```
fruits[1] = 'strawberry'
TypeError: 'tuple' object does not support item assignment
```

앞서 변경 가능한 객체 예를 들어 리스트의 경우 해당 값이 저장된 위치가 함수에 전달된다고 배웠습니다. 반면, 정수형 자료와 같이 변경이 불가능한 자료는 값이 복사되어 전달되었습니다. 튜플의 경우 변경이 불가능한 자료이므로 저장된 위치가 아닌 값이 함수에 전달됩니다.

참고로 문자열도 변경 불가능한 객체이고 함수 내부에서 조작해도 아무런 영향이 없다는 것을 기억하도록 합시다.

## 시퀀스와 다중 대입문

튜플, 문자열 등의 자료를 시퀀스(sequence) 자료형이라고 부릅니다. 이 시퀀스 자료형은 다중 대입문으로 각 요소를 뽑는 것이 가능합니다.

```python
x, y = (3, 4)
print(x)
print(y)
```

```
Output

3
4
```

함수를 작성하는데 여러 값을 반환해야 한다고 가정해봅시다.

```python
def get_score():
    # ...
    strikeCount = 2
    ballCount = 1
    return (strikeCount, ballCount)

strike, ball = get_score()
print('strike', strike)
print('ball', ball)
```

```
strike 2
ball 1
```

너무 끼워맞춘 듯하지만 숫자야구게임에서 strike와 ball의 값을 반환하고 싶다고 가정해봅시다. 그렇다면 위 코드처럼 튜플로 값을 반환하고 다중 대입문으로 값을 하나씩 풀어서 대입시키면 됩니다.

문자열의 경우도 다중 대입문이 가능합니다.

```python
a, b, c = 'abc'
print(a)
print(b)
print(c)
```

```
a
b
c
```

또 두 변수의 값을 바꾸는 것도 다음과 같이 간결하게 작성 가능합니다.

```python
x = 10
y = 15
x, y = y, x
print('x', x)
print('y', y)
```

```
x 15
y 10
```

## 함수와 구조형 자료의 사용

함수를 매개변수로 넘겨 구조형 자료와 함께 유용하게 사용하실 수 있습니다. 다음 예제를 살펴보도록 하겠습니다.

```python
def apply_to_each(input_list, input_function):
    for i in range(len(input_list)):
        input_list[i] = input_function(input_list[i])

negative_numbers = [-1, -2, -3, -4]
apply_to_each(negative_numbers, abs)
print(type(apply_to_each))
print(negative_numbers)
```

```
Output

<class 'function'>
[1, 2, 3, 4]
```

javascript 등을 이미 사용해보셨다면 너무 자연스러운 개념이지만 프로그래밍 언어를 처음 접하거나 C 언어 등의 언어만 사용해왔다면 조금 신기할 수 있습니다. 함수 역시 자료형이 있고 객체입니다. 그래서 함수의 인자로 사용 가능하고 리스트의 요소로도 사용 가능합니다.(객체에 대해서는 다음에 설명하도록 하겠습니다.)

## 딕셔너리(dictionary)의 특징

딕셔너리에 대해 이미 학습했지만 조금 더 잘 사용하기 위해 몇 가지 특징을 살펴보도록 하겠습니다.

1. 변경 가능한 객체는 무엇이든 dictionary의 key로 사용 가능하다.

```python
lotto_result = {
    # ...
    ('sixth', 5000): 1
}

print(lotto_result[('sixth', 5000)])
```

```
Output

1
```

조금 이상하고 끼워맞춘 듯하지만 가능하다는 것을 보여드리기 위한 예제입니다. 로또 복권을 여러 개 구입한 결과 상금이 5000원인 6등에 하나 당첨되었다고 저장하고 싶다면 위와 같이 저장할 수 있습니다.

2. 딕셔너리 변수도 리스트와 같이 값이 저장된 위치를 가리키는 참조변수다.

딕셔너리도 리스트와 같이 값이 저장된 위치가 함수로 전달됩니다. 그러므로 딕셔너리 역시 리스트처럼 의도하지 않게 해당 자료에 영향을 줄 수 있다는 것을 기억해야 합니다.

3. 리스트와 비교했을 때 딕셔너리의 장점

```python
my_fruits = [['apple', 1], ['grape', 3], ['strawberry', 5]]
```

굳이 나타내려고 한다면 리스트도 key-value의 자료를 표현할 수 있을 것입니다. 하지만 위와 같이 나타내는 것은 비효율적입니다. 특정 정보를 검색할 때 리스트를 순회하면서 하나씩 비교하며 검색해야 하기 때문입니다.

반면, 딕셔너리는 input_list[5] 와 같이 바로 인덱스로 접근하는 것처럼 한 번만에 데이터를 조회할 수 있습니다.

딕셔너리는 내부적으로 해싱 기법을 사용하기 때문입니다. 해싱 기법이 꼭 무엇인지 알아야 하는 것은 아닙니다. key-value의 데이터를 저장할 때는 딕셔너리를 사용하는 것이 성능 면에서도 더욱 적절하다는 것을 기억하시면 됩니다.

딕셔너리는 fruits['apple'] 와 같은 표현을 했을 때 'apple'을 숫자값으로 변환해 숫자 인덱스로 값을 빠르게 조회할 수 있기 때문입니다.

조금 더 궁금하시면 다음 링크를 참고하시기 바랍니다.

[해싱](http://coinhy.io/index.php/2018/06/27/column/)
