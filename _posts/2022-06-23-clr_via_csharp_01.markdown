---
layout: post
title:  "CLR via C#"
date:   2022-04-04 11:03:59 +0900
categories: Csharp CLR
---

<br>
<br>
<br>
제프리 리처 아저씨의.. CLR via C# 책을보고 씨샵 공부를 시작했다..<br>
<br>
원래는 시작하세요 c# 9.0 프로그래밍 책을 보고 씨샵 공부를 하고 있었는데..<br>
<br>
추천 받은것도 있고..<br>
windows via c/c++ 을 보면서 제프리 리처 아저씨 고수구나(?) 하고 알아버려서..<br>
<br>
<br>
<br>
초반부 타입 쪽 부터 볼건데..<br>
이부분은 빠르게 짚고 넘어갈 수 있을것 같다. <br>
다른 책 (시작하세요! c# 9.0 프로그래밍) 책으로도 본 부분이기도 해서..<br>
<br>
<br>
그럼 본문으로..<br>
<br>
<br>
<br>
<br>
c# 에서 모든 타입은 System.Object를 상속한다. <br>
```c#
class a {} 
```
랑 <br>
```c#
class a : System.Object {}
```
랑 같다는 얘기다.<br>
<br>
그리고 나중에 아마도 나올거 같지만, c#은 new 만 있고 delete가 없다. <br>
GC 언어니까. <br>
<br>
대신 IDispose와 DIspose 패턴이 있는데 나중에도 안나오면 그때가서 다시 적어보는걸로..<br>
<br>
그리고 c#에 있는 타입 캐스팅을 위한 연산자인 is와 as는 예외를 절대로 발생시키지 않는다.<br>
안전한 타입 캐스팅에 사용하자.<br>
a is b 면, b b as a 로 <br>
위 is/as 연산자와 관련된 문법중에 패턴매칭문법이 있다.<br>
```c#
a is int b
```
로 a가 int면, b를 쓸 수 있다. <br>
<br>
<br>
<br>
namespace와 관련된 부분은 스킵하고 넘어가겠다. <br>
굳이 c#을 공부하고 다시 확인하는데 있어서 언급하고 넘어갈 부분은 아닌것 같다. <br>
using 키워드를 다루는것이면 모르겠지만.. <br>
<br>
<br>
이 글에서는 이정도만 하고 마무리 하겠다..<br>
<br>
하루에 최소 한번.. 쓸려고 노력 할거지만..<br>
이 책을 다보는데 얼마나 걸릴까 궁금하다.. <br>
<br>
