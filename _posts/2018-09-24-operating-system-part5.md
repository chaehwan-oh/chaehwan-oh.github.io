---
layout: post
subclass: post
title: "운영체제 (5) Process Synchronization"
date: 2018-09-24 00:00:05
tags: [dudaji, computer-science, operating_system]
excerpt: "운영체제 프로세스 동기화(Process Synchronization)에 대한 정리입니다."
disqus: True
---

## 프로세스 동기화란?

프로세스 동기화 : 둘 이상의 프로세스가 공유 자원에 접근할 때 데이터의 일관성이 깨지는 문제를
방지하기 위해 순서를 조정해주는 메커니즘

## 왜 프로세스 동기화가 필요한가?

![concurrent_access_to_count](/images/operating_system/concurrent_access_to_count.png)

예를 들어 두 개의 프로세스가 있는데 하나의 프로세스(A)는 count라는 변수에 대해
count++ 명령을 실행하고 다른 하나의 프로세스(B)는 count 변수에 대해 count--를
실행한다고 생각해보자.

이 때 count 변수의 값이 원래 5였다고 한다면 두 개의 명령이 실행되고 났을 때
count 변수의 값은 5가 되어야 한다. 하지만 count 변수의 값은 4가 될 수도 있고
6이 될 수도 있다.

### 왜 count 값은 4가 될 수도 있고 6이 될 수도 있는 것일까?

**고급 언어(high-level)의 문장 하나하나가 단일 instruction은 아니여서 단일 instruction의
실행 도중에 CPU의 제어권이 넘어갈 수 있다.**

A 프로세스가 실행하는 count++는 다음의 instruction으로 나눌 수 있다.

```
1) load(register = count)
2) add(register = register + 1)
3) store(count = register)
```

한편, B 프로세스가 실행하는 count--는 다음의 instruction으로 나눌 수 있다.

```
1) load(register = count)
2) subtract(register = register - 1)
3) store(count = register)
```

두 프로세스가 다음과 같은 순서로 실행된다고 생각해보자.

```
1) Process A : load (register = 5)
-> 이때 A 프로세스의 1번 load 명령어까지 실행된 다음 B 프로세스로
   CPU의 제어권이 넘어갔다고 생각해보자.
2) Process B : load (register = 5)
3) Process B : substract (register = 4)
4) Process B : store (register = 4)
5) Process A : add (register = 6)
-> Process B 에 의해 4 라는 값이 store되었지만 CPU의 제어권이
   B 프로세스로 넘어가기 전에 load 되었던 값은 5이고 5 라는 값이
   복원된 다음 복원된 레지스터 값에 대해 add 연산이 실행된다.
6) Process A : store (register = 6)
-> 최종적으로 count 변수의 값은 6이 된다.
```

마지막에 어떤 프로세스가 CPU의 제어권을 가져가느냐에 따라 4가 될 수도 있고 6이 될 수도 있다.

**이렇게 count 변수의 값이 일관성 없는, 부정확한 결과가 되는 이유는 두 개의 프로세스가 count라는
공유 데이터(Shared data)에 동시에 접근했기 때문이다.** 공유 데이터에 접근하거나 커널 모드로 들어가는 것이
아니라면 문맥 교환(context switch)가 일어나도 문제가 되지 않는다. (커널 모드에 들어가는 것이 문제가 되는
이유는 시스템이 공유하는 커널 자료에 접근하기 때문이다.)

### 경쟁 조건(race condition)

위와 같이 동시에 여러 개의 프로세스가 공유 데이터에 접근하여 조작하고, 그 실행결과가 접근 순서에 따라 달라지는
상황을 '경쟁 조건(race condition)이라고 한다.

이러한 경쟁 조건에서의 데이터 불일치 문제를 해결하기 위해서는 결국 한 순간에 하나의 프로세스만이 공유 데이터를
조작하도록 실행 순서를 정해주는 메커니즘이 필요하다. 이를 통해 프로세스들이 동기화되도록 할 필요가 있다.

## 임계영역 문제(The Critical Section Problem)

경쟁 조건에서의 데이터 불일치 문제를 해결하기 위해서는 프로세스들을 동기화해주어야 하는데 이 프로세스 동기화에서
중요한 문제가 바로 '임계영역 문제(The Critical Section Problem)'이다.

임계영역 문제(The Critical Section Problem) : 임계영역으로 지정되어야 할 코드 영역이 임계영역으로 지정되지 않았을 때
발생할 수 있는 문제

