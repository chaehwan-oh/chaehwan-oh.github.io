---
layout: post
subclass: post
title: "Code Complete 정리 (2)"
date: 2018-08-12 00:00:02
tags: [programming, must-read]
excerpt: "programming"
disqus: True
---

# CODE COMPLETE 2 부 고품질 코드 작성

## 5 장 구현 설계

### 1. 설계의 어려움

학교에서 개발하는 프로그램과 전문가로서 개발하는 프로그램의 주요 차이점 중 하나는 학교에서 작성하는 프로그램은 해결하고자 하는 설계상의 문제점이 명확하다는 것이다. 학교의 프로그래밍 과제는 처음부터 끝까지 똑바로 진행할 수 있게 만든다. 프로그램 과제를 내주고는 설계를 끝내자마자 과제를 바꾸고 프로그램을 완성해 제출하려는 순간 다시 과제를 바꿔 버리는 교수가 있다면 화가 날 것이다.
하지만 이런 일이 실무 프로그래밍 세계에서는 매일 일어나는 현실이다.

### 2. 핵심 설계 개념 - 복잡성 관리

소프트웨어의 주요 기술적 의무는 복잡성을 관리하는 것이다.

프로젝트에서 한 영역의 코드를 변경했을 때 다른 부분에서 어떤 영향을 미치는지 완벽하게 이해하는 사람이 없을 정도라면 더 이상 개발을 할 수 없는 지경에 이른 것이다.

소프트웨어 개발자가 전체 프로그램을 억지로 한 번에 머릿속에 밀어 넣으려고 해서는 안 된다.
한 번에 한 부분을 제대로 집중할 수 있게 프로그램을 구성하도록 노력해야 한다. 목표는 한 번에 생각해야 하는 프로그램의 크기를 최소화하는 것이다. 마치 공이나 접시로 저글링을 하는 것과 같다고생각하면 된다. 한 번에 다뤄야 하는 공의 개수가 많아지면 많아질수록 공을 떨어뜨릴 확률도 높아지고,
결국 이는 설계 또는 코드에서의 오류를 일으킨다.

모든 소프트웨어 설계 기법의 목표는 복잡한 문제를 간단한 문제로 나누는 것이다. 서브 시스템이 독립적일수록 한 번에 복잡한 부분의 일부에 집중하기가 더 쉽다. 객체를 신중하게 정의함으로써 한 번에 하나의기능에만 집중할 수 있도록 작업을 나눈다. 패키지도 더 높은 수준의 통합이라는 같은 장점을 제공한다.

결론적으로 인간의 선천적인 한계를 보완할 줄 아는 개발자는 자신뿐만 아니라 다른 사람도 이해하기 쉽고 오류가 적은 코드를 작성한다.

### 3. 바람직한 설계의 특징

3-1) 복잡성 최소화

'재치 있는' 설계는 피한다. 재치 있는 설계는 일반적으로 이해하기 어렵다. 대신 '간단'하고 '이해하기 쉬운' 설계를 만들어라.

3-2) 유지보수의 편리함

유지보수 개발자가 작성한 코드에 대해서 물어볼 만한 질문을 계속 떠올려 본다. 유지보수 개발자를 청중이라고 생각하고 시스템을 쉽게 이해할 수 있도록 설계한다.

3-3) 느슨한 결합

프로그램의 각 부분 사이의 연결을 최소화하도록 설계하는 것을 의미한다. 클래스 사이의 연결을 최소화하기 위해 클래스 인터페이스에서의 추상화, 캡슐화, 정보 은닉과 같은 방법을 사용한다. 연결을 최소화하면 통합이나 테스트, 유지보수할 때 작업을 최소화할 수 있다.

3-4) 확장성

확장성은 내부 구조를 해치지 않고 시스템의 기능을 개선할 수 있다는 뜻이다. 다른 부분에 영향을 미치지 않고 시스템 일부분을 변경할 수 있다. 예측 가능한 변경 사상을 미리 고민하면 시스템에 입히는 충격을 최소화할 수 있다.

3-5) 재사용성

