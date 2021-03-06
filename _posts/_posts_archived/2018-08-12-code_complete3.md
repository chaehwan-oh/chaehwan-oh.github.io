---
layout: post
subclass: post
title: "Code Complete 정리 (3)"
date: 2018-08-12 00:00:03
tags: [programming, must-read]
excerpt: "programming"
disqus: True
---

※ 스티브 맥코넬의 CODE COMPLETE를 읽고 정리한 글입니다.

# CODE COMPLETE 3 부 변수

## 10 장 변수 사용 시 고려할 사항

"개발 시 변수를 생성하는 데 많은 시간이 소요되고 좋은 습관을 갖게 되면 전체적인 작업 시간을 줄이고 나중에 문제를 겪지 않을 가능성도 높아진다."

### 변수 초기화

### 1. 변수를 선언할 때 초기화한다.

"부적절한 데이터 초기화는 컴퓨터 프로그래밍에서 오류를 발생시키는 가장 빈도가 높은 원인이다."

변수가 예상치 못한 초깃값을 포함하고 있을 때 문제가 발생하는데 그 원인은 다양하다. 예를 들어 변수에 애초에 값을 할당하지 않는다면 문제가 될 수 있다. 프로그램을 시작했을 때 해당 메모리 공간에 원래 있던 비트 값이 반영될 수 있다.

### 2. 변수가 처음 사용되는 곳 근처에서 초기화한다.

모든 변수의 선언과 초기화를 한 곳에서 몰아서 하는 경우가 많은데 변수를 처음으로 사용하는 곳과 떨어져 있다면 해당 변수가 어떤 값으로 초기화되어 있는지 확인하기가 불편하다.

### 3. 가능하다면 final 나 const 를 사용한다.

변수가 초기화된 후에 다른 값으로 할당되는 것을 막아야 하는 변수에 대해서는 final 이나 const 로 선언함으로써 안전장치를 둘 수 있다.

### 4. 다시 초기화해야 할 필요가 있는지 검사한다.

1.  카운터와 누산기를 특히 주의한다. 카운터나 누산기를 다음에 재사용하기 전에 초기화하는 것을 잊는 경우가 많다.
2.  루틴 안에서 루프가 변수를 여러 번 사용하거나 변수가 루틴이 여러 번 호출될 때 값을 유지해야 하거나 재설정되어야 해서 변수를 다시 초기화해야 하는지 검사할 필요가 있다.

### 변수의 범위

### 1. 변수의 "수명"을 가능한 한 짧게 유지한다.

1.  변수의 수명을 낮추면 변수가 부정확하게 또는 부주의하게 변경될 가능성을 줄일 수 있다.
2.  변수의 수명을 짧게 유지하면 코드를 쉽게 이해할 수 있다.
3.  변수의 수명이 짧으면 코드의 가독성이 높아진다.

### 2. 루프에서 사용되는 변수는 루프를 포함하고 있는 루틴의 시작이 아니라 루프 바로 앞에서 초기화한다.

### 3. 변수를 사용하기 바로 전까지 변수에 값을 할당하지 않는다.

### 변수의 목적

### 1. 각 변수를 한 가지 목적만을 위해서 사용하라.

때때로 하나의 변수를 다른 위치에서 다른 목적으로 사용하고 싶을 때가 있다. 그런 상황에서는 일반적으로 변수의 이름이 사용 목적을 제대로 표현하지 못하거나 "임시" 변수가 두 경우에 모두 사용된다.

### 2. 숨은 의미가 있는 변수를 피하라.

변수가 한 가지 이상의 목적으로 사용될 수 있는 또 다른 방법은 변수가 서로 다른 것을 의미하는 서로 다른 값을 갖는 것이다.
변수를 이중 목적으로 사용하는 것은 자신에게는 명백해 보일지라도 다른 사람에게는 그렇지 않을 것이다.

### 3. 선언된 모든 변수를 사용하는지 확인하라.

변수를 한 가지 이상의 목적으로 사용하는 것과 상반되는 것은 변수를 전혀 사용하지 않는 것이다.

## 11 장 변수 이름의 기능

### 1. 변수 이름을 지을 때 그 이름이 변수가 나타내는 것을 완전하고 정확하게 설명하는지를 가장 중요하게 고려해야 한다.

만약 현재의 이자율을 나타내는 변수는 r 이나 x 보다 interestRate 가 더 좋은 이름이다. 이러한 변수이름은 쉽게 읽을 수 있어서 해석할 필요가 전혀 없다. (하지만 몇몇 이름은 너무 길어서 실용적이지 않다.)

