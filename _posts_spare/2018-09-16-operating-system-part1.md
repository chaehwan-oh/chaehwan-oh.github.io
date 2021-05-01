---
layout: post
subclass: post tag-speeches
title: "운영체제 (1) 운영체제의 정의와 동작 방식"
date: 2018-09-16 00:00:01
tags: ["computer-science"]
# tags: [dudaji, computer_science, operating_system]
excerpt: "운영체제 서론에 대한 정리입니다."
disqus: True
---

## 운영체제의 정의

![os_definition](/images/operating_system/os_definition.png)

운영체제는 '컴퓨터의 하드웨어 바로 윗단에 설치되어 하드웨어를 관리하는 프로그램'이다.
운영체제의 기능과 역할은 2가지 관점에서 생각해볼 수 있다.

1. 시스템 관점

운영체제는 하드웨어와 가장 밀접한 소프트웨어다. 운영체제는 내부 여러 가지 자원들을
관리하는 **자원 관리자**(resource manager)로서의 역할을 한다.

운영체제는 다양한 사용자를 위해 **다양한 응용 프로그램 간의 하드웨어 사용을 제어하고 조정한다.**

2. 사용자 관점

운영체제는 사용자들에게 컴퓨터 시스템을 편리하게 사용할 수 있는 환경을 제공한다는
점에서 '**인터페이스**'를 제공하는 역할을 한다.

이러한 운영체제의 동작을 제대로 이해하기 위해서는 컴퓨터 시스템 구조에 대한
전반적인 지식이 필요하다. 그래서 '컴퓨터 시스템의 구성과 구조'에 대해 이해하는 것이
중요하다.

## 컴퓨터 시스템의 구성

![computer_system](/images/operating_system/computer_system.png)

기본적으로 컴퓨터 시스템이 구동되려면 실행할 초기 프로그램이 있어야 한다.
컴퓨터 시스템 내부의 ROM에 초기 프로그램이 저장되어 있어서 이 프로그램이 시스템의 모든 면을
초기화한다.

또한, 운영체제를 동작시키기 위해 초기 프로그램은 운영체제의 커널을 메모리에 적재하는 작업을 한다.
그런 다음은 운영체제는 init이라는 첫 프로세스를 실행하고 어떤 이벤트를 발생하기를 기다린다.

컴퓨터 시스템에서 가장 먼저 CPU와 메모리부터 살펴보면 CPU는 매 클럭마다 메인 메모리에 적재된 명령어를
읽어와서 계속적으로 실행할 뿐이다. 순차적인 명령을 실행할 뿐이다. 하지만 키보드 입력과 모니터 출력 등 다양한
이벤트에 대해서도 대응해야 한다. 이 때 필요한 것이 '**인터럽트**'다.

조금 더 자세히 살펴보면 각 장치에는 디바이스 컨트롤러라는 디바이스를 담당하는 작은 CPU가 있다. 또, CPU가
Memory라는 작업공간이 필요하듯 디바이스 컨트롤러도 작업 공간이 필요한데 그것이 로컬 버퍼다. I/O 장치에 접근해서
데이터를 읽어오고 출력하는 작업을 해야 하는데 이 작업은 상당히 오래 걸린다. 그래서 CPU는 직접 I/O 장치에 접근하지
않는다. I/O controller라는 I/O 장치마다 있는 작은 CPU가 I/O 장치에 접근해 데이터를 읽어올 수 있도록 작업을
지시한다. 이 작업에 소요되는 시간 역시 오래 걸리기 때문에 CPU는 지시한 다음 다른 명령을 읽어와서 수행하는 작업을 한다.
(읽어온 값에 대해 연산을 진행해야 하는 경우가 대부분이다. 그래서 CPU는 다른 프로그램을 실행한다.)
그리고 I/O controller가 로컬 버퍼로 데이터를 읽어오는 작업을 완수하면 인터럽트를 통해 보고하는 방식을 작업을 수행한다.

