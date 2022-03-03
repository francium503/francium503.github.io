---
layout: post
title:  "Call By Value, Reference, Address"
date:   2022-03-03 15:04:59 +0900
categories: cpp
---

## Call By Value, Reference, Address
<br>
요즈음 블로그 포스팅을 하면서 C,C++ 언어에 대한 기초 지식(?) 및 운영체제, 네트워크 공부를 하다가 가슴으로는 알고 있는데 머리로는 정확히 설명을 하지 못할거 같은 3가지의 차이에 대해서 한번 적고 가야겠다고 생각했다. <br>
<br>
<br>
```c++
void CallByValue(int a) {
	cout << a << endl;
}

void CallByReference(int& b) {
	cout << b << endl;
}

void CallByAddress(int* c) {
	cout << *c << endl;
}


int main()
{
	int a = 10;
	int& b = a;
	int* c = &a;

	CallByValue(a);
  CallByValue(b);
  CallByValue(*c);

	CallByReference(a);
	CallByReference(b);
  CallByReference(*c);

	CallByAddress(&a);
	CallByAddress(&b);
	CallByAddress(c);

	return 0;	
}
```
매우 간단한(?) 짧은 소스지만 value 콜 reference 콜 address 콜 전부 들어있고 이 소스를 어셈으로 보면 어디가서 차이를 설명할 수 있지 않을까 싶어서 작성한 코드다. <br>
<br>
```c++
int a = 10;
mov   dword ptr [a], 0Ah
```
우선 int a = 10; 에는 당연하게도(?) 그냥 a에 10을 때려 박는다. <br>
```c++
int& b = a;
lea   eax [a]
mov   dword ptr [b], eax
```
int& b = a; 도 역시 a를 eax 레지스터에 저장한 뒤 b에 때려박는다. <br>
```c++
int* c = &a;
lea   eax,[a]
mov   dword ptr [c],eax
```
int* c = &a; 도 역시 eax 레지스터에 a를 때려박고 c에 넣는다. <br>
여기까지는 예상 범위 내였고.. <br>
<br>
Value 인 a를 사용한 각 함수 호출은
```c++
CallByValue(a);
mov   eax,dword ptr [a]
push  eax
call  CallByValue
add   esp,4

CallByReference(a);
lea   eax,[a]
push  eax
call  CallByRefernece
add   esp,4

CallByAddress(&a);
lea   eax,[a]
push  eax
call CallByAddress
add   esp,4
```
<br>
놀랍게 동작이 완전히 reference call 이랑 address call 이랑 똑같다. <br>
어셈 코드만 봤을때는 dword ptr [a]를 eax에 mov로 넣어서 전해주냐 lea로 해주냐 차이인데..<br>
mov는 mov a,b 일때 b의 값을 a로 옮기는거고..<br>
lea는 lea a,b 일때 b의 주소를 a로 옮기는걸로 알고 있다..<br>
계속해서 레퍼런스 변수 일때 동작을 확인 해보면 <br>
<br>
```c++
CallByValue(b);
mov   eax,dword ptr [b]
mov   ecx,dword ptr [eax]
push  ecx
call  CallByValue
add   esp,4

CallByReference(b);
mov   eax,dword ptr [b]
push  eax
call  CallByReference
add   esp,4

CallByAddress(&b);
mov   eax,dword ptr [b]
push  eax
call  CallByAddress
add   esp,4
```
<br>
Value 콜 일때는 ecx에 한번 더 eax를 넣고 함수 콜을 한다.. <br>
레퍼런스 변수인 b 말고 포인터 변수인 c를 넣고 호출할때도 완전히 똑같다.. <br>
<br>
<br>
함수 내부에서는 어떻게 동작하나.. <br>
우선 Value 일때
```c++
mov   eax, dword ptr [a]
push  eax

mov   eax, dword ptr [b]
mov   ecx, dword ptr [eax]
push  ecx

mov   eax, dword ptr [c]
mov   ecx, dword ptr [eax]
push  ecx

```
각각 Value, Reference, Address 순서이다. <br>
<br>
<br>
그렇다.. <br>
사실 레퍼런스랑 포인터랑 완전이 같게 동작한다. <br>
문법적인 차이를 제외하면 사실상 없는걸로.. <br>