![critical_section](/images/operating_system/critical_section.png)

위의 그림에서 Critical Section은 공유 데이터가 있는 count 변수가 아니라 공유 데이터에 접근하는 A 프로세스와
B 프로세스의 코드다. Critical Section은 '공유 데이터에 접근하는 코드'라고 말할 수 있다.

임계영역 문제가 발생하지 않기 위해 중요한 사실은 "하나의 프로세스가 자신의 임계영역에서 실행하는 동안에는
다른 프로세스들은 그들의 임계영역에 들어갈 수 없다"는 사실이다.

```cpp
do {
    // entry section
        // critical section
    // exit section
        // remainder section
} while (true)
```

위의 사실이 지켜지기 위해서는 진입 영역(entry section)에서는 해당 프로세스가 진입 가능한지 확인한 뒤 진입할 때는
다른 프로세스들이 진입할 수 없도록 하는 코드가 필요하고, 퇴출 영역(exit section)에서는 공유 데이터에 접근한 뒤
다른 프로세스가 접근할 수 있도록 하는 코드가 필요하다.

또한, 임계영역 문제에 대한 해결책은 3가지 조건을 만족시켜야 한다.

### 1) Mutual Exclusion(상호 배제)

특정 프로세스 Pi 가 자신의 임게영역에서 실행된다면, 다른 프로세스는 자신의 임계영역에서 실행될 수 없다.

### 2) Progress(진행)

Critical Section에서 실행되고 있는 프로세스가 없고 Critical Section에 진입하려는 프로세스가 있다면
그 프로세스는 Critical Section에 진입할 수 있어야 한다.

### 3) Bounded Waiting(유한 대기)

특정 프로세스가 Critical Section에 진입하려고 할 때 다른 프로세스들에 의해 계속 진입하지 못하는 상황이
발생해서는 안된다.

## 피터슨의 해결안(Peterson's Solution)

피터슨의 해결안은 임계영역 문제의 해결책이 가져야 하는 3가지 조건을 만족시키는 소프트웨어 기반의 해결책이다.
피터슨의 해결안은 살펴보기 전에 임계영역 문제를 해결하기 위한 2가지 방법을 먼저 살펴보면 피터슨의 해결안을
이해하는데 큰 도움이 된다.

### 소프트웨어 기반 해결책1 : turn 변수 사용

첫 번째 방법은 turn 변수를 통해 두 프로세스 사이에 임계영역에 진입할 수 있는 차례를 정해주는 방법이다.
먼저 다음과 같이 초기화해준다.

```cpp
int turn;
turn = 0;
```

그리고 Process 0의 코드를 다음과 같이 작성한다.

```cpp
do {
    while (turn != 0);
    // critical section
    turn = 1;
    // remainder section
} while(1);
```

Process 1의 코드는 다음과 같이 작성해준다.

```cpp
do {
    while (turn != 1);
    // critical section
    turn = 0;
    // remainder section
} while(1);
```

첫 번째 방법을 임계영역 문제의 해결책이 가져야 하는 3가지 조건을 기준으로 살펴보자.

#### mutual exclusion(상호 배제)

먼저 상호 배제 조건은 만족되고 있는 것을 알 수 있다. 각 프로세스가 자신의 turn이 아니면
critical section에 진입하기 전에 while문을 계속 반복 실행하면서 다음 코드를 실행할 수 없기
때문에 상호 배제 조건이 만족되고 있다.

#### progress(진행)

progress 조건의 경우 만족되고 있지 않다. 이유는 critical section은 무조건 두 프로세스가 교대로
들어갈 수 있다. 그런데 critical section에 진입하는 빈도가 균일한 것은 아니다!

### 소프트웨어 기반 해결책2 : flag 배열 사용

두 번째 방법은 무조건 하나의 프로세스가 turn을 가지도록 하는 것이 아니라 flag 배열을 둠으로써
critical section에 진입하겠다는 의사를 표현해 진입이 필요한 프로세스가 진입할 수 있도록 하는
방법이다.

```cpp
boolean flag[2];
flag[i] = false;
flag[j] = false;
```

Process A의 코드를 다음과 같이 작성해준다.

```cpp
do {
    flag[i] = true;
    while (flag[j]);
    // critical section
    flag[i] = false;
    // remainder section
} while(1);
```

