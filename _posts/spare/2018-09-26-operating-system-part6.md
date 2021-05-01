---
layout: post
subclass: post
title: "운영체제 (6) DeadLock"
date: 2018-09-26 00:00:06
tags: [dudaji, computer-science, operating_system]
excerpt: "운영체제 교착상태(Deadlock)에 대한 정리입니다."
disqus: True
---

## Deadlock이란?

Deadlock : 사거리가 마비된 것처럼 각 프로세가 자원을 가지고 다른 자원을
요구하면서 자원을 내려놓지 않아서 진행되지 않고 마비된 상태
(여기서 resource(자원)은 하드웨어 자원일 수도 있고, 소프트웨어 자원일 수도
있다.)

```
semaphore A;
semaphore B;

P0          P1
P(A);       P(B);
P(B);       P(A);
```

## Deadlock의 특징 : Deadlock은 왜 발생하는가?

Deadlock의 4 가지 조건

- Mutual Exclusion(상호 배제) : 한 시점에 하나의 프로세스만 접근 가능
- No Preemption(비선점) : 빼앗기지 않고 스스로 자원을 반납하기 때문
- Hold and Wait(보유 대기)
  : 적어도 하나 이상의 자원을 보유하고 다른 자원을 요구하기 때문에 발생
- Circular Wait(순환 대기)
  : 각 프로세스가 요구하는 자원과 보유하고 있는 자원이 엇갈려 사이클의
  형태를 띄기 때문

## Deadlock이 발생했는지 파악하는 방법은?

자원 할당 그래프(Resource-Allocation Graph)

- 그래프에 사이클이 없으면 Deadlock이 아니다.
- 그래프에 사이클이 있다고 무조건 Deadlock인 것은 아니다.
  - 자원마다 하나의 instance를 가지는 경우 -> Deadlock
  - 자원마다 여러 개의 instance를 가지는 경우 -> Deadlock의 가능성 존재

### 위 그림은 Deadlock일까?

![resource_allocation_graph](/images/operating_system/resource_allocation_graph.png)

deadlock이 아니다.
이유는?

- R1과 R2는 자원을 2개씩 가지고 있고 P2와 P4에게 자원이 할당되어 있지만
  이 둘은 Deadlock에 연루되어 있지 않다.

그러므로,

- P2와 P4가 자원을 사용하고 반납하면 가용 자원이 생기므로 Deadlock이 아니다.

## 그렇다면 Deadlock을 어떻게 처리할 수 있을까?

크게 4가지 방법이 있다.

- Deadlock Prevention
- Deadlock Avoidance
- Deadlock Detection and recovery
- Deadlock Ignorance

하나씩 살펴보도록 하자.

### Deadlock Prevention

Deadlock Prevention : Deadlock이 발생할 수 있는 4가지 조건 중 하나를 원천적으로 차단해
Deadlock을 미연에 방지하는 방법

#### Mutual Exclusion

상호 배제 조건이다. 상호 배제 조건은 막을 수 있는 조건이 아니다. mutex lock처럼 동시에
공유해서는 안 되는 자원의 경우 반드시 상호 배제이 성립되어야 하기 때문이다.

#### Hold and Wait

Deadlock이 생기는 이유는 이미 보유한 자원을 놓지 않으면서 다른 자원을 기다리기 때문이다.

그러므로, 자원을 기다려야 하는 상황에서는 자원을 보유하지 않으면 된다.

- 방법1. 프로세스가 시작될 때 프로세스가 필요로 하는 모든 자원을 할당받는 방법

모든 자원을 할당 받고 종료할 때 완전히 반납한다. 매 시점마다 필요로 하는 자원이 다르기 때문에
모든 자원을 보유하고 있는 것은 굉장히 비효율적이다.

- 방법2. 자원이 필요할 때마다 보유 자원을 모두 반납하고 다시 요청하는 방법

이 방법의 경우 자원을 필요로 하는 프로세스는 필요로 하는 자원을 받아서 진행할 수 있다.

#### No Preemption

Deadlock이 생기는 이유 중 하나가 자원을 빼앗아서 진행할 수 없기 때문이다. 그러므로 이를
빼앗아서 진행할 수 있도록 바꾸는 것이다.

하지만 이 방법은 상태를 쉽게 저장하고 복원할 수 있는 자원에 대해서만 가능한 방법이다.
CPU register와 Memory의 경우 이러한 방법이 가능하다.

#### Circular wait

circular wait는 필요한 자원이 꼬리에 꼬리를 물고 있어서 사이클이 형성된 상태다.

이를 방지하는 방법 중 하나는 자원에 번호를 매겨 순서를 정해주는 것이다. 예를 들어
높은 번호의 자원을 획득하려면 낮은 번호의 자원을 먼저 획득해야만 획득할 수 있도록
하는 것이다.