현재 시스템의 일부를 다른 시스템에 사용할 수 있도록 시스템을 설계하는 것을 의미한다.

## 6 장 클래스 다루기

초창기 개발자들은 프로그래밍을 명령문 수준으로 생각했다. 1970 년대와 1980 년대를 거치면서 개발자들이 루틴 관점에서 프로그래밍을 생각하기 시작했다. 21 세기에는 클래스 관점에서 프로그래밍을 이해하고 있다.

클래스는 연관성이 높고 잘 정의된 기능을 공유하는 데이터와 루틴의 모음이다. 앞서 설명했듯이 능력 있는 개발자가 되려면 현재 작업 중인 코드에 집중할 수 있게 프로그램에서 무시할 수 있는 부분을 최대화할 수 있어야 한다. 클래스는 그러한 목표를 달성하기 위한 일차적인 도구다.

### 1. 클래스의 토대: 추상 데이터형

추상 데이터형은 데이터와 데이터를 처리하는 연산의 집합이다.
객체지향 프로그래밍을 이해하기 위해서는 ADT(추상 데이터형)을 반드시 이해해야 한다. ADT 를이해하지 못하고서는 이름만 "클래스"인 클래스를 작성하게 될 것이다. 그런 클래스는 실제로는 연관성이 높지 않은 데이터와 루틴을 편의를 위해 보관하는 상자와 다를 게 없다. ADT 를 이해한다면 처음에 구현하기 쉽고 나중에 변경하기 쉬운 클래스를 만들 수 있다.

1-1) ADT 를 사용할 때 좋은 점

1.  구현 세부 사항을 감출 수 있다.
2.  변경이 전체에 영향을 미치지 않는다.
3.  인터페이스가 더 많은 정보를 제공하도록 만들 수 있다.
4.  성능을 향상시키기 쉽다.
5.  프로그램이 명백하게 정확해진다.
6.  프로그램의 가독성이 높아진다. (currentFont.attribute = BOLD -> currentFont.SetBoldOn())
7.  전체 프로그램에 데이터를 넘길 필요가 없다. (ADT 에 속하지 않은 루틴은 그 데이터를 신경 쓸필요가 없다.)
8.  저수준 구현 구조체 대신 현실 세계의 개체를 다룰 수 있다. (폰트를 다루는 연산들을 정의하면 프로그램 대부분이 배열 접근이나 구조체 정의, True 와 False 같은 형태가 아닌 폰트의 관점에서 돌아간다.)

1-2) 전형적인 저수준 데이터형을 저수준 데이터형이 아닌 ADT 로 만들거나 사용하라

스택이 직원 집합을 나타낸다면 ADT 를 스택이 아닌 직원들로 이해하자.
가능한 높은 추상화 수준에서 이해하려고 하라.

1-3) 간단한 객체도 ADT 로 취급하라

굉장히 큰 데이터를 다룰 때만 추상 데이터형을 사용할 필요는 없다.

### 2. 좋은 클래스 인터페이스

클래스 추상화에 대한 평가는 공개 루틴의 집합, 즉 클래스의 인터페이스를 기초로 한다.

2-1) 클래스는 오직 하나의 ADT 만 구현해야 한다.

하나 이상의 ADT 를 구현한 클래스를 발견하거나 클래스가 구현한 ADT 가 무엇인지 확신할 수 없다면클래스를 잘 정의된 하나 이상의 ADT 로 재구성해야 할 때가 된 것이다.

2-2) 클래스 인터페이스가 일관된 추상화 수준을 갖도록 한다.

ex) AddEmployee 함수와 NextItemInList 함수
AddEmployee 루틴의 추상화는 '직원'을 다루지만 NextItemInList 루틴의 추상화는 '리스트'를다룬다.

2-3) 클래스가 구현하고 있는 추상화가 무엇인지 이해해야 한다.

2-4) 서로 반대되는 기능을 갖는 서비스 쌍을 제공하라.