Process B의 코드는 다음과 같이 작성한다.

```cpp
do {
    flag[j] = true;
    while (flag[i]);
    // critical section
    flag[j] = false;
    // remainder section
} while(1);
```

두 번째 방법도 임계 영역 문제의 해결책이 가져야 하는 3가지 조건을 모두 만족시키지는 못 한다.
예를 들어 Process A에서 flag[i] = true; 명령까지 실행된 다음에 Process B로 CPU가 넘어갔다고 생각해보자.
그렇다면 두 프로세스 모두 critical section에 진입하지 못한다.

### 소프트웨어 기반 해결책3 : Peterson's Algorithm

피터슨의 해결안은 turn 변수를 이용하는 1번 방법과 flag 배열을 활용하는 2번 방법을 적절히 활용하여
임계영역 문제의 해결책이 가져야 하는 3가지 조건을 충족시킨다.

먼저 두 프로세스는 동기화를 위해 다음의 데이터를 공유해야 한다.

```cpp
int turn;
bool flag[2];
```

Process A의 코드

```cpp
do {
    flag[i] = true; // true로 진입할 준비가 되었음을 밝힌다.
    /* 다음 명령어는 다른 프로세스가 진입의사가 있을 때 진입할 수
    있도록 하기 위해 turn을 j로 지정한다. */
    turn = j;
    while (flag[j] && turn == j);
    // critical section
    flag[i] = false;
    // remainder section
} while(1);
```

Process B의 코드

```cpp
do {
    flag[j] = true;
    turn = i;
    while (flag[i] && turn == i);
    // critical section
    flag[j] = false;
    // remainder section
} while(1);
```

flag 배열로 진입의사를 표현하고 turn 값에 의해 결국 어떤 어떤 프로세스가
임계영역으로 진입할 것인지를 결정하는 방식이다.

#### 상호 배제 조건

flag배열의 값은 모두 true이더라도 turn 변수 값은 하나의 값만을
가질 수 있기 때문에 상호 배제는 지켜진다.

#### 진행, 유한 대기 조건

프로세스 A는 프로세스 B의 flag 값이 set 되어 있지 않으면 turn을 따지지 않고도 진입할
수 있고, flag 값이 set 되어 있더라도 turn 변수의 값에 따라 진입하는 것이 가능하다.
또한, 하나의 프로세스는 진입 후 critical section에서 벗어나면서 flag 변수를 false로
바꾸기 때문에 진행, 유한 대기 조건을 모두 만족한다.

참고로 이 코드에도 문제점은 있다. 바로 'Busy Waiting'이고 다른 말로는 'spinlock'이다!
즉, while문을 계속 돌면서 lock을 거는 방식이기 때문에 계속 CPU와 memory를 사용하면서
기다린다는 문제점이 있다.

소프트웨어 기반 해결책에서의 문제점은 lock를 걸고 푸는 코드 내에서도 경쟁 조건이 발생할
가능성이 있다는 것이다. 그리고 프로세서가 하나인 상황이 아니라 멀티 프로세서 환경이라면
상황은 더 복잡해진다. 그래서 필요한 것이 동기화 하드웨어다.

## 동기화 하드웨어

임계 영역 문제의 해결책으로 간단하게는 lock이 필요하다고 말할 수 있다. 그런데 하나의 프로세서를 가지는 구조라면
공유 데이터에 접근할 때 인터럽트가 불가능하도록 하는 비선점형 방식으로 문제를 해결할 수 있다. 하지만 멀티
프로세서의 구조라면 효율이 상당히 떨어진다. 다른 모든 CPU에 대해 인터럽트가 불가능하다는 메세지가 전달되어야
하기 때문이다. 임계 영역에 진입하는 것이 상당히 지연되고 인터럽트에 의해 클록이 갱신되면 시스템 클록에 대한 영향도
고려해야 한다.

이 때까지의 소프트웨어 기반 해결책의 문제점은 결국 하나의 문장이 실행되는 도중에 제어권이 넘어갈 수 있다는 것이었다.
그러므로 단일 인스트럭션 내에 읽고 쓰는 것이 가능하면 문제될 것이 없다. 인스트럭션 단위로 인터럽트가 들어오기 때문이다.

현대 하드웨어에서는 인터럽트되지 않는 단일 인스트럭션 단위로 읽고 쓸 수 있는 get_and_set 연산이 지원된다.
이렇게 되면 문제가 상당히 간단하게 해결 가능하다.

