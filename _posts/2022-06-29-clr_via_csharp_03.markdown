---
layout: post
title:  "기본 타입"
date:  2022-06-29 20:20:59 +0900
categories: Csharp CLR
---

<br>
<br>
<br>
<br>
컴파일러가 직접 지원하는 데이터 타입을 기본 타입 이라고 한다고 한다.<br>
<br>
기본 타입들은 프레임워크 클래스 라이브러리 상에 정의된 타입과 직접 연결된다. <br>
C#의 경우 Int는 System.Int32 타입으로 바로 연결된다. <br>
이렇게 바로 대응 되는 타입들이 있는데 이건 책 130페이지에.. 단순히 목록이라 생략한다.. <br>
<br>
중요한건 C#에서 같은 경우 위와 같은 이유 때문에 int가 64비트, 32비트 구별없이 System.Int32로 연결된다는점이다. <br>
C/C++에서처럼 x86인지, x64인지에 따른 사이즈 변경을 신경 안 쓸수 있다. <br>
<br>
기본타입연산에는 오버플로우 검사가 있다. <br>
CLR에서 IL을 통해 직접 해당 동작은 선택할 수 있도록 해준다. <br>
add 와 add.ovf<br>
기본적으로는 검사를 안한다. <br>
컴파일러 스위치에 /checked+ 를 넣으면 오버플로우를 검사하며, System.OverflowException 예외를 던진다. <br>
코드로도 하는 방법이 있다. <br>

```c#
Byte a = 100;
a = checked((Byte) a + 200));
```

<br>
같은 방법과 checked {} 블록으로 감싸는 방법등, 아니면 역으로 unchecked을 사용하는 방법등이 있다. <br>
<br>
<br>
System.Decimal 타입은 특별한 타입이다. <br>
CLR은 Decimal을 기본타입으로 취급하지 않는다. <br>
 = CLR은 IL 명령을 제공하지 않는다. <br>
 <br>
 .NET Framwork SDK는 Decimal 에 대해 add, subtract, multiply, divide 들을 위한 정적 메서드가 있고 +,-,*,/ 등 각종 연산자들을 오버로딩 했다.<br>
 <br>
 Decimal 타입을 사용하는 코드를 컴파일하면, 컴파일러는 Decimal에 대해 서술된 모든 계산식을 Decimal 타입의 멤버에 대한 호출로 대치된다. <br>
 이로 인해서 CLR이 제공하는 기본 타입의 코드보다 느리게 동작한다. <br>
 또, Decimal에 대한 IL 코드가 없어서 checked, unchecked 를 사용할 수 없다. <br>
 대신 안전하지 않은 연산에 대해서는 항상 OverflowException이 발생한다. <br>
 <br>
 비슷한 예로 System.Numerics.BigInteger 가 있다. <br>
 특이한 점으로는 무한대의 수를 표현 하므로 OverflowException은 없지만, 메모리 부족으로 인한 OutofMemory가 발생 할 수 있다. <br>
 <br>
 <br>
 
