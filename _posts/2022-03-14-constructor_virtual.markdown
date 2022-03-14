---
layout: post
title:  "생성자와 가상 함수"
date:   2022-03-14 16:20:59 +0900
categories: cpp
---

<br>
<br>
가상 함수에 대해서 조금 몇가지 더 얘기를 해볼까 한다. <br>
<br>
<br>
생성자에서 가상 함수 콜을 하면 어떻게 될까? <br>
<br>
생성자에서 가상 함수 콜을 할 경우,<br>
<br>
자기 자신의 가상 함수가 호출된다. (?) <br>
여기서 문제가 생긴다.<br>
parent class와 child class가 있다고 했을때, <br>
```c++
class parent {
public:
  parent() {
    foo();
  }
  
  virtual void foo() {
    cout << "parent foo"<< endl;
  }
}


class child : public parent {
public:
  child() {
    foo();
  }
  
  virtual void foo() {
    cout << "child foo"<< endl;
  }
}

```
가 있다고 해보자. <br>
<br>
child 객체를 생성하면 생성자는 단순히 부모 -> 자식 순서로 호출되는게 아니다. <br>
우선 child의 생성자를 호출해서 child 생성자의 내부 작동을 하기 전에 parent의 생성자를 호출한다. <br>
호출한 부분의 asm 코드는 다음과 비슷하다. <br>
```c++
mov   ecx, dword ptr [this]
call  parent::parent(주소)
mov   eax, dword ptr [this]
mov   dword ptr [eax], offset child::`vftable`(주소)
```
<br>
부모 생성자를 호출 한 후, 가상함수 테이블이 세팅된다. <br>
이래서 생성자에서 가상 함수를 호출 할 경우 항상 자기자신의 가상함수가 호출하게 된다. <br>
순수 가상함수를 쓰다가 오류가 떠서 작성하게됬다.. <br>
<br>
<br>
<br>