get_and_set(a) 연산은 a의 현재 값을 읽고 a를 1로 바꾸는 연산이다.
만약 a가 0이면 0이 읽히고 a는 1이 된다.
만약 a가 1이면 1이 읽히고 a는 다시 1로 세팅된다.

```cpp
bool lock = false;
do {
    while (get_and_set(lock));
    // critical section
    lock = false;
    // remainder section
}
```

위 코드를 살펴보면 lock이 0 즉 false이면 읽는 값이 0이니까 통과하고 다음 instruction으로 넘어가면서
lock은 1 즉 true로 세팅되어 다른 프로세스는 해당 프로세스가 진입한 동안 critical section에 진입할 수
없다. 만약 lock이 True라면 읽었을 때 1이므로 통과하지 못하고 다시 세팅되는 값도 1이므로 문제 될 것이 없다.

그런데 이러한 하드웨어 기반의 해결책 역시도 프로그래머에게 복잡할 수 있고 어플리케이션 프로그래머는 이런
하드웨어 기반의 해결책을 활용할 수 없다. 그래서 운영체제 설계자들이 이러한 문제를 해결하기 위해 단순한 동기화
도구들을 제공하는데 가장 기본적인 것이 'mutex lock'이다.(mutex는 mutual exclusion의 약어다.)

## 뮤텍스 락(mutex lock)

뮤텍스 락(mutex lock) :

mutex lock은, available이라는 boolean 변수와 acquire() 와 release() 두 가지 연산을 가진다.

available 변수는 critical section 에 진입 가능한지 여부를 의미하고, acquire 연산은 lock을
획득하는 행위, release 연산은 lock을 반납하는 행위를 의미한다.

```cpp
void acquire(){
    while (!available);
    /* busy wait */
    available = false;
}

void release(){
    available = true;
}
```

이러한 연산들을 활용하여 critical section 문제를 해결할 수 있다.

```cpp
do {
    // acquire lock
        // critical section
    // release lock
        // remainder section
} while (true);
```

주의할 점은 이러한 뮤텍스 락은 앞서 언급된 동기화 하드웨어를 활용해 원자적으로 실행되어야 한다는 것이다.

이와 같이 구현했을 때 가장 큰 문제점은 바로 busy waiting이다. 또는 while loop를 계속 돌면서 critical section의
진입을 막는다고 하여 spinlock이라고도 한다. 이렇게 루프를 계속 돌면서 진입을 저지하는 방식은 다중 프로그래밍 시스템
에서 상당한 문제가 있다. CPU와 memory를 계속 사용하면서 진입을 막기 때문에 굉장히 비효율적으로 CPU를 사용한다는 문제점이
있다.

반면, 장점도 있는데 lock에 막혀 기다릴 때 문맥 교환이 필요 없다는 것이다. 문맥 교환도 상당한 오버헤드라고 할 수 있기 때문에
장점이라고 할 수 있다. 그러므로 하나의 프로세스가 lock을 잡고 있는 시간 즉 critical section의 길이가 길지 않을 때 유용하게
사용될 수 있다.

mutex lock은 가장 단순한 형태의 동기화 도구이다. mutex lock보다 다양하게 사용될 수 있는 동기화 도구가 있는데 바로
'세마포어(Semaphore)'다.

## 세마포어(Semaphore)

### 세마포어란?

세마포어(Semaphore) : 임계영역 문제를 해결하기 위한 동기화 도구로 프로그래머에게 제공되는 추상 자료형

세마포어는, Semaphores S 라는 정수 변수와 wait() 와 signal() 두 가지 원자적인 연산을 가진다.

```cpp
void wait(S){
    while (S <= 0); // busy wait
    S--;
}

void signal(S){
    S++;
}
```

세마포어의 종류는 2가지다. binary semaphore와 counting semaphore가 있다. binary semaphore는 0과 1의 값을
가지고, mutex와 유사하게 동작한다. critical section에 진입하면서 lock을 걸고 나오면서 lock을 푸는 역할을 한다.

counting semaphore 에서의 Semaphores S 정수 변수는 가용 자원의 개수를 의미한다.
wait(S) 연산은 공유 자원을 획득하기 위해 기다리다가 자원을 획득하는 연산이다. signal(S) 연산은 가용 자원을 반납하면서
기다리고 있는 프로세스에게 신호를 주는 연산이다.