하지만 이러한 방법은 Deadlock은 방지할 수 있지만, CPU 이용률이 저하되고, 처리율이
감소하고, Starvation 문제가 발생할 수 있다.

### Deadlock Avoidance

Deadlock Avoidance : 자원을 요청할 때 부가 정보를 이용해 자원이 있지만 deadlock의
가능성이 전혀 없는 경우에만 자원을 할당하는 방법

다양한 부가정보를 활용할 수 있지만 가장 단순하면서 유용한 방법은 '프로세스가 필요로 하는
자원의 최대량'이다.

이 부가정보를 미리 알고 있다고 가정하고 Deadlock을 피해가는 방법이다. 이 부가정보를 활용해
Deadlock의 가능성이 있으면 자원의 여분이 있어도 주지 않는 것이다.

Deadlock Avoidance는 크게 2가지 상황으로 나누어 생각해볼 수 있다.

#### 자원의 instance가 하나 밖에 없는 경우

자원 할당 그래프를 활용한다.

![deadlock_avoidance](/images/operating_system/deadlock_avoidance.png)

- 프로세스를 가리키는 화살표는 자원이 할당된 경우를 말한다.
- 자원을 가리키는 화살표는 자원을 요청했지만 할당받지 못한 상황을 말한다.
- 점선은 프로세스가 생성되고 나서 종료될 때까지 한 번은 이 자원을 사용할 일이 있음을
  의미한다.
- 점선에서 실선이 되면 실제로 요청한 경우를 의미한다.

여기서 P2가 R2 자원을 할당받았다고 가정해보자. 상황은 여러 가지가 될 수 있다.

- P1이 자원을 반납해서 Deadlock이 발생하지 않는 경우
- 하필 이 때 P1이 R2를 요청해서 Deadlock이 발생하는 경우

점선과 실선들로 사이클이 발생한다면 unsafe 즉 Deadlock으로부터 안전하지 않은 상황이다.
Deadlock avoidance는 가용 자원이 있다고 해서 자원을 다 내어주는 것이 아니라 Deadlock의
위험성이 있다면 애초부터 자원을 내어주지 않는 방식으로 Deadlock을 방지한다.

#### 자원이 여러 개일 경우

이 때는 Banker's algorithm을 통해 해결한다.

```
5개의 Process P0 P1 P2 P3 P4
3종류의 자원 A(10) B(5) C(7) instances

    Allocation      Max      Available    Need(Max - Allocation)
    A B C           A B C    A B C              A B C
P0  0 1 0           7 5 3    3 3 2              7 4 3
P1  2 0 0           3 2 2                       1 2 2
P2  3 0 2           9 0 2                       6 0 0
P3  2 1 1           2 2 2                       0 1 1
P4  0 0 2           4 3 3                       4 3 1
```

- Max는 자원의 최대 사용량을 의미한다.
- Available은 현재 가용 자원의 양을 의미한다.
- Need는 추가 요청 가능한 양을 의미한다.

만약 P1 프로세스가 (1, 0, 2)를 요청했다면?

Available 즉 가용자원의 양으로 수용 가능하다. Banker's Algorithm은
그냥 자원을 할당하는 것이 아니라 Need 즉 추가 요청 가능한 양과 비교해서
충족 가능할 때 자원을 할당해준다.

만약 P0 프로세스가 B자원 2개 (0, 2, 0)을 요청했다면?

이 경우 P0 프로세스의 Need가 Available 즉 가용 자원의 양보다 크기 때문에
혹시 최대로 요청하면 가용 자원으로 수용할 수 없으므로 요청을 받아들이지 않는다.
요청이 가용자원보다 작더라도 최악의 경우 Need 만큼 요청하면 수용이 안 되기 때문에
자원을 할당해주지 않는다.

##### 가용 자원만큼 먼저 할당해주면 Deadlock이 발생하는가? no!

만약 P0 프로세스가 Need인 (7, 4, 3)을 요청했을 때 Available인 (3, 3, 2)만큼은
할당해줄 수 있다. 그렇게 해도 Deadlock은 아니기 때문이다. 다른 프로세스가
자원을 내려놓을 수 있기 때문이다. 하지만 Banker's algorithm은 보수적으로
Deadlock을 방지하기 때문에 애초에 그런 상황을 만들지 않는다.

##### safe sequence가 존재하면 시스템은 safe state에 있다.

safe sequence는 "가용자원(Available) + 모든 Pj(j < i)의 보유자원"에 의해
자원 요청이 모두 충족되는 프로세스의 sequence다. 이전에 프로세스에 할당하고
반납하므로 가용자원에 더해지는 것이다. 이 safe sequence가 만들어지면 시스템이
safe state에 있다고 말할 수 있다.

