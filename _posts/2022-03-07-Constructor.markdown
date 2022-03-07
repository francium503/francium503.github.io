---
layout: post
title:  "생성자"
date:   2022-03-07 12:27:59 +0900
categories: cpp
---

생성자에 대해서 알아볼것은 복사 생성자, 이동 생성자 그리고 초기화 리스트 인것 같다. <br>
<br>
왜냐하면 당연하게도 저거에 대해서 물어봤을때 정확히 대답하지 못할것 같기 때문.. <br>
<br>
### 복사 생성자

<br>
<br>
우선 복사 생성자부터 알아보자. <br>
원형은 다음과 같다. <br>
```c++
Bar(const Bar &rhs);
```
<br>
복사 생성자는 깊은 복사를 위해서 사용한다. <br>
일반 변수에서는 복사할때 문제가 없지만, 객체에서 맴버 변수가 동적할당된 포인터일 때 <br>
얕은 복사를 해서 포인터를 값만 가져오게 되면 참조하고 있는 포인터가 해제되는 등의 문제가 발생 할 수 있다. <br>
이런 문제를 해결하면서 값은 완전히 같은 객체를 만들때 사용한다. <br>
<br>
호출되는 시점은 다음과 같다. <br>
<br>
```c++
Bar b2(b1);
foo(b1);

Bar b3 = b1;

Bar b4 = foo2(b1); // Bar foo2(Bar);
```
<br>
생성자에 객체를 넣어서 전달할때, 함수에 객체를 매개변수로 집어넣을때. <br>
이 두가지 경우에는 매개변수로 전달 되는 경우에는 복사생성자가 호출된다고 보면 될거 같다. <br>
<br>
객체를 대입 연산 할 때 <br>
그리고 객체를 리턴타입으로 반환 받을때 호출한다. <br>
<br>
근데 대입 연산의 경우 `operator=` 인데 해당 객체에 연산자와 복사 생성자가 같이 있으면 어떻게 될까 궁금해서 해봤다.
```c++
Bar& operator=(Bar&& rhs);
```
<br>
는 c++ 11부터 막혔다고 한다. <br>
궁금해서 해보니 컴파일이 막혀서 좋은거 알게 된거 같다. <br>
<br>
<br>
### 이동 생성자

<br>
<br>
이동 생성자 차례이다. <br>
원형은 다음과 같다. <br>
```c++
Bar(Bar&& rhs);
```
<br>
나에게 취약한 모던 c++(?) 인 c++11에서 추가된 개념이다.. <br>
[cppreference.com](https://en.cppreference.com/w/cpp/language/move_constructor) 에 의하면 rvalue 나 xvalue 에 의해서 초기화 될때 호출 된다고 한다. <br>
예를 들면 <br>
```c++
Bar a = std::move(b);
Bar a(std::move(b));
foo(std::move(a));
return a;
```
등 에서 이다. <br>
return a; 가 위에 복사 생성자에서 `Bar foo2(Bar);` 를 호출 했을때가 비슷한거 같아서 추가 한뒤 호출 해보면.. <br>
<br>
```c++
Bar b4 = foo2(b1); // Bar foo2(Bar);
```
<br>
의 경우에서는 move constructor로 바껴서 호출된다. <br>
이런 제한적인 상황에서 사용되는 이동 생성자를 만든 이유는 뭘까.. <br>
위에 리턴 타입으로 값을 받는 경우에 값을 받고 난 뒤 해당 리턴 타입은 사라진다. (소멸자 호출) <br>
값은 남아있어야 하는 상황에서 객체 내부에 할당 된 메모리에 대해서 깊은 복사를 하면 매우 느릴 것이다. <br>
포인터만 옮긴 뒤 끝내면 더 빠르다. <br>
라고 단순히 생각했는데.. 일반적인 상황을 가정하고 코딩을 하면 소멸자에서 당연히 `delete` 를 호출한다. <br>
<br>
그럼 이동 한 후의 객체에서 접근하면 오류가 뿜! <br>
그래서 언제 쓸지 더 찾아봤다. <br>
STL 에서 컨테이너에 객체를 넣을때 쓴다고 한다. <br>
`emplace 블라블라` 와 함께 사용하면 이동 생성자가 호출된다. <br>
일반적인 `push_back` 에서 객체를 생성 - 복사 - 삽입 이지만 `emplace_back` 에서는 내부에서 생성 - 삽입 이 된다. <br> 
혹은 내가 아직 잘 모르는 스마트 포인터를 사용하면서 소멸자에서 delete 콜을 손으로 안하고, <br>
참조를 더이상 안하면 자동으로 delete 되게 만들었을 경우에도 요긴하게 쓸 수 있을것 같다. <br>
<br>
<br>
### 초기화 리스트

<br>
<br>
초기화 리스트는 다음과 같다. <br>
```c++
Bar(): i(10)
{}
```
생성자 뒤에 : 붙이고 쓰는놈 이다. <br>
인터넷을 찾아봤는데 성능 차이라는 말이 있고 해서 어셈을 직접 까보기로 했다. <br>
일단 기본적인 생성자는 <br>
```c++
Bar(int n) {
~스택정리~
mov   eax, dword ptr [this]
mov   ecx, dword ptr [n]
mov   dword ptr [eax], ecx
}
```
ecx에 매개변수로 받은 값을 mov 후 eax에 있는 자기 자신의 변수에 값을 대입한다. <br>
```c++
Bar(int n) : num(n) {
~스택정리~
mov   eax, dword ptr [this]
mov   ecx, dword ptr [n]
mov   dword ptr [eax], ecx
}
```
별 차이 없는거 같다. <br>
혹시나 해서 스택정리 과정을 비교해봤는데 똑같다. <br>
```c++
00F92140  push        ebp  
00F92141  mov         ebp,esp  
00F92143  sub         esp,0CCh  
00F92149  push        ebx  
00F9214A  push        esi  
00F9214B  push        edi  
00F9214C  push        ecx  
00F9214D  lea         edi,[ebp-0Ch]  
00F92150  mov         ecx,3  
00F92155  mov         eax,0CCCCCCCCh  
00F9215A  rep stos    dword ptr es:[edi]  
00F9215C  pop         ecx  
00F9215D  mov         dword ptr [this],ecx  
00F92160  mov         ecx,offset _344D0F4F_ConsoleApplication1@cpp (0F9F02Ah)  
00F92165  call        @__CheckForDebuggerJustMyCode@4 (0F91389h)  
00F9216A  mov         eax,dword ptr [this]  
00F9216D  mov         ecx,dword ptr [n]  
00F92170  mov         dword ptr [eax],ecx  
```
<br>
그래서 좀 더 찾아보니, class 내부 멤버 변수에 const 변수가 있을 때 초기화가 가능하다는걸 찾았다.. 너무 늦게.. <br>
찾고 나서 cppreference 사이트를 확인해봤다. <br>
```c++
class Bar {
  int num = 10;
public:
  Bar() : num(20) {
  }
}
```
같은 경우에 num은 20으로 설정된다고 한다. <br>
그리고 다른 생성자를 호출하는데 사용 할 수 있다. <br>
```c++
class Bar {
  int num;
public:
  Bar() : Bar(10) {
  }
  Bar(int i) : num(i) {
  }
}
```
생성자 리스트의 용도는 이정도 인거 같다.. <br>
<br>
공부 하다 보면.. c++ 98부터 다시해야하나... 아니 c++ 자체를 잘 아는건가.. 사실 c에 class만 붙여두고 c++이라고 생각한거 아닐까.. 싶다.. <br>
따흑..<br>
<br>
<br>