정리하면 Semaphores S는 가용 자원의 개수를 표현하기 때문에 S의 값이 5라면 wait 연산을 한 다음에는 자원의 개수가 4가 된다.
5라는 값을 가지면 다섯 개의 프로세스가 동시에 가져갈 수 있다는 뜻이다.

반면, signal 연산은 S 값이 5에서 acquire 연산으로 4로 줄어들어도 signal 연산을 하면 다시 4개에서 5개가 된다.

하지만 위 semaphore 코드에도 문제가 있는데 mutex lock과 마찬가지로 Busy waiting 즉 spinlock를 하게 된다는 것이다.
그러므로 이러한 문제점을 해결하기 위해 Block & wakeup 방식으로 Semaphore를 구현한다.

### Semaphore의 구현 : Block & Wakeup 방식

CPU 스케줄링에서는 특정 Process가 수행되다가 I/O 작업을 해야 하면 running에서 blocked 상태로 바뀐다.
blocked가 되면 device queue에서 기다리다가 돌아오는 방식이다. 여기서도 유사하게 blocked 상태로 전환되어 sleep하고
있다가 공유 데이터를 가지고 있던 프로세스가 반납하면 깨어나서 공유 데이터에 접근하는 방식이다.

```cpp
typedef struct
{
    int value; /* semaphore */
    struct process *list;
} semaphore;
```

block 연산과 wakeup 연산은 운영체제의 시스템 콜로 제공된다. block 연산은 block을 호출한 프로세스를
suspend 즉 중단시키고 프로세스의 PCB를 semaphore에 대한 waiting queue에 넣는다. wakeup 연산은 block된
프로세스 P를 wakeup 시키는 연산이다. 이 프로세스의 PCB를 ready queue로 옮긴다.

![block_and_wakeup](/images/operating_system/block_and_wakeup.png)

위의 그림과 같은 방식으로 저장된다.

```cpp
void wait(semaphore *S){
    S->value--;
    if (S->value < 0){ /* negative; I can not enter; 자원의 여분이 없는 상태 */
        // add this process to S->list;
        block();
    }
}

void signal(semaphore *S){
    S->value++;
    if (S.value <= 0){
        // remove a process P from S->list;
        wakeup(P);
    }
}
```

실행하는 코드에서 주의해야 할 점은 0이상일 때 자원의 여분이 있는 것이 아니라는 점이다. S의 값을 다 빼고 잠들었다.
그러므로 하나의 프로세스가 자원을 내놓았는데도 0 이하라는 말은 누군가가 잠들어 있다는 말이다. 즉, S->value가 음수면
그 절대값은 가용 자원을 기다리면서 잠들어 있는 프로세스의 수를 의미한다.

### Busy waiting vs Block and wakeup

어떤 방식이 더 낫다고 할 수 있을까? 대개 block and wakeup 방식이 더 효율적이다. CPU를 효율적으로 사용할 수 있기 때문이다.
하지만 Block and wakeup이 무조건 낫다고 할 수는 없다. Block and wakeup 역시 오버헤드가 있다. 정리하면 Critical Section의
길이가 길면 Block and wakeup이 적당하고 Critical Section의 길이가 짧으면 Block and wakeup의 오버헤드가 Busy-waiting의 오버헤드보다 더 커질 수 있다는 것을 고려해야 한다.

Semaphore를 살펴봤고 Semaphore를 구현하는 2가지 방식도 살펴봤는데 Semaphore를 사용할 때는 주의할 점이 있다.

### Semaphore를 사용할 때 주의할 점

Semaphore를 사용할 때는 원치 않는 문제가 생길 수 있다. 바로 Deadlock 과 Starvation 이다.

첫 번째 생길 수 있는 문제점은 Deadlock이다. 예를 들어 하드디스크 파일 A 에서 읽어서 B 에 쓰고 싶은 상황이라면?
간단하게 자원 2개를 획득해서 한꺼번에 처리해야 하는 상황이라고 볼 수 있다.
Semaphore S 와 Q 를 획득해서 한꺼번에 처리해야 한다면 그리고 그런 프로세스가 2개 있다면 어떻게 처리할 수 있을까?

```
P0             P1

wait(S);       wait(Q);
wait(Q);       wait(S);

signal(S);     signal(Q);
signal(Q);     signal(S);
```

