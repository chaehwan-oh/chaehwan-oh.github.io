---
layout: post
subclass: post
title: "입문자를 위한 파이썬 프로그래밍 (7) 예외 처리"
date: 2019-05-17 00:00:07
tags: [python-programming]
excerpt: "파이썬 프로그래밍 입문, 예외 처리에 대한 설명입니다."
disqus: True
---

## 예외 처리

프로그래밍을 할 때 쉽지 않은 부분 중 하나, 예외처리입니다. 기본적인 내용을 살펴보고 넘어가도록 하겠습니다.

- 예외란?
- 예외 처리하기(try ~ except문)
- 예외 처리 구문의 제어 흐름
- finally 구문
- 예제

## 예외란?

먼저 예외란 무엇인지 살펴보겠습니다.

에러(error) : 프로그램 코드에 의해 수습될 수 없는 심각한 오류

예외(exception) : 프로그램 코드에 의해 수습될 수 있는 다소 미약한 오류

많은 도서와 설명글에서 에러와 예외를 위와 같이 정의하고 있습니다. 말이 딱딱하니 예제와 함께 살펴보도록 하겠습니다.

```python
print("hello world"))
```

위와 같이 프로그램을 작성하면 불필요한 ')' 하나 더 추가되어 아래와 같은 오류 메세지를 출력하고 바로 종료하게 됩니다. 이와 같은 경우를 에러라고 부릅니다.

```
    print("hello world"))
                        ^
SyntaxError: invalid syntax
```

다음은 예외입니다. 4를 0으로 나누어 오류 메세지를 출력하고 종료한 경우입니다.

```python
print(4/0)
```

```
    print(4/0)
ZeroDivisionError: division by zero
```

하지만 위의 경우는 문법 에러와 달리 프로그램 코드에 의해 수습이 가능합니다.
에러와 예외가 발생하면 프로그램은 비정상적으로 종료됩니다. 사용자가 원해서 종료되는 것이 아니라 비정상적으로 종료되므로 문제입니다. 그러므로 예외는 사전에 처리될 수 있도록 해야 합니다. 예외를 어떻게 처리할 수 있는지 살펴보도록 하겠습니다.

## try ~ except 구문

java, javascript 등 다른 프로그래밍 언어에서는 try ~ catch 문을 사용하지만 python의 경우 except가 catch를 대신합니다.

```python
try:
    # ...
except:
    # ...
```

기본적인 구조는 위와 같습니다. 수행하려는 명령어를 try 블럭에 작성하고 예외가 발생하면 처리하려는 명령을 except 문 내부에 작성하면 됩니다.

## 예외처리 구문에서의 제어 흐름

1. try문에 해당하는 명령을 순차적으로 실행합니다.
2. 예외가 발생하면 해당하는 except문을 찾아 내려갑니다.(일치하는 예외의 except문을 찾지 못하면 예외가 처리되지 않습니다.)
3. finally문을 추가해주었다면 finally문이 실행됩니다.(finally문은 예외 발생여부와 관계없이 실행됩니다.)

그렇다면 몇 가지 경우를 살펴보도록 하겠습니다.

먼저 해당 예외를 정상적으로 처리하는 경우입니다.

```python
cards = {"diamond": 4, "spade": 5}
try:
    print(cards["heart"])
except KeyError:
    print("Oops")
```

위 코드를 실행하면 dictionary에 조회할 key가 없는 경우이므로 KeyError에 해당해 다음과 같이 출력됩니다.

```
Output

Oops
```

다음은 예외 처리를 위한 except문을 작성했지만 예외가 해당하지 않는 경우입니다.

```python
cards = {"diamond": 4, "spade": 5}
try:
    print(cards["heart"])
except ValueError:
    print("Oops")
```

발생한 예외가 KeyError이므로 ValueError에 해당하지 않아 다음과 같이 예외가 처리되지 않습니다.

```
Output

    print(cards["heart"])
KeyError: 'heart
```

## finally문

finally문에는 예외 처리 시 예외 발생 여부와 관계없이 실행되는 명령어들이 들어갑니다. 예제를 살펴보도록 하겠습니다.