대부분의 연산은 유사하거나 같거나 정반대의 연산을 갖고 있다. 불을 켜는 연산이 있으면 끄는 연산도 필요할 것이다. 리스트에 항목을 추가하는 연산이 있으면 리스트에서 항목을 제거하는 연산이 필요할 것이다. 클래스를 설계할 때 각 공개 루틴에 반대되는 기능이 필요한지 확인한다. 불필요하게 반대되는 기능을 만들어서는 안 되지만, 꼭 필요한지 확인해야 한다.

2-5) 관련이 없는 정보를 다른 클래스로 옮겨라

### 3. 좋은 캡슐화

캡슐화는 추상화보다 더 강력한 개념이다. 추상화는 구현 세부사항을 무시할 수 있는 모델을 제공함으로써 복잡성 관리에 도움을 준다. 캡슐화는 세부사항을 알고 싶어 할 때조차 이를 원천적으로 봉쇄해버리는 더 강력한 방법이다.

캡슐화 없이는 추상화가 깨질 수 있기 때문에 이 두 개념은 연관되어 있다. 경험상 추상화와 캡슐화 모두 갖고 있든지 둘 다 없든지 둘 중 하나다. 중간은 없다.

3-1) 클래스와 멤버의 접근성을 최소화하라

접근성의 최소화는 캡슐화를 장려하기 위해서 고안된 여러 가지 규칙 중 하나다. 중요한 지침은
"어떻게 해야 인터페이스 추상화의 무결성을 최상으로 유지할 수 있는가?"다. 확신이 서지 않으면 일반적으로 숨기는 것이 숨기지 않는 것보다 낫다.

3-2) 멤버 데이터를 public 으로 노출하지 말라

멤버 데이터 노출은 캡슐화에 위반되고 추상화를 어렵게 만든다. 클라이언트 코드가 클래스를 직접 조작하고 이 값들이 변경되었는지조차 모를 수 있게 된다.

3-3) 내부 구현 세부 사항을 클래스의 인터페이스에 입력하지 말라

3-4) 클래스의 사용자를 가정하지 말라

3-5) 프렌드(Friend) 클래스를 피하라

3-6) 코드를 작성할 때의 편의성보다 가독성이 높은 코드를 작성하라

코드는 작성하는 것보다 읽는 데 훨씬 오래 걸린다.

### 4. 설계와 구현의 문제

4-1) 포함("has a" 관계)

포함은 클래스가 데이터 요소나 객체를 포함한다는 간단한 개념이다. 예를 들면 직원은이름을 "갖고" 전화번호를 "갖고" 세금 ID 등을 "갖는"다.

4-2) 7 개 이상의 데이터 멤버를 포함하는 클래스를 주의하라.

클래스가 7 개 이상의 데이터 멤버를 포함한다면 클래스를 더 작은 클래스로 나눌 수 있는지 고민해보자.

4-3) 상속("is a" 관계)

상속은 한 클래스가 다른 클래스의 특별한 형태라는 개념이다. 상속의 목적은 두 개 이상의 파생 클래스에서 공통적으로 사용되는 요소를 갖는 기본 클래스를 정의하여 더 간단한 코드를 작성하는 데 있다. 공통적인 요소는 루틴 인터페이스나 구현부, 데이터 멤버, 데이터 형이 될 수 있다. 상속은 코드와 데이터를 기본 클래스에 집중시킴으로써 여러 위치에서 반복적으로 사용하는 것을 피하게 해준다. 상속을 사용하기로 했다면 다음 사항을 결정해야 한다.

1.  각 멤버 루틴의 경우, 루틴이 파생 클래스에서 보일 것인가? 기본 구현을 포함할 것인가?
    기본 구현의 오버라이드(Override)가 가능할 것인가?
2.  각 데이터 멤버(변수, 이름 상수, 열거형 등 포함)의 경우, 데이터 멤버가 파생 클래스에서 보일 것인가?

개발자가 기존 클래스를 상속하여 새로운 클래스를 작성하기로 결정할 때 새로운 클래스는 기존 클래스의 특수화된 버전이다.

파생 클래스가 기본 클래스에 정의된 인터페이스 계약을 완벽하게 따르지 않는다면 상속은 올바른 구현 기법이 아니다. 포함이나 상속 계층 변경을 고려해 보라.