일단 P0이 wait(S)로 획득한 다음 CPU의 제어권이 P1에 넘어간다면?
그리고 P1으로 넘어가서 P1은 wait(Q)로 Q를 획득한다.
그러고 나서 P1이 wait(S)로 S를 획득하려고 하면 P0이 먼저 획득했기 때문에
획득할 수 없고 기다려야 한다.

언제까지? 영원히 획득할 수 없고 기다려야 한다.
P0는 Q까지 획득해서 처리한 다음 반납한다. 이렇게 서로 하나씩 쥐고 안 놓는 상황을
'Deadlock'(교착상태)라고 한다.

그렇다면 어떻게 해결할 수 있을까?
자원을 획득하는 순서를 맞추어 주어야 한다. 아래와 같이 순서를 조정해주면 해결된다.

```
P0             P1

wait(S);       wait(S);
wait(Q);       wait(Q);

signal(S);     signal(Q);
signal(Q);     signal(S);
```

두 번쨰 문제점은 Starvation 이다.
특정 프로세스가 자원을 얻지 못하고 무한정 기다려야 하는 상황이다.
예를 들면 식사하는 철학자 문제를 예로 들 수 있다.

![dining_philosophers](/images/operating_system/dining_philosophers.png)

왼쪽과 오른쪽 젓가락을 짚어야 식사를 할 수 있는데 왼쪽 사람과 오른쪽 사람이 계속 번갈아가면서
식사를 하면 기아 현상(starvation)이 생길 수 있다.

Semaphore가 무엇인지, 어떻게 사용하는지 그리고 주의점을 살펴봤다.
다음은 Semaphore로 해결할 수 있는 고전적인 동기화 문제들이다.

## 고전적인 동기화 문제들

### Bounded-Buffer Problem

![bounded_buffer_problem](/images/operating_system/bounded_buffer_problem.png)

버퍼의 크기가 유한할 때 생산자와 소비자 문제다.
이 경우 동기화와 관련해서 어떤 문제가 생길 수 있을까?

#### 1) 공유 버퍼에 Producer 2명이 도착해서 동시에 데이터를 집어넣는다면 문제가 생길 수 있다.

이런 문제를 방지하려면 lock을 걸고 데이터를 넣고 풀어야 한다.(mutual exclusion)

#### 2) Producer가 한꺼번에 도착해서 n개의 버퍼를 다 채우면 또 문제가 생길 수 있다.

새로운 Producer가 와도 집어넣을 공간이 없다!

생산자 입장에서는 버퍼의 공간 하나 하나가 resource라고 볼 수 있다. 또, 소비자 입장에서는
버퍼에 담긴 내용이 resource라고 할 수 있다.

위의 사항을 고려하여 Bounded-Buffer Problem의 해결책을 설계해야 한다. 각 프로세스의 실행 절차는
다음과 같다.

#### Producer가 실행하는 절차

```
1) 빈 buffer가 있는지 확인
2) 빈 buffer 생기면 공유 데이터에 mutex lock 걸기
3) 빈 buffer에 데이터 입력 및 buffer 조작
4) mutex lock 풀기
5) Full buffer 하나 증가
```

#### Consumer가 실행하는 절차

```
1) full buffer가 있는지 확인
2) 공유 데이터에 mutex lock 걸기
3) full buffer에 데이터 입력 및 buffer 조작
4) mutex lock 풀기
5) empty buffer 하나 증가
```

```cpp
// Shared data : buffer 자체 및 buffer 조작 변수
// 동기화 변수들
// mutual exclusion -> 이진 세마포어가 필요하다.
// resource count -> 세마포어 정수 변수가 필요하다.

// 동기화 변수
int n;
semaphore empty = n; // the number of empty buffer
semaphore full = 0; // resource count
semaphore mutex = 1; // for lock
```

생산자 프로세스의 코드

```cpp
do {
    // produce an item in x
    wait(empty);
    wait(mutex);
    // ...
    // add x to buffer
    // ...
    signal(mutex);
    signal(full);
} while(1);
```

소비자 프로세스의 코드

```cpp
do {
    wait(full);
    wait(mutex);
    // ...
    // remove an item from buffer to y
    // ...
    signal(mutex);
    signal(empty);
    // ...
    // consume the itme in y
    // ...
} while(1);
```

이러한 방법으로 Semaphore를 통해 유한 버퍼 문제를 해결할 수 있다.
다음은 Readers-Writers 문제다.

### Readers-Writers 문제