### 2. 이름은 가능한 한 구체적이어야 한다.

위 설명와 유사하지만 x, temp, i 와 같이 한 가지 이상의 목적으로 사용되기 쉬운 막연한 이름은 많은 정보를 제공하지도 않고 일반적으로 나쁜 이름이다.

### 3. 좋은 이름은 "어떻게"보다 "무엇"을 표현하는 경향이 있다.

문제보다 해결과정의 어떤 측면을 가르키고 있다면 이는 무엇보다는 "어떻게"에 대한 것이다. 문제 자체를 가리키는 이름을 사용하도록 한다.

### 4. 상태 변수라면 flag 보다 더 나은 이름을 생각해본다.

플래그라는 이름은 그것이 무엇을 하는지 아무런 단서도 제공하지 않기 때문에 변수 이름에 사용하지 말아야 한다.
코드는 직관적으로 이해할 수 있어야 한다.

### 5. '임시'변수를 조심하라.

일반적으로 임시 변수는 개발자가 문제를 완벽하게 이해하지 못하고 있다는 신호다. 게다가 변수가 공식적으로 '임시'상태이기 때문에 개발자는 임시 변수를 다른 변수보다 별생각 없이 다루게 되어 오류가 발생할 가능성이 커진다.

### 6. 참이나 거짓의 의미를 함축하는 boolean 변수의 이름을 사용한다.

done 과 success 와 같은 이름은 상태가 참 아니면 거짓, 또는 무언가가 수행되었거나 수행되지 않았거나 둘 중 하나이기 때문에 좋은 boolean 변수 이름이다.
(긍정적인 boolean 변수 이름을 사용한다. notFound, notdone 과 같은 부정적인 이름은 이 변수의 값이 부정이 되었을 때 읽기가 어려워진다.)

### 7. 오해의 소지가 있는 이름이나 축약어를 피한다.

### 8. 이름에 숫자를 넣는 것을 피한다.

이름에 있는 숫자가 정말로 중요하다면 개별적인 변수 대신 배열을 사용한다. 배열이 부적절하다면 숫자는 더욱 부적절하다.

## 12 장 기본 데이터형

### 1. "매직 넘버"를 피한다.

매직 넘버는 100 이나 47524 와 같이 아무런 설명도 없이 프로그램 중간에 사용되는 숫자를 말한다. 이름 상수를 대신 사용하도록 한다.

매직 넘버를 사용하지 않으면

첫째, 더욱 안정적으로 변경할 수 있다.
둘째, 더욱 쉽게 변경할 수 있다.
셋째, 코드의 가독성이 높아진다.

### 2. 정수 나눗셈을 검사한다.

정수를 사용할 때 7/10 은 0.7 이 아니다. 일반적으로 0 이거나 음수 무한대, 가장 가까운 정수 등이 될 수 있다. 계산 결과는 프로그래밍 언어마다 다르다. 이러한 문제점을 없애는 가장 쉬운 방법은 나눗셈이 가장 나중에 수행되도록 표현식의 순서를 (10 \*7)/10 과 같이 바꾸는 것이다.

### 3. 정수 오버플로우를 검사한다.

부호가 없는 정수의 최댓값은 32 비트 기계에서는 일반적으로 2 의 32 승 마이너스 1 이며 때에 따라서 2 의 16 승 마이너스 1 즉 65,535 이다. 두 정수를 곱할 때 결과가 최대값보다 크면 문제가 발생한다. 예를 들어 250 \* 300 을 계산하면 75,000 이다. 하지만 정수 최댓값이 65,535 라면 정수 오버플로우(75,000 - 65,536 = 9464) 이기 때문에 9464 라는 결과값을 얻게 될 것이다.
(중간 결과에서의 오버플로우도 검사할 필요가 있다.)

### 4. 부동 소수점 수를 사용할 때 가장 중요하게 고려해야 할 사항은 많은 분수 값이 디지털 컴퓨터의 1 과 0 을 사용하여 정확하게 표현할 수 없다는 점이다.

첫째, 서로 다른 크기가 매우 다른 수를 더하거나 빼지 않는다.

32 비트 부동 소수점 변수를 사용한다면 1,000,000.00 + 0.1 의 결과값이 1,000,000.00 일 것이다. 32 비트는 1,000,000 과 0.1 사이의 범위를 포함할 수 있을 만큼 충분한 자릿수를 제공하지 않기 때문이다. 이렇게 크게 차이가 나는 숫자를 더해야 한다면 우선 숫자를 구분하고 가장 작은 값부터 더한다. 마찬가지로 무한한 수를 더할 때도 가장 작은 항부터 시작한다. 반드시 작은 항부터 더한다. 이렇게 해도 잘리는 문제점을 완전히 없앨 수는 없지만, 최소화할 수는 있다.

