---
layout: post
title: GoF의 디자인 패턴 (Summary) - 5. 적응자(Adapter)
tags: [theory, design_pattern, book, gof, adapter]
---

## 의도

클래스의 인터페이스를 사용자가 기대하는 인터페이스 형태로 적응(변환)시킵니다. 서로 일치하지 않는 인터페이스를 갖는 클래스들을 함께 동작시킵니다.

---

## 다른 이름

래퍼(Wrappter)

---

## 활용성

적응자 패턴은 다음 상황에서 사용합니다.

* 기존 클래스를 사용하고 싶은데 인터페이스가 맞지 않을 때
* 이미 만든 것을 재사용하고자 하나 이 재사용 가능한 라이브러리를 수정할 수 없을 때
* (객체 적응자(object adapter)의 경우에) 이미 존재하는 여러 개의 서브클래스를 사용해야 하는데, 이 서브클래스들의 상속을 통해서 이들의 인터페이스를 다 개조한다는 것이 현실성이 없을 때. 객체 적응자를 써서 부모 클래스의 인터페이스를 변형하는 것이 더 바람직함.

---

## 구조

![Adapter](/img/adapter.png)

---

## 결과

클래스 적응자와 객체 적응자는 각각 장단점이 있습니다. 먼저 클래스 적응자를 살펴봅시다.

* Adapter는 명시적으로 Adaptee를 상속받고 있을 뿐 Adaptee의 서브클래스들을 상속받는 것은 아니므로, Adaptee의 서브클래스에 정의된 기능들을 사용할 수 없습니다.
* Adapter 클래스는 Adaptee 클래스를 상속하기 때문에 Adaptee에 정의된 행동을 재정의할 수도 있습니다.
* 한 개의 객체(Adapter)만 사용하며, adaptee로 가기 위한 추가적이 포인터 간접화는 필요하지 않습니다.
* 양방향 적응자를 통한 투명성 제공이 가능합니다. Target과 Adaptee의 인터페이스 모두를 제공할 수 있습니다.

객체 적응자를 사용하면 다음과 같은 특징을 경험할 수 있습니다.

* Adapter 클래스는 하나만 존재해도 수많은 Adaptee 클래스들(서브클래스 포함)과 동작할 수 있습니다.
* Adaptee 클래스의 행동을 재정의하기가 매우 어렵습니다.

---

## 의견

기존에 존재하는 다른 클래스를 내가 원하는 인터페이스에 맞추기 위해 사용하는 패턴으로, TextShape라는 클래스의 구현을 위해 Shape의 인터페이스와 TextView의 구현을 모두 상속받든지(클래스 버전), 아니면 TextView의 인스턴스를 TextShape에 포함시키고, TextView 인터페이스를 사용하여 TextShape를 구현합니다(객체 버전). 이 때 TextShape를 적응자(Adapter)라고 합니다.

---

### 참고
에릭 감마, 리처드 헬름, 랄프 존슨, 존 블리시디스. [GoF의 디자인 패턴](https://book.naver.com/bookdb/book_detail.nhn?bid=8942623). 프로텍미디어 2015

---

GoF의 디자인 패턴 (Summary) 시리즈입니다.

1. [GoF의 디자인 패턴 (Summary) - 1](/2018-12-24-GoF의-디자인-패턴-(Summary)-1)
2. [GoF의 디자인 패턴 (Summary) - 2. 추상 팩토리(Abstract Factory)](/2018-12-24-GoF의-디자인-패턴-(Summary)-2.-추상-팩토리(Abstract-Factory))
3. [GoF의 디자인 패턴 (Summary) - 3. 팩토리 메서드(Factory Method)](/2018-12-24-GoF의-디자인-패턴-(Summary)-3.-팩토리-메서드(Factory-Method))
4. [GoF의 디자인 패턴 (Summary) - 4. 단일체(Singleton)](/2018-12-24-GoF의-디자인-패턴-(Summary)-4.-단일체(Singleton))
5. **GoF의 디자인 패턴 (Summary) - 5. 적응자(Adapter)**
6. [GoF의 디자인 패턴 (Summary) - 6. 복합체(Composite)](/2018-12-24-GoF의-디자인-패턴-(Summary)-6.-복합체(Composite))
7. [GoF의 디자인 패턴 (Summary) - 7. 장식자(Decorator)](/2018-12-24-GoF의-디자인-패턴-(Summary)-7.-장식자(Decorator))
8. [GoF의 디자인 패턴 (Summary) - 8. 퍼사드(Facade)](/2018-12-24-GoF의-디자인-패턴-(Summary)-8.-퍼사드(Facade))
9. [GoF의 디자인 패턴 (Summary) - 9. 감시자(Observer)](/2018-12-24-GoF의-디자인-패턴-(Summary)-9.-감시자(Observer))
10. [GoF의 디자인 패턴 (Summary) - 10. 전략(Strategy)](/2018-12-25-GoF의-디자인-패턴-(Summary)-10.-전략(Strategy))
11. [GoF의 디자인 패턴 (Summary) - 11 템플릿 메서드(Template Method)](/2018-12-25-GoF의-디자인-패턴-(Summary)-11.-템플릿-메서드(Template-Method))
