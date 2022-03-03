---
layout: post
title:  "가상 함수"
date:   2022-03-03 15:39:59 +0900
categories: cpp
---

Virtual 함수.. <br>
가상함수로 이제 Class에 포함되면 vtable이 생긴다. <br>
가상함수 테이블은 각 가상 함수의 위치를 함수의 이름과 함께 매칭 시켜서 함수가 런타임에 call 됬을때 RTTI에 의해 결정된 객체의 형태를 찾아서 적절한 함수를 호출해준다. <br>
RTTI는 객체가 어느 타입인지 런타임에 알 수 있게 도와주는 기능(?) 이다. <br>
[RTTI MSDN 문서](https://docs.microsoft.com/en-us/cpp/cpp/run-time-type-information?view=msvc-170#:~:text=Run%2Dtime%20type%20information%20(RTTI)%20is%20a%20mechanism%20that,were%20implementing%20this%20functionality%20themselves.)
<br>
RTTI가 없으면 어떻게 될까? <br>
라는 질문이 면접에 나온다는거 같은데.. <br>
상속관계에서 소멸자에 virtual 을 붙여서 캐스팅 했을때 이제 소멸자 호출이 정상적으로 이루어지도록 도와주는데 여기서 문제가 생겨서 쌩까고 캐스팅한 타입의 소멸자를 호출해서 문제가 생길것 같다. <br>
는게 내 결론이다. <br>
위에 RTTI MSDN 문서에 `dynamic_cast` 가 있는데 부모 클래스를 자식 클래스로 `dynamic_cast` 로 캐스팅 하면 `nullptr` 이 리턴된다. <br>
```c++
child* c = dynamic_cast<child*>(p);
c->Foo();
```
이런경우를 조심해야할거 같다. <br>
단순 `static_cast` 의 경우에는 RTTI로 타입 추론이 되서 p의 Foo()가 잘 호출된다.<br>
<br>
가상함수에 = 0 을 붙여 주면 순수 가상함수가 된다. <br>
```c++
virtual void foo() = 0;
```
이때는 사용 할 클래스에서 해당 함수를 무조건 오버라이딩 해줘야 한다. <br>
순수 가상함수가 한개라도 있으면 추상 클래스 (abstract class) 라고 한다. <br>
클래스 내의 모든 멤버 함수가 순수 함수로 이루어져 있으면 인터페이스라고 한다. <br>
<br>
<br>
마지막으로 다시 정리해보면.. <br>
다른 타입으로 캐스팅 해서 호출했을때 가상함수 테이블을 참조해서 원형의 함수를 호출 할 수있게 도와주는 키워드.. 정도로 정리가 되는거 같다.. <br>
<br>
<br>
어째 점점 글을 쓸 수록 모르는게 많구나 하는 느낌이.. 든다.. <br>