둘째, 동치 비교를 피한다.

같아야 하는 부동 소수점 수가 항상 같지는 않다. 한 가지 효과적인 접근 방법은 적당한 정확도의 범위를 결정하여 값이 충분히 근사한지 결정하기 위한 불린 함수를 사용하는 것이다.

셋째, 라운딩 오류를 예측한다.

### 5. 매직 문자와 문자열을 사용하지 않는다.

이름 상수를 대신 사용한다. 리터럴 문자열 사용을 피해야 하는 이유에는 여러 가지가 있다.

첫째, 이름 상수는 의도를 이해하기 쉽게 만들 수 있다.
둘째, 변경할 필요가 있을 수 있다.

### 6. 복잡한 테스트를 단순화하기 위해 불린 변수를 사용한다.

불린 표현식을 테스트하는 대신 표현식의 의미가 더욱 명확해지게 변수에 할당한 후 테스트를 수행해도 된다.

### 7. 배열의 모든 인덱스가 배열의 경계 내에 있는지 확인하라

배열을 사용할 때 발생하는 모든 문제는 배열의 요소에 임의로 접근할 수 있기 때문에 발생한다.

### 8. 배열 대신 컨테이너 사용을 고려하거나 배열을 순차적인 구조체로 생각하라.

컴퓨터 분야에서 뛰어난 몇몇 사람들은 배열에 절대로 임의로 접근해서는 안 되며 오로지 순차적으로만 접근해야 한다고 했다. 그들은 배열에 임의로 접근하는 것이 프로그램에 goto 를 임의로 사용하는 것과 유사하다고 주장한다. 임의 접근은 대개 규칙이 없고 오류를 유발하기 쉬우며 정확성을 입증하기가 어렵기 때문이다. 그들은 배열을 사용하는 대신 요소에 순차적으로 접근하는 집합이나 스택, 큐를 사용할 것을 제안한다.

### 9. 배열의 마지막 위치를 확인하라

### 10. 인덱스에 혼선이 있지 않도록 주의하라.

## 13 장 특이한 데이터형

### 1. 데이터 관계를 이해하기 쉽게 하기 위해서 구조체를 사용하라.

구조체를 사용하면 연관된 데이터를 그룹으로 묶기 때문에 구조체를 변경할 때 프로그램 전체적으로 변경할 것이 더 적어진다.

### 2. 포인터 오류는 일반적으로 포인터가 가리키면 안 되는 곳을 가리켜 발생한다.

### 3. 포인터 연산을 루틴이나 클래스에 고립시켜라.

포인터에 접근하는 코드를 최소화함으로써 프로그램 전체에 영향을 미치고 찾는 데 아주 오랜 시간이 걸리는 부주의한 실수를 저지를 확률을 최소화할 수 있다.

### 4. 포인터 선언과 동시에 정의하라.

가능하면 변수가 선언된 곳과 가까운 곳에서 변수의 초기 값을 할당하는 것이 일반적으로 좋은 프로그래밍 습관이며 포인터를 다룰 때는 더욱 유용하다.

### 5. 포인터를 할당한 곳과 같은 영역에서 삭제하라

### 6. 포인터를 사용하기 전에 검사하라.

### 7. 그림을 그려라.

포인터를 코드로 설명하면 혼란스러울 수 있다. 일반적으로 그림을 그리는 것이 도움이 된다.

### 8. 링크드 리스트에 있는 포인터를 올바른 순서로 삭제하라.

동적으로 할당되는 링크드 리스트를 다룰 때 발생하는 일반적인 문제점은 리스트에 있는 첫 번째 포인터부터 해제하여 다음 포인터를 얻지 못하는 것이다. 이 문제를 피하기 위해서는 현재 요소를 해제하기 전에 다음 요소를 가리키는 포인터를 갖고 있어야 한다.

### 9. 쓰레기를 확실하게 삭제하라.

포인터 오류는 포인터가 가리키는 메모리가 어디에서 유효하지 않게 되는지 정해져 있지 않기 때문에 디버깅하기가 어렵다.

### 10. 포인터를 삭제하거나 해제한 다음 널로 설정하라.

전형적인 포인터 오류 형태는 이미 delete 되거나 free 된 포인터를 사용하는 허상 포인터(dangling pointer)다.
