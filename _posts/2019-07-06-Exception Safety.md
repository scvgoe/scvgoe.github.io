---
layout: post
title: 'Exception Safety'
tags: [theory]
---

# What is exception safety?

2000년 David Abrahams는 본인의 저서 [Exception-Safety in Generic Components](https://www.boost.org/community/exception_safety.html)에서 Exception safety guarantees라는 개념을 설명했다. Exception safety guarantees란 class library를 구현하는 사람과 clients가 exception handling safety를 검증하는 데에 사용하는 일종의 가이드라인이다.

# Levels of exception safety

Exception safety에는 4가지 수준이 있다. (안전한 순으로 정렬)

1. **No-throw guarantee** (failure transparency): 모든 operation의 성공과 안전성이 보장되는 수준으로 exceptional situation이 발생하면 이것이 내부적으로 처리되며 clients에게 보이지 않고 역시 성공과 안정성이 보장된다.
2. **Strong exception safety** (commit or rollback semantics): Operation이 실패할 수 도 있는 수준이지만 실패한 operation이 다른 side effects를 불러오지 않는다는 것이 보장된다. 따라서 모든 data는 원래의 값을 유지한다.
3. **Basic exception safety** (no-leak guarantee): 실패한 Operation의 일부 동작이 side effects를 불러올 수 있는 수준이지만 모든 invariants들은 항상 보존되며 memory leaks를 포함한 어떠한 resource leaks도 없음이 보장된다. 저장된 기존의 data는 값이 변할 수는 있지만 여전히 유효한 값을 갖게 된다.
4. **No exception safety**: 어떠한 것도 보장되지 않는 수준
일반적으로 견고한 코드를 위해서는 적어도 basic exception safety 이상의 수준이 요구 된다. 높은 수준의 안전성은 달성하기가 어렵고 부가적인 copy로 인해 overhead가 발생 할 수도 있다. Exception safety의 핵심은 코드의 block (그것이 exceptions 이라도)이 실행 되고 난 뒤에도 프로그램의 실행이 계속됨이 보장되어야 한다는 것이다. 몇몇의 language들은 dispose pattern (with, try-with-resources)를 사용하여 이것들을 간단히 할 수 있도록 했다.

# Example of exception safety

C++의 std::vector나 Java의 ArrayList의 insert 함수를 구현하는 경우를 생각해보자. item x가 vector v에 추가되려고 할 때, vector는 x를 내부 list에 추가하고 count field를 update 해야한다. 이는 새로운 memory의 할당을 필요로 할 수도 있다.

1. **No-throw guarantee**: 구현하기 매우 어렵거나 불가능하다. memory 할당이 실패하여 exception을 던질 수 있기 때문이다. allocation failure를 handling하는 것은 문제가 많다. 계속된 할당 시도는 실패할 확률을 더 높이기 때문이다.
2. **Strong exception safety**: 상대적으로 구현하기 쉽다. 할당을 먼저 하고 난 뒤에 temporary buffer에 저장한다. 만약에 에러가 발생하지 않았다면 buffer에 저장된 값을 원래의 자리에 덮어 씌우고, 에러가 발생했을 경우는 원래의 값을 유지한다.
3. **Basic exception safety**: x가 성공적으로 삽입되었을 때, size field가 올바르게 보존됨을 보장하면 된다. 또 모든 할당은 성패에 관계 없이 resource leak이 일어나지 않는 방향으로 구현되어야 한다.
4. **No exception safety**: 삽입의 실패가 손상된 v, 잘못된 size value, 또는 resource leak 을 야기한다.

# How to: Design for Exception Safety In C++

1. **Keep Resource Classes Simple**: 수동 리소스를 클래스에서 캡슐화할 때에는 리소스 관리 이외에는 아무것도 하지 않도록 해야한다. 또 가능하다면 smart pointer를 사용 하는 것이 좋다.
2. **Use the RAII Idiom to Manage Resources**: RAII (Resource Acquisition Is Initialization) Idiom은 C++의 창시자인 Bjarne Stroustrup에 의해 제안된 디자인 패턴이다. RAII 패턴은 C++ 같이 개발자가 직접 resource 관리를 해주어야 하는 언어에서 leak 을 방 지하기 위한 중요한 기법으로 해당 리소스의 사용 scope이 끝날 경우에 자동으로 해 제를 해주며 exception이 발생하거나 하는 경우에도 획득한 자원이 해제됨을 보장 해야 한다.

# Exception-Safe Implementation Techniques

1. 어떤 정보들의 대체재를 저장하기 전까지는 정보를 삭제하지 말 것
2. exception이 발생했을 때라도 객체를 항상 유효한 상태로 유지할 것
3. resoucre leaks를 피할 것

---

### 참고

https://en.wikipedia.org/wiki/Exception_safety<br>
https://msdn.microsoft.com/library/hh279653.aspx<br>
http://lesstif.tistory.com/entry/RAII-Resource-Acquisition-Is-Initialization<br>
http://www.stroustrup.com/except.pdf