정리하면, Deadlock은 자주 발생하는 상황이 아니지만 혹시나의 상황을 방지하기 위해
자원을 주지 않는 것이 Deadlock avoidance다. Banker's Algorithm도 간단하다.
Need를 만족시킬 수 있으면 자원을 할당하고 아니면 자원을 할당해주지 않는다. 이러한
방식으로 항상 safe한 상태를 유지할 뿐이다.

Deadlock avoidance는 비효율적이라고 볼 수 있다.

### Deadlock Detection and Recovery

Deadlock Detection and Recovery : Deadlock이 미연에 방지하지 않고 발생했을 때
감지하여 조치하는 방법

Deadlock이 발생해도 놓아두는 방법이다. Deadlock 자체가 빈번히 발생하는 일이 아니므로
그 오버헤드를 감당하기보다 시스템이 이상하면 감지한 후 회복하는 것이다.

Deadlock Detection에는 두 가지 방법이 있다.
Deadlock Detection에도 자원의 instance가 하나인 경우와 여러 개인 경우로 나누어진다.

#### 자원의 instance가 하나인 경우

자원의 instance가 하나인 경우에는 자원 할당 그래프를 활용한다. 자원 할당 그래프에서
자원을 빼고 간략하게 그리는 것이 가능한데 이를 'wait-for graph'라고 한다.

![wait_for_graph](/images/operating_system/wait_for_graph.png)

이 wait-for 그래프에서 사이클을 확인한다.

참고) wait-for graph에서 cycle을 찾는 오버헤드

wait-for graph에서 cycle을 찾는 오버헤드는 O(n^2)이다.
화살표는 정점 n개일 때 n \* (n-1) 개가 있을 수 있다.

너비 우선 탐색과 깊이 우선 탐색 방법으로 cycle을 찾을 수 있다.

#### 자원의 instance가 여러 개인 경우

instance가 여러 개인 경우 Banker's algorithm처럼 table을 그려서
해결한다. Banker's algorithm 처럼 Need는 필요 없고, Banker's algorithm과
달리 보수적으로 보지 않고 낙관적으로 본다. 자원을 가지고 있어도 반납할 것이라고 본다.

```
    Allocation      Requeset      Available
    A B C           A B C         A B C
P0  0 1 0           0 0 0         0 0 0
P1  2 0 0           2 0 2
P2  3 0 3           0 0 0
P3  2 1 1           1 0 0
P4  0 0 2           0 0 2
```

- Allocation은 이미 할당된 것을 말한다.
- Request는 현재 요청을 말한다.

순서대로 살펴보면 P0은 B 자원 하나를 가지고 있고 추가 요청이 있을 수 있지만
반납할 것이라고 본다. 현재 요청이 없는 Process에 대해 자원을 반납할 것이라고 가정한다.
그리고 수용 가능한 요청을 수용한 다음 반납한다고 본다. 이런 식으로 요청을 전부 받아들이는
sequence가 존재하면 Deadlock이 없는 것으로 본다.

만약 P2가 자원을 하나 더 요청한다면?

P0만 요청이 없고 P0만 자원을 반납한다. 그래서 Deadlock에 빠진다.

정리하면

- 1. 가용 자원이 몇 개 있는지 확인한다.
- 2. 가용 자원으로 처리 가능한 것이 있는지 확인한다.
- 3. 지금 요청하지 않은 프로세스는 가용 자원으로 합친다.
- 4. 합쳐진 가용자원으로 처리 가능한 것이 있는지 확인한다.
- 5. 처리하고 자원을 반납하고 가용자원에 합친다.
- 6. safe sequence가 나오면 Deadlock이 아니고, 안 나오면 Deadlock이다.

#### Deadlock을 감지했을 때 회복하는 방법

회복하는 방법은 크게 2가지 방법이다.

##### Process termination

- 1. Deadlock에 연루된 프로세스를 모두 죽이는 방법
- 2. Deadlock에 연루된 프로세스를 하나씩 죽이면서 확인하는 방법

##### Resource Preemption

Deadlock에 연루된 프로세스로부터 자원을 빼앗는 방법

- 1. victim을 정해서 뺏기
- 2. safe state로 rollback
- 3. process restart

Resource Preemption의 문제점은 자원을 빼앗았는데 다른 프로세스가 획득하기 전에
다시 요청해서 가져가는 상황이 발생할 수 있다는 것이다. 그러므로 자원을 선점하는
방식을 조금씩 바꾸어주어야 한다.

이를 통해 특정 프로세스만 계속 자원을 빼앗기는 Starvation 문제를 방지해야 한다.

### Deadlock Ignorance

현대 OS들이 택하고 있는 방법이다. Deadlock이 발생해도 아무것도 하지 않는 방법이다.
Deadlock이 자주 발생하는 상황이 아니니 이를 해결하기 위해 오버헤드를 만들지 않겠다는
방식이다. 시스템이 느려지는 등의 상황이 발생하면 사용자가 해결해야 한다.