4-4) 상속을 고려해서 설계하고 문서화하라. 그게 아니면 상속을 금지하라

상속은 프로그램을 복잡하게 만들기 때문에 위험한 기법이다. 자바 전문가인 조슈아 블로크가 말했듯이 "상속을 고려해서 설계하고 문서화하라. 그게 아니면 상속을 금지하라"

기본 클래스에 정의된 모든 루틴은 파생 클래스에서 사용될 때도 의미가 같아야 한다.

4-5) 공통으로 사용되는 인터페이스와 데이터, 행위를 상속 단계에서 가능한 한 가장 높은 곳으로 옮겨라

4-6) 인스턴스가 하나뿐인 클래스를 의심하라

생성한 인스턴스가 하나뿐일 때는 객체를 클래스로 잘못 알고 설계했을 수 있다.

4-7) 파생 클래스가 하나뿐인 기본 클래스를 의심하라

꼭 필요한 것 이상으로 상속 구조를 만들어서는 안 된다

4-8) 깊은 상속 구조를 피하라

일부 객체지향적 기법은 복잡성을 줄이기보다 증가시키는 경향이 있다.

대부분의 사람이 상속 구조가 두세 단계 이상 되면 머리에서 한 번에 처리하는 데 어려움을겪었다.

깊은 상속 구조는 복잡성을 증가시켜 애초에 상속을 사용하는 목적을 무색하게 만든다. 가장 중요한 기술적 임무를 명심하라. 중복된 코드를 피하고 복잡성을 최소화하기 위해서 상속을 사용하고 있는지를 확인하라.

4-9) 광범위한 타입 검사보다 다형성을 택하라

자주 반복되는 case 문을 보면 상속을 적용하는 게 낫지 않을까 생각하게 된다.

4-10) 모든 데이터를 보호가 아닌 비공개로 만들어라

조슈아 블로크가 말했듯이 "상속은 캡슐화를 망가뜨린다." 어떤 객체로부터 상속을 받을 때는 객체의 보호 루틴과 데이터에 대한 접근 권한을 얻게 된다. 정말로 파생 클래스에서 기본 클래스의 특성에 접근해야 한다면 protected 로 선언된 함수를 대신 제공하라.

4-11) 다중 상속

상속은 강력한 도구다. 상속은 나무를 자르기 위해서 수동 톱 대신 전기톱을 사용하는 것과 같다. 상속은 주의해서 사용한다면 굉장히 유용할 수 있지만, 주의 사항을 잘 지키지 않는 사람이 사용하면 위험한 기법이다.

### 5. 상속에 관한 규칙이 왜 이렇게 많은 것일까?

상속을 사용하면 그만큼 복잡성을 관리하기가 어렵기 때문이다. 복잡성을 관리하고 싶다면 상속을 최대한 멀리해야 한다. 다음은 언제 상속을 사용하고 언제 포함을 사용할지를 요약한 내용이다.

5-1) 다중 클래스가 공통 데이터는 공유하지만 행위를 공유하지 않는다면 그 클래스가 포함할 수 있는 공통 객체를 만든다.
5-2) 다중 클래스가 공통 행위는 공유하지만 데이터를 공유하지 않는다면 공통적인 루틴을 정의한 기본 공통 클래스를 상속받는다.
5-3) 다중 클래스가 공통 데이터와 행위를 공유한다면 공통적인 데이터와 루틴을 정의한 기본 공통 클래스를 상속받는다.
5-4) 인터페이스를 제어하기 위한 기본 클래스가 필요할 때는 상속을 하고 인터페이스를 제어하고 싶다면 포함한다

5-5) 클래스에 가능한 한 적은 수의 루틴을 유지하라

클래스당 루틴의 수가 늘어나는 것이 오류율 상승과 연관되어 있다.

5-6) 다른 클래스에 대한 간접적인 루틴 호출을 최소화하라

5-7) 일반적으로 클래스가 다른 클래스와 협력하는 정도를 최소화하라

### 6. 생성자

