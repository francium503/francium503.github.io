---
layout: post
title:  "final 과 override"
date:   2022-03-25 16:58:59 +0900
categories: cpp
---


final 과 override 키워드에 대해서 알아보자. <br>
<br>
<br>
이번에는 깊숙하게 들어가지 않고, 기능에 대해서만 알아보도록 하자. <br>
final은 c++11에 추가된 키워드로 해당 메소드,클래스 혹은 구조체가 마지막으로 재정의 되고, 그 자식에서는 더이상 재정의 되지 못하게 만드는 키워드 이다. <br>
[https://en.cppreference.com/w/cpp/language/final](https://en.cppreference.com/w/cpp/language/final)<br>
<br>
<br>
override는 다른 가상 함수를 override 했다는걸 나타내주는 지정자이다. <br>
특이하게 예약된 키워드가 아니라서 함수 이름으로 override 같은걸 사용 할 수 있다. <br>
[https://en.cppreference.com/w/cpp/language/override](https://en.cppreference.com/w/cpp/language/override)<br>
<br>
```c++
void override(); // OK, member function name, not a reserved keyword
```
<br>
새로운 키워드를 알았으니 다음부턴 모던(?) c++ 기능들을 써봐야 겠다.. <br>
<br>






