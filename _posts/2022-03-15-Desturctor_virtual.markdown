---
layout: post
title:  "소멸자와 가상 함수"
date:   2022-03-15 10:36:59 +0900
categories: cpp
---

<br>
<br>
소멸자와 가상 함수에 대해서 이번에는 적어보겠다. <br>
마찬가지로 이전 글과 동일하게..<br>
생성자와 소멸자에서 순수 가상 함수를 호출 하다가 문제가 있다는걸 알게 됬다. <br>
pure function call 오류라니.. <br>
소스는 비슷하게 간단하다. <br>
부모 - 자식 간의 상속이 있고 똑같이 foo 가상 함수를 소멸자에서 호출 해준다.<br>
당연히 소멸자에는 virtual 키워드를 붙였다. <br>
<br>
메인 함수에서 retun 0이 호출되는 순간 자식의 소멸자가 호출된다. <br>
자식의 소멸자가 호출되면, 소멸자 내부의 함수 본문(?) 이 실행 되고, 부모의 소멸자가 호출된다. <br>
부모의 소멸자 내부에서 foo가 호출 되는 시점에는 이미 this의 가상함수 테이블에는 자식::foo() 가 아니라 <br>
부모::foo() 가 들어가있다. <br>
가상 함수 세팅 되는 순서를 거꾸로 생각해보면 쉬울거 같다. <br>
위에서부터 virtual 함수를 세팅하면서 내려오는것 처럼, 아래서부터 소멸자가 호출되면서 해당 가상 함수가 없어진다. <br>
이게 객채의 생성 시점에 가상함수 테이블이 세팅된다 라는 말의 뜻인거 같다. <br>
<br>
<br>
아무튼.. <br>
생성자 / 소멸자 에서 순수 가상 함수를 호출하면 안된다. <br>
생성자 / 소멸자에서 호출 한 가상 함수는 해당 시점의 가상함수 테이블을 따라간다. <br>
<br>
<br>
앞으로 틀리지 말고.. <br>
주의해서 코딩하도록 하자... <br>
<br>
<br>