이런 방식으로 작업을 수행하기 위해 CPU에는 interrupt line이 별도로 존재하고 이 인터럽트 라인이 set되면 CPU는 인터럽트를
인식할 수 있다.

문제는 장치마다 인터럽트를 걸다 보면 CPU가 바쁘게 다른 명령을 수행해야 하는데 어떤 작업을 수행하려다 인터럽트가 와서 다른
작업을 수행하는 방식으로 명령을 수행하게 된다. 이렇게 되면 효율성이 떨어진다. 그래서 두는 것이 '**DMA(Direct Memory Access) 컨트롤러**'다.

**메모리는 원칙적으로 CPU만 접근이 가능하다.** 그런데 CPU에 인터럽트가 너무 걸리니 DMA 라는 메모리에 직접 접근할 수 있는 장치를
하나 더 둠으로써 CPU가 조금 더 효율적으로 움직일 수 있도록 하는 것이다. DMA가 메모리에 접근해서 작업을 수행하고 마지막에
한 번 DMA 컨트롤러가 인터럽트로 CPU에 완료를 알리는 방식으로 진행되는 것이다.

**원래는 CPU만이 메모리에 접근할 수 있는데 DMA도 같이 접근할 수 있게 되면서 생기는 문제점은 CPU와 DMA가 동시에 같은 데이터에 접근해서 데이터의 일관성이 깨지는 것이다. 이를 방지하기 위해 메모리에도 메모리 컨트롤러를 별도로 둠으로써 우선순위를 조정해
이런 문제를 해결하고 있다.**

마지막으로 살펴볼 장치는 **타이머**다. 예를 들어 하나의 프로세스가 무한 루프에 빠진다면 계속 CPU의 제어권을 가지고 내놓지 않는다.
타이머는 **특정 프로세스가 CPU를 독점하는 것을 방지하기 위해 CPU 할당 시간을 기준으로 CPU에 인터럽트를 걸어서 다른 프로세스로
제어권이 넘어갈 수 있도록 하는 역할**을 한다.

이러한 컴퓨터 시스템에서 프로그램이 실행될 수 있는 환경을 제공하는 것이 운영체제의 역할인데 그 구조를 살펴보도록 하자.

## 운영체제의 구조

운영체제의 가장 중요한 측면은 다중 프로그래밍을 할 수 있는 능력이다.

다중 프로그래밍은 메모리에 여러 개의 프로그램을 상주시켜 하나의 CPU로 동시에 여러 프로그램을 실행하는 것처럼
처리하는 기법이다. 다중 프로그래밍은 CPU가 항상 하나의 작업을 실행할 수 있게 작업을 조정함으로써 CPU 이용률을
높인다.

예를 들어 비다중 프로그래밍이라면 하나의 작업을 진행하다가 입출력 작업을 실행해야 한다면 입출력 작업이 종료될 때까지
CPU가 기다려야 한다. 반면, 다중 프로그래밍의 경우 CPU를 다른 프로세스에 전환해 CPU 이용률을 높인다.

시분할 시스템은 이런 다중 프로그래밍 시스템의 논리적 확장이다. 시분할 시스템은 CPU가 다수의 작업을 매우 짧은 시간동안
교대로 실행함으로써 다수의 사용자가 하나의 컴퓨터를 공유하지만 사용자가 전체 컴퓨터를 전용하는 것처럼 느낄 수 있도록 하는
기법이다.

이러한 시분할 시스템과 다중 프로그래밍 운영체제에서는 메모리에 여러 작업이 동시에 유지되어야 한다.
그러므로 다음과 같은 작업을 수행해야 한다.

- 유한한 메모리 공간에 몇 개의 프로그램을 선택해 적재하는 '작업 스케줄링'
- 메모리에 여러 개의 프로그램이 있을 때의 '메모리 관리'
- 여러 개의 프로세스가 실행 준비 상태일 때 하나의 프로세스를 선택하는 'CPU 스케줄링'
  위 작업들을 포함한 다양한 작업들을 수행함으로써 시분할 시스템 환경을 유지한다.