6-1) 가능하다면 모든 멤버 데이터를 모든 생성자에서 초기화하라

6-2) 비공개 생성자를 사용해 싱글턴 속성을 구현하라

6-3) 다른 사실이 증명될 때까지 얕은 복사보다 깊은 복사를 택하라

### 7. 클래스를 작성해야 하는 이유

7-1) 현실 세계의 객체를 모델링한다

7-2) 추상 객체를 모델링한다

7-3) 복잡성을 줄인다

클래스를 생성하여 정보를 감추면 그러한 정보에 대해서 생각하지 않아도 된다. 물론 클래스를 작성할 때 어떤 정보를 다룰 것인지 고민해야 한다. 하지만 클래스를 작성하고 난 후에는 세부 사항을 잊고 내부 작동 방식에 대해 몰라도 클래스를 사용할 수 있게 된다. 클래스를 생성하는 또 다른 이유는 코드의 크기를 최소화하거나 유지보수를 쉽게 하거나 정확성을 향상시키는 것이다.
하지만 클래스의 추상적인 능력이 없다면 복잡한 프로그램을 관리하는 것이 불가능해진다.

7-4) 복잡성을 고립시킨다

오류가 발생했을 때 오류가 코드 전체에 퍼져 있지 않고 어떤 클래스 안에만 있다면 훨씬 찾기 쉬울 것이다. 오류를 수정해도 해당 클래스만 수정하면 되고 다른 코드는 손을 대지 않기 때문에 다른 코드에 영향을 미치지 않을 것이다.

7-5) 변경의 효과를 제한한다

변경할 가능성이 있는 부분을 고립시켜서 변경의 효과를 단일 클래스나 소수의 클래스로 제한한다. 변경될 가능성이 가장 높은 부분을 가장 쉽게 변경할 수 있도록 설계하라.

### 8. 피해야 할 클래스

8-1) god 클래스를 생성하지 말라

모든 것을 알고 있고 모든 것을 할 수 있는 전지전능한 클래스를 생성하지 말라. 어떤 클래스가 Get()과 Set() 루틴을 사용하여 다른 클래스로부터 데이터를 가져오고 있다면 그 기능을 다른 클래스로 구성하는 방법을 고려해보자

8-2) 관련이 없는 클래스를 제거하라

클래스가 행위는 없이 데이터로만 구성된다면 그 클래스가 정말로 클래스인지 생각해보고 그 클래스를 없애서 클래스의 멤버 데이터가 다른 클래스의 속성이 될 수 있을 지 고려해본다

8-3) 동사를 뒤에 붙이는 클래스를 피하라

데이터는 없이 행위로만 구성된 클래스는 일반적으로 클래스가 아니다.

### 9. 클래스를 넘어서: 패키지

클래스는 현재 모듈화를 달성하기 위한 최고의 방법이다. 하지만 모듈화는 범위가 큰 주제이며 클래스를 넘어선다.

### 10. 체크리스트: 클래스 품질

10-1) 추상 데이터형
-> 프로그램에 있는 클래스는 추상 데이터형으로 생각하고 그러한 관점에서 클래스의 인터페이스를 평가했는가?

10-2) 추상화
-> 클래스에 핵심적인 목적이 있는가?
-> 클래스의 이름이 잘 지어졌고 클래스의 이름이 핵심적인 목적을 설명하고 있는가?
-> 클래스의 인터페이스가 일관성 있는 추상화를 제공하는가?
-> 클래스의 인터페이스가 클래스의 사용 방법을 분명하게 만들고 있는가?
-> 클래스의 구현 세부 사항을 전혀 알 필요가 없을 정도로 클래스의 인터페이스가 추상적인가? 클래스를 블랙박스로 취급할 수 있는가?
-> 다른 클래스들이 클래스의 내부 데이터를 쓸데 없이 간섭할 필요가 없을 만큼 클래스의 서비스가 완전한가?
-> 관련 없는 정보를 클래스에서 제거했는가?
-> 클래스를 컴포넌트 클래스로 분할하는 것에 대해서 생각해 봤는가? 그리고 최대한 분할했는가?
-> 클래스를 수정할 때 클래스 인터페이스의 무결성을 유지하고 있는가?