```python
cards = {"diamond": 4, "spade": 5}
try:
    print(cards["heart"])
except ValueError:
    print("Oops")
finally:
    print("Executed")
```

위 코드를 실행하면 다음과 같은 결과가 출력됩니다.

```
Output

Executed
Traceback (most recent call last):
    ...
KeyError: 'heart'
```

'Executed'라는 문장이 먼저 실행되고 예외가 처리되지 않았기 때문에 오류 메세지가 출력됩니다.

finally는 굳이 왜 있을까? 라고 생각하실 수 있습니다. 다음 예시를 살펴보도록 하겠습니다.
파일을 열어서 작업하는 예시입니다.

```python
try:
    f = open("test.txt",encoding = 'utf-8')
    # file operation
    f.close()
except:
    # exception handling
    f.close()
```

파일을 열어서 작업하면 닫아주어야 파일의 내용이 하드디스크에 온전히 기록되고 사용하던 자원이 반납될 수 있습니다. close()으로 명시적으로 닫아주지 않더라도 프로그램이 종료될 때 자동으로 파일을 닫아주지만 명시적으로 작성해주는 것이 좋습니다.

이렇게 작업하던 파일을 닫거나, 네트워크 연결을 끊는 등의 마무리 작업은 예외 발생 여부와 관계없이 실행되어야 하는 경우가 대부분입니다. try와 except문 내부에 똑같이 작성될 수 있습니다. 그러므로 불필요한 중복이 발생합니다. 이러한 문제를 해결하기 위해 finally문을 활용할 수 있습니다.

```python
try:
    f = open("test.txt",encoding = 'utf-8')
    # file operation
except:
    # exception handling
finally:
    f.close()
```

위와 같이 작성하면 중복 없이 finally문에만 마무리 작업의 코드가 작성될 수 있습니다. 예시의 경우 엄청 짧지만 마무리 작업의 코드가 길다면 finally문이 유용하게 사용될 수 있을 것입니다.

## try ~ except문 예시

컴퓨터가 1 ~ 100 사이의 임의의 숫자 하나를 정하고 사용자가 숫자를 입력해 맞추는 UP-DOWN 게임을 한다고 가정해보겠습니다.

먼저 사용자의 숫자 입력을 받아야할 것입니다. 이 때 사용자가 숫자가 아닌 엉뚱한 문자를 입력하는 경우를 생각해볼 수 있을 것입니다.

```python
def input_number():
    try:
        return int(input("Please enter a number: "))
    except ValueError:
        print("That was no valid number")
        return input_number()

number_input = input_number()
print(number_input)
```

```
Output

Please enter a number: hello
That was no valid number
Please enter a number: 3
3
```

위와 같이 작성하면 유효한 숫자입력을 할 때까지 다시 입력을 받고 정상적인 숫자 입력을 받으면 입력와 동시에 프로그램을 정상적으로 종료할 수 있습니다.

흐름을 조금 살펴보겠습니다. input 함수로 입력받은 문자열을 int 함수로 형변환하려고 시도합니다. 하지만 hello와 같이 숫자로 변환할 수 없는 부적절한 입력이 int 함수에 전달되어 ValueError가 발생합니다.(ValueError는 함수나 연산에 부적절한 인자가 전달되었을 때 발생합니다.) except문이 실행되어 "유효하지 않은 입력입니다."라는 문장을 출력하고 다시 재귀함수로 input_number()를 실행하므로 프로그램이 종료되지 않고 다시 입력을 받을 수 있습니다.

```python
def input_number():
    while True:
        number_input = input("Please enter a number: ")
        try:
            number_input = int(number_input)
            return number_input
        except:
            print("That was no valid number")

number_input = input_number()
print(number_input)
```

물론 위와 같이 작성하는 것도 가능합니다. 이번에 언급한 UP-DOWN 게임이나 숫자야구 게임 등의 콘솔 프로그램을 구현해보면서 예외처리를 연습해보시는 것도 좋을 것 같습니다. "숫자 입력이 아닌 입력이 들어온다면?", "만약 음수를 입력한다면?" 등 다양한 상황과 그에 따른 대응을 생각해보시면 큰 도움이 될 것이라고 생각합니다.