## 운영체제의 동작 방식

### 인터럽트 구동식(interrupt driven)

현대 운영체제는 인터럽트 구동식(interrupt driven)이다. 별도의 이벤트가 없다면 CPU는 프로그램 카운터의 명령어를 하나씩
순차적으로 실행할 뿐이다. 어떠한 이벤트가 발생하면 그 때 인터럽트(interrupt)나 트랩(trap)를 통해 신호를 발생시킨다.

여기서 **트랩**은 **사용자 프로세스에서 I/O 작업을 위해 CPU의 제어권을 커널에 넘기는 등의 소프트웨어에 의해 생성된 인터럽트**를
말한다. 예를 들어 사용자 프로세스가 I/O 장치에 접근할 필요가 있을 때 사용자 프로세스는 I/O 장치에 접근할 수 없다. **다른 사용자의
디스크 저장 파일에 접근하여 악의적으로 조작하는 것이 가능해지기 때문이다.** 이 때 운영체제에 부탁해서 처리하는 과정이 필요하다.

일반 함수 호출과는 다르다. 사용자 프로세스가 OS 코드의 메모리 공간으로 jump하거나 다른 영역의 코드를 실행하는 것은 불가능하기 때문에 소프트웨어적으로 CPU에 인터럽트를 거는 셈인 것이다. device controller 등의 하드웨어가 interrupt를 거는 것도 가능하지만 소프트웨어적으로 인터럽트를 거는 것도 가능하다. 일반적인 인터럽트는 하드웨어 인터럽트를 말하고 넓은 의미의 인터럽트는 소프트웨어적인 인터럽트를 포함한다.

0으로 나누는 등의 오류나 운영체제의 코드를 접근하려고 하거나 하는 등의 예외를 발생시키면 이를 막아버리기 위해 인터럽트 라인이 자동으로 세팅되고 제어권은 운영체제로 넘어간다. 이러한 과정 역시도 트랩이라고 할 수 있고 소프트웨어 인터럽트에 속한다.

다시 돌아와 정리하면, 하드웨어는 시스템 버스를 통해 CPU에 신호를 보내 인터럽트를 발생시킬 수 있고, 소프트웨어는 시스템 콜(system call)이라고 불리는 소프트웨어 인터럽트를 통해 인터럽트를 발생시킬 수 있다.

인터럽트는 시스템에서 굉장히 중요한 부분이기 때문에 각 컴퓨터 설계에서 인터럽트 메커니즘을 가진다.

인터럽트 발생 시 인터럽트 처리 루틴이 호출되고 그 처리 루틴은 다시 인터럽트 고유의 핸들러(handler)를 호출하는 것이 기본적이다.
그런데 인터럽트는 매우 빠르게 처리되어야 하고, 사용 가능한 인터럽트의 수가 미리 정의되어 있기 때문에 인터럽트 루틴에 대한 포인터의 테이블을 이용하는 방식으로 처리하는 것도 가능하다. 여기서 인터럽트 루틴에 대한 주소를 담고 있는 테이블을 '인터럽트 벡터'라고 한다.

### 이중 모드

운영체제의 동작에서 보안, 보호를 위해 사용자 모드와 커널 모드로 별도로 구분되어 실행되는 것이 필요하다. 앞서 살펴봤듯이 I/O
요청은 사용자 프로세스가 직접 수행할 수 없다. 다른 사용자의 디스크 저장파일을 악의적으로 조작하는 것이 가능해지기 때문이다.
이러한 이유로 시스템 내부에 접근하는 명령 등은 사용자 모드가 아닌 커널 모드를 통해 실행된다. 이를 통해 시스템 내부와 다른 사용자의 데이터를 보호한다.

이러한 모드 설정은 모드 비트라는 비트를 통해 하드웨어적으로 지원된다. 모드 비트가 1로 설정되어 있으면 사용자 모드이므로 제한된 명령만 실행이 가능하고 모드 비트가 0으로 설정되어 있으면 특권 명령을 실행할 수 있게 된다.