10-3) 캡슐화
-> 클래스가 멤버에 대한 접근성을 최소화하고 있는가?
-> 클래스가 멤버 데이터의 노출을 피하고 있는가?
-> 클래스가 프로그래밍 언어가 허용하는 만큼 다른 클래스로부터 구현 사항을 감추고 있는가?
-> 클래스가 파생 클래스를 포함해 그 사용자에 대한 가정을 피하고 있는가?
-> 클래스가 다른 클래스에 독립적인가? 느슨하게 결합됐는가?

10-4) 상속
-> 상속이 'is a' 관계를 모델링하기 위해서만 사용됐는가? 다시 말해 파생 클래스가 LSP 를 따르고 있는가?
-> 클래스의 설명 문서가 상속 전략을 기술하고 있는가?
-> 파생 클래스가 오버라이드 불가능한 루틴에 대해서 "오버라이딩"을 피하고 있는가?
-> 공통적인 인터페이스, 데이터, 행위가 상속 트리에서 최대한 높은 곳에 있는가?
-> 상속 단계가 적절한 수준인가?
-> 기본 클래스에 있는 모든 데이터 멤버가 protected 가 아닌 private 인가?

10-5) 그 밖의 구현 문제
-> 클래스가 7 개 이하의 데이터 멤버를 포함하고 있는가?
-> 클래스가 다른 클래스에 대한 직접적이고 간접적인 루틴 호출을 줄였는가?
-> 클래스가 꼭 필요한 범위에서만 다른 클래스와 협동 작업을 하는가?
-> 모든 멤버 데이터를 생성자에서초기화했는가?
-> 얕은 복사를 해야 하는 합당한 이유가 없다면 얕은 복사 대신 깊은 복사로 사용되도록 클래스를 설계했는가?

10-6) 언어에 따른 문제
-> 사용 중인 프로그래밍 언어에서 클래스에 관한 이슈를 조사했는가?

## 7 장 고급 루틴

### 1. 루틴을 작성하는 이유

1-1) 복잡성을 줄인다.

1-2) 중복 코드를 피한다.

1-3) 코드의 실행 순서를 감춘다.

1-4) 포인터 연산을 감춘다.

1-5) 복잡한 불린 테스트를 단순화한다.

### 2. 루틴의 설계

루틴을 작성하는 목적은 한 가지 일을 잘하도록 하는 것이지 여러 가지 일을 처리하는 데 있지 않다.
응집성이 높을 수록 코드의 오류가 적다.

### 3. 좋은 루틴의 이름

3-1) 루틴이 하는 모든 것을 표현하라.

3-2) 의미가 없거나 모호하거나 뚜렷한 특징이 없는 동사를 사용하지 말라.

3-3) 루틴의 이름을 숫자만으로 구분하지 말라.

3-4) 루틴 이름의 길이에 신경 쓰지 말라.

"명료함"에 초점을 맞춰야 한다.

3-5) 프로시저의 이름을 지을 때 확실한 의미가 있는 동사를 객체 이름과 함께 사용하라.

ex) PrintDocument(), CalcMonthlyRevenues()

3-6) 반의어를 정확하게 사용하라

ex) add/remove, insert/delete

### 4. 루틴의 길이

4-1) 가장 적합한 최대 길이는 한 화면에 꽉 찰 정도나 출력했을 때 한두페이지 분량인 대략 50 줄에서 150 줄 정도다.

4-2) 결과적으로 200 줄 이상의 긴 루틴을 작성하고자 할 때는 주의해야 한다.

### 5. 루틴 매개변수 처리

5-1) 매개변수를 입력-수정-출력 순서로 입력한다.

5-2) 유사한 매개변수가 여러 루틴에서 사용된다면 해당 매개변수를 항상 같은 순서로 입력한다.

5-3) 모든 매개변수르 사용한다.

5-4) 상태 변수나 오류 변수를 마지막에 입력한다.
이 변수들은 루틴의 주요 목적에 크게 영향을 주지 않고 값을 반환하기 위한 매개변수이기 때문이다.

