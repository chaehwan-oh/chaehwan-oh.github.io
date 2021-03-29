---
layout: post
subclass: post
title: "파이썬 프로그래밍 입문(13) 시간 관리, 작업 예약, 다른 프로그램 실행"
date: 2019-05-17 00:00:13
tags: [python-programming, multithreading]
excerpt: "파이썬 프로그래밍 입문, mutlithreading에 대한 설명입니다."
disqus: True
---

## 시간 관리, 작업 예약, 다른 프로그램 실행

시간관리, 작업 예약, 다른 프로그램 실행에 대한 내용이었습니다. 멀티스레딩(multithreading) 프로그램에 대한 내용이 언급되어 조금 어렵더라도 간단하게 짚고 넘어가도록 하겠습니다.

## 스레드(thread)

multithreading이라는 말에서 thread는 영어로 실, 가닥, 줄기와 같은 의미가 있습니다. 그래서 thread는 실행 흐름에서의 하나의 줄기라고 생각하시면 좋을 것 같습니다.

```python
import threading, time

def take_a_nap():
    time.sleep(5)
    print('Wake up!')

print('Start of program')

thread = threading.Thread(target=take_a_nap)
thread.start()

print('End of program')
```

위 프로그램은 교재에서 언급되었던 프로그램입니다.

```
Output

Start of program
End of program
Wake up!
```

실행 결과는 위와 같습니다. Start of program이 출력되고, 바로 End of program이 출력되고, 5초 후에 Wake up!이 출력되었습니다. 일반적으로 함수가 호출되는 프로그램을 생각하면 함수 실행 종료 후 다음 명령문이 실행된다고 생각할 수 있을 것입니다.

하지만 위 코드의 경우 thread.start() 명령문이 실행되는 순간 하나의 실행 흐름이 더 생기는 것입니다. 그래서 원래의 실행흐름에서 이어서 End of program이 실행된 것입니다.

교재에서는 작업 예약 프로그램을 작성하기 위해 시간을 추적하는 실행흐름 하나, 사용자가 작업을 예약할 수 있는 실행흐름 하나 두 가지의 실행흐름을 가지고 가기 위함이었을 것입니다.

예를 들어 게임을 만드는데 캐릭터가 돌아다니는 동안 비는 상관 없이 배경에서 내리게 하고 싶다고 하면 스레드를 사용할 수 있을 것입니다. 이렇게 다양한 상황에서 스레드를 사용하는 것을 고려해볼 수 있을 것입니다.

스레드를 다룰 때는 주의사항이 있습니다. 여러 스레드를 사용할 때는 여러 스레드가 동시에 같은 변수에 대해 작업하도록 하지 않는 것이 필요합니다. 데이터의 불일치가 발생할 수 있기 때문입니다.

은행 계좌에 대한 처리를 하는 프로그램에 비유해보겠습니다. A 스레드에서 계좌 잔액이 500만원이라고 데이터를 읽어갔습니다. 그 후 B 스레드에서 500만원 송금 처리를 진행합니다.
A 스레드에서 300만원 입금 처리를 합니다. 최종 결과로는 300만원이 있어야 하지만 엉뚱하게 결과는 800만원이 됩니다. 이러한 문제 때문에 동기화 이슈 등이 발생하고 주의가 필요합니다.

스레드 자체에 대한 자세한 내용은 더 언급하지 않겠습니다. 운영체제를 학습하며 정리했던 글인데 궁금하시면 읽어보시는 것도 도움이 될 것이라고 생각합니다.

[스레드](https://chaehwan-oh.github.io/operating-system-part3/)

## 파이썬에서의 스레드(thread)

파이썬에서의 스레드는 순수한 병렬 프로그래밍의 수단이라고 보기 힘듭니다. 동시에 각각의 실행흐름이 실행되는 것이 아니라 각 실행흐름을 왔다갔다 하며 실행되는 방식이기 때문입니다. 이유는 다음 링크를 통해 글을 참고하시고 다음 코드도 같이 실행해보시면 도움이 될 것입니다.

[파이썬 멀티 스레딩](https://www.slideshare.net/yongho/2011-h3)

```python
# single_thread.py
from threading import Thread

def do_work(start, end, result):
    total = 0
    for i in range(start, end):
        total += i
    result.append(total)
    return


if __name__ == "__main__":
    START, END = 0, 20000000
    result = list()
    thread1 = Thread(target=do_work, args=(START, END, result))
    thread1.start()
    thread1.join()
    print("Result: ", sum(result))
```

```python
# double_thread.py
from threading import Thread

def do_work(start, end, result):
    total = 0
    for i in range(start, end):
        total += i
    result.append(total)
    return

if __name__ == "__main__":
    START, END = 0, 20000000
    result = list()
    thread1 = Thread(target=do_work, args=(START, int(END/2), result))
    thread2 = Thread(target=do_work, args=(int(END/2), END, result))
    thread1.start()
    thread2.start()
    thread1.join()
    thread2.join()
    print("Result: ", sum(result))
```

아래 코드 실행하시면 각 실행시간을 확인하고 비교하실 수 있습니다.

```
time python3 (파일명-ex: thread_test.py)
```

## subprocess 모듈

기본적으로 하나의 프로세스에서 다른 프로세스를 생성해 실행하는 것은 가능합니다. python의 subprocess 모듈을 통해서 이러한 작업을 하는 것도 가능합니다.

```python
import subprocess

subprocess.Popen('/usr/bin/gnome-calendar')
```

위와 같이 운영체제의 프로그램을 실행하는 것도 가능하고 아래와 같이 다른 python 프로그램을 실행하는 것도 가능합니다.

```python
import subprocess

subprocess.Popen(['/usr/bin/python3', '/home/user/test.py'])
```

[실행하려는 프로그램, 매개변수] 형태로 전달해주시면 됩니다. 위와 같이 사용하면 특정 시간에 다른 프로그램이 실행되도록 하는 등 다양한 프로그램을 손쉽게 구현하실 수 있으실 것입니다.