유한 버퍼 문제와 유사하지만, 차이점은 데이터베이스를 읽고 쓰는 상황을 가정하고 있기 때문에
Write가 진행될 때는 동시에 다른 일이 병행 처리되면 안 되지만 Read가 진행될 때는 다른 작업의 병행 처리가
가능해야 한다는 것이다. read 중 다른 reader가 lock 걸어서 못 읽게 하면 안 된다.

```cpp
// Shared Data
int read_count = 0
DB; // DB 자체
// 동기화 변수
semaphore db = 1; // writer를 위한 mutex
semaphore mutex = 1; // read_count 변수를 위한 lock
```

Writer의 코드

```cpp
wait(db);
// ...
writing DB is performed
// ...
signal(db);
```

Reader의 코드

```cpp
wait(mutex);
read_count++:
// 최초의 reader라면
if (read_count == 1){
    wait(db);
}
signal(mutex);
// ...
reading DB is performed
// ...
wait(mutex);
read_count--;
// 마지막 reader라면
if (read_count == 0){
    signal(db);
}
signal(mutex);
```

Writer는 일반적인 lock, unlock을 하는 코드로 구성되어 있고 Readers의 코드의 경우
최초 독자와 마지막 독자가 Write를 막기 위해 lock을 걸고 다시 lock을 푸는 방식으로
진행된다. 이 Writers-Readers 문제에서의 문제점은 starvation이 생길 수 있다는 것이다.
Writer의 경우 Reader가 전부 빠져나가기 전에는 접근하지 못하는 문제가 있다. 신호등이
없으면 차가 계속 지나가서 길을 건널 틈이 없는 것과 유사하다.

### Dining-philosophers 문제

![dining_philosophers](/images/operating_system/dining_philosophers.png)

위 테이블에서 여러 명의 철학자가 앉아 있다고 할 때 철학자가 하는 일은 2가지다.
하나는 식사이고, 하나는 생각하는 일이다. 생각을 하고 있다가 배가 고파지면 왼쪽
젓가락과 오른쪽 젓가락을 집어서 식사를 하는 것이다.

복수 개의 프로세스가 복수 개의 자원에 접근할 때를 표현하기 떄문에 중요성을 가지는
문제다.

```cpp
// 동기화 변수
semaphore chopstick[5];
```

철학자 i의 코드

```cpp
do {
    wait(chopstick[i]); // 왼쪽 젓가락
    wait(chopstick[(i+1) % 5]); // 오른쪽 젓가
    // ...
    eat()
    // ...
    signal(chopstick[i]);
    signal(chopstick[(i+1) % 5]);
    // ...
    think();
    // ...
} while(1);
```

위 코드에는 문제점이 있다.
바로 'Deadlock'의 가능성이다. 모든 5명의 철학자가 동시에 왼쪽 젓가락을 잡으면?
식사를 마칠 때까지 젓가락을 놓지 않는데 하나씩 쥐고 안 놓아서 Deadlock이 발생한다.

#### Dining-Philosopher 문제에서의 Deadlock 해결방안

- 4명의 철학자만이 테이블에 동시에 앉을 수 있도록 한다.
- 젓가락을 두 개 모두 집을 수 있을 때에만 젓가락을 집을 수 있게 한다.
- 비대칭으로 젓가락을 잡도록 한다. 짝수 철학자는 왼쪽 젓가락부터, 홀수 철학자는
  오른쪽 젓가락부터 잡도록 순서를 정해주는 것이다.

다음은 두 개의 젓가락을 모두 집을 수 있을 때에만 젓가락을 잡는 해결책의 코드다.

```cpp
enum{thinking, hungry, eating} state[5];
semaphore self[5] = 0; // 권한이 없음을 의미한다. 원래는 대개 자원의 수가 된다.
semaphore mutex = 1; // lock의 역할을 한다.
```

Philosopher i의 코드

```cpp
do {
    pickup(i);
    eat();
    putdown(i);
    think();
} while(1);

void putdown(int i){
    wait(mutex); // state라는 공유 변수에 접근하기 위해
    state[i] = thinking;
    test((i+4) % 5);
    test((i+1) % 5);
    signal(mutex);
}

void pickup(int i){
    wait(mutex);
    state[i] = hungry;
    test(i);
    signal(mutex);
    wait(self[i]);
}

void test(int i){
    if (state[(i+4) % 5] != eating && state[i] == hungry && state[(i+1)%5] != eating){
        state[i] = eating;
        signal(self[i]); // 0에서 1로 권한을 줌
    }
}
```