5-5) 루틴의 매개변수를 연산을 위한 변수로 사용하지 않는다.

5-6) 매개변수에 대한 제약사항을 주석으로 작성한다.

제약 사항을 주석으로 작성하는 것보다 훨씬 좋은 방식은 어설션을 사용하는 것이다.

5-7) 루틴 매개변수의 수를 7 개 정도로 제한한다.

## 8 장 방어적 프로그래밍

"방어적 프로그래밍은 방어적으로 운전하는 습관에 가깝다. 방어 운전을 하면 다른 운전자가 비정상적으로 운전할 수 있다는 생각을 항상 갖고 있어서 좀 더 안전하게 운전할 수 있다."

"프로그램은 언제나 문제가 있고 지속해서 변경될 것이고 똑똑한 개발자는 그러한 상황에 대처할 수 있는 코드를 개발할 것이다."

### 1. 좋은 프로그램은 쓰레기를 입력받았다고 하더라도 절대로 쓰레기를 내보내지 않는다.

1-1) 외부로부터 들어오는 모든 데이터의 값을 검사하라.
1-2) 루틴의 모든 입력 매개변수 값을 검사하라.
1-3) 잘못된 입력을 어떻게 처리할 것인지를 결정하라.

### 2. 어설션(Assertion) - 루틴이나 매크로 실행 시 프로그램이 스스로 검사할 수 있도록 사용하는 코드

2-1) 발생이 예상되는 상황에 대해서는 오류 처리 코드를 사용하되, 절대로 발생해서는 안 되는 조건에 대해서는 어설션을 사용하라.

2-2) 매우 견고한 코드를 작성하기 위해서는 어설션은 무조건 포함하고 그 다음에 오류를 처리하라.

### 3. 예상되는 오류를 어떻게 처리할 것인가?

3-1) 중립적인 값을 반환한다.
3-2) 다음에 오는 유효한 데이터로 대체한다.
3-3) 이전과 같은 값을 반환한다.
3-4) 가장 가까운 유효한 값으로 대체한다.
3-5) 경고 메세지를 파일에 기록한다.
3-6) 오류 코드를 반환한다.
3-7) 오류 처리 루틴이나 객체를 호출한다.
3-8) 오류가 발생한 곳에서 오류 메세지를 출력한다.
3-9) 종료한다.

### 4. 예외를 신중하게 사용하면 복잡성을 줄일 수 있다. 무분별하게 사용하면 이해하기가 거의 불가능한 코드를 만들 수 있다.

### 5. 정말로 예외적인 조건인 경우에만 예외를 던져라.

### 6. 예외의 대안을 고려해 본다.

어떤 개발자들은 프로그래밍 언어가 특정 오류 처리 메커니즘을 제공한다는 이유만으로 오류를 처리하는 데 예외를 사용한다.

## 9 장 의사코드 프로그래밍 프로세스

### 1. 루틴의 이름을 신중히 짓는다.

루틴 이름이 좋다는 것은 프로그램 품질이 좋다는 지표고 실제로 어려운 작업이기도 하다.

### 2. 루틴을 어떻게 테스트할 것인지 결정한다.

### 3. 코드의 품질과 생산성 모두를 향상시킬 수 있는 최고의 방법은 좋은 코드를 재사용하는 것이다.

### 4. 의사코드를 고수준의 주석으로 변환한다.

"취미로 프로그래밍을 하는 사람과 전문적인 개발자 사이의 가장 큰 차이점 중 하나는 미신에서 벗어나 이해하려고 노력하면서 성장한다는 데 있다. 여기에서 '미신'은 몸을 오싹하게 하거나 보름달이 뜰 때 오류가 더 발생하는 프로그램을 말하는 게 아니라 코드를 이해하려고 하기보다는 코드의 느낌을 더 믿는 현상을 말한다."

"작동하는 루틴만으로는 충분하지 않다. 왜 작동하는지를 모른다면 알 때까지 연구하고 토론하고 다른 설계로 실험해 본다."