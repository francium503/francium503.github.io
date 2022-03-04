---
layout: post
title:  "타입 캐스팅"
date:   2022-03-04 15:49:59 +0900
categories: cpp
---

c++에서 존재하는 4가지 타입 캐스팅에 대해서 말해보자.. <br>

<br>
4가지의 c++ 스타일 캐스팅이 있다. <br>
static_cast, dynamic_cast, reinterpret_cast, const_cast <br>
<br>
각각의 캐스팅에 대해 알아보자 <br>
<br>
<br>
### static_cast
<br>
<br>
우선 static_cast 다.
사용법은
```c++
static_cast<type name>(var);
```
C 스타일의 형변환이랑 완전히 같다. <br>
```c++
int i;
char c;

i = c;
i = static_cast<int>(c);
```
일 때, 둘 다 동일한 어셈으로 나온다. <br>
<br>
<br>
<br>
### dynamic_cast
<br>
<br>
다음으로는 dynamic_cast 이다. <br>
런타임에서 안전한 캐스팅을 위해서 사용한다. <br>
캐스팅 실패시 예외를 뿜는다. `std::bad_cast` exception <br>
```c++
dynamic_cast<new type*>(old);
```
일때 old 가 nullptr 이면 new type* 형태의 nullptr을 준다. <br>
RTTI 정보에 의존해서 캐스팅 한다. *[참고](https://en.cppreference.com/w/cpp/language/dynamic_cast)<br>
<br>
<br>
<br>
### reinterpret_cast
<br>
<br>
reinterpret_cast다.<br>
이 캐스팅도 처음 얘기한 static_cast 와 어셈은 같게 나온다. <br>
```c++
i = reinterpret_cast<int*>(c);
mov   rax, qword ptr [c]
mov   qword ptr [i], rax
```
근데 자료형 -> 다른 자료형은 안되고 포인터 ->포인터, 포인터->일반변수, 일반변수->포인터 로 포인터 관련된 캐스팅이 가능하다. <br>
근데 어느 타입의 포인터를 다른 타입의 포인터로 캐스팅 하는 일이 많은가..? 에 대해서 생각해보면 <br>
상속관계에 있는 객체에 대해서는 dynamic_cast 를 쓸거 같고 <br>
아마도 stream 형식으로 데이터를 넣었을때 원하는 만큼의 데이터를 읽는데 쓰지 않을까 싶다.. <br>
<br>
<br>
<br>
### const_cast
<br>
<br>
마지막으로 const_cast다. <br>
const성이나 volatile을 제거하는 cast이다.<br>
```c++
void foo(int& n){
}

const int& i = 10;

foo(const_cast<int&>(i));
```
같은 경우에 쓴다. <br>
아마도 const char * 로 선언된 문자열을 넘길때 잘 쓸 수 있을거 같은데..<br>
사실 const 가 달려있는 애는 원래 의도대로 변경하지 않고 써야 되지 않을까 싶다..<br>
<br>
<br>
이렇게 오늘도 누가 물어보면 바로 대답이 안나올거 같은 c++ 캐스팅 4종류에 대해서 알아봤다.<br>
오늘도 지식의 얕음을 깨닫고 간다.. <br>