### Semaphore의 문제점

- 정확성 입증이 어렵다.
- 자발적 협력이 필요하다.
- 한 번의 실수가 모든 시스템에 치명적인 영향을 미친다.
  ex) wait()을 하고 또다시 wait()를 한다면?

아래와 같은 상황에서는 mutual exclusion 즉 상호배제의 조건에 위배되게 된다.

```cpp
wait(mutex);
// critical section
signal(mutex);
```

그리고 다음과 같은 상황은 Deadlock의 가능성이 생긴다.

```cpp
wait(mutex);
// critical section
signal(mutex);
```

이러한 문제점들 때문에 제공되는 것이 바로 'Monitor'다.

## 모니터(Monitors)

### 모니터란?

Monitor : 동시 수행 중인 프로세스 사이에서 abstract data type의 안전한 공유를 보장하기 위한
high-level synchronization construct

Semaphore가 추상 자료형이라면 Monitor는 프로그래밍 언어 차원에서 제공하는 동기화 도구라고 생각하면 된다.

공유 데이터에 접근하기 위해서는 monitor 내부에 정의된 procedure을 통해서만 공유 데이터를 접근할 수 있다.
한 시점에 하나의 프로세스만 모니터 내부에 진입할 수 있도록 보장한다.

```
monitor monitor-name {
    shared variable declarations;
    procedure body P1 (...){
        //..
    }
    procedure body P2 (...){
        //..
    }
    initialization code {
        // ..
    }
}
```

![monitor](/images/operating_system/monitor.png)

위의 그림처럼 OOP에서 변수와 메소드를 묶어주는 것처럼 묶어준다. 이렇게 되면 lock이 필요없어진다.
semaphore와의 큰 차이점이고 편리하다.

하나씩 살펴보면 일단 Semaphore 처럼 자원의 개수를 세는 것이 필요하다. 그런 역할을 하는 것이 condition variable이다.
그리고 monitor에서는 두 가지 연산을 지원하는데 wait와 signal이다.

- x.wait() : 여분이 없어서 기다려야 할 때 호출한다. x라는 조건을 만족하지 못해 x라는 condition variable의 뒤에 가서
  줄을 서게 된다.
- x.signal() : 공유 데이터에 접근하고 나갈 때 기다리는 프로세스가 있으면 signal을 보내준다. wakeup을 해주는 역할이다.

### Monitor를 통한 고전적인 동기화 문제 해결

#### 1) Bounded-Buffer Problem

```cpp
monitor bounded_buffer
{
    int buffer[N];
    condition full;
    condition empty;
    /* condition variable은 값을 가지지 않고 자신의 큐에 프로세스를 줄 세워서
       sleep시키거나 큐에서 프로세스를 꺠우는 역할만을 한다. */

    void produce(int x)
    {
        if there is no empty buffer
            empty.wait();
        add x to an empty buffer
        full.signal();
    }

    void consume(int *x)
    {
        if there is no full buffer
            full.wait();
        remove an item from buffer and store it to *x
        empty.signal();
    }
}
```

참고) semaphore 코드와 monitor 코드는 쉽게 변환이 가능하다. semaphore 코드는 값의 변화가 있지만
monitor 코드는 그렇지 않다. 줄을 세울 뿐이다.

#### 2) Dining Philosophers example

```cpp
monitor dining-philosopher
{
    enum{thinking, hungry, eating} state[5];
    /* condition variable is to delay herself when she is hungry
    but is unable to obtain chopsticks */
    condition self[5];

    void pickup(int i)
    {
        state[i] = hungry;
        test(i);
        if (state[i] != eating){
            self[i].wait();
        }
    }

    void test(int i)
    {
        if ((state[(i+4) % 5] != eating) && (state[i] == hungry) && (state[(i+1) % 5] != eating)){
            state[i] = eating;
            self[i].signal(); /* Wake up Pi */
        }
    }

    void putdown(int i){
        state[i] = thinking;
        /* test left and right neighbors */
        test((i+4) % 5); /* if L is waiting */
        test((i+1) % 5);
    }

    void init(){
        for (int i = 0; i < 5; i++){
            state[i] = thinking;
        }
    }
}

Each Philosopher;
{
    pickup(i);
    eat();
    putdown(i);
    think();
} while(1);
```
