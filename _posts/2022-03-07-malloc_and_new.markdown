---
layout: post
title:  "malloc 과 new"
date:   2022-03-07 14:38:59 +0900
categories: cpp Windows
---

이번에는 메모리 할당에 대해서 공부해 볼려고 한다. <br>
일단 메모리 할당 방법은 크게 malloc 과 new 가 있다. <br>
두가지의 차이점을 간단히 요약해보면, <br>
<br>
1. malloc은 함수 new는 연산자.
2. new는 할당과 동시에 초기화가 가능 (생성자 호출 역시 가능)
3. malloc은 realloc을 통해 재할당이 가능
<br>
정도가 있는거 같다. <br>
<br>
공통점으로는 내부적으로 malloc과 new 둘다 `HeapAlloc` 를 호출한다. <br>
`HeapAlloc` 안에서는 `VirtualAlloc` 을 호출한다. <br>
그럼 순서대로 좀더 깊이 있게(?) 알아보도록 하자. <br>
<br>
<br>

### HeapAlloc

<br>
<br>
우선 `HeapCreate` 는 `VirtualAlloc` 을 통해 large block of virtual memory 를 reserve 한다. <br>
`HeapAlloc` 은 실제로 `VirtualAlloc` 을 통해서 할당 받는게 아니라 `HeapCreate` 를 통해서 reserve 된 메모리에서 요청 받은 메모리를 주는것이다. <br>
당연히 범위를 초과하면 실제로 할당 받는다.. <br>
<br>
<br>

### VirtualAlloc

<br>
<br>
`VirtualAlloc` 은 메모리(페이지)를 reserve, commit 혹은 상태를 변경(changes the state) 한다. <br>
할당 된 주소는 자동으로 페이지 단위로 정렬되고, 크기는 페이지 단위의 배수로 할당된다. <br>
<br>
<br>
<br>
<br>

### 다시 본론

<br>
<br>
다시 본론으로 돌아와서 살펴 보면 <br>
<br>
1. malloc, new 를 호출 한다
2. HeapAlloc을 호출한다.
2.1 힙이 부족하면 HeapCreate를 호출한다.
3. VirtualAlloc을 호출한다.
<br>
순으로 진행되는 것을 할 수 있다. <br>
<br>
<br>
그럼 `VirtualAlloc` 에서 나온 reserve, commit 등 에 대해서 알아보자. <br>

### 가상 메모리

<br>
<br>
당연하게도 컴퓨터는 모든 프로세스의 메모리를 물리 메모리에 올려두지 않는다. <br>
아무것도 하지 않아도 운영체제에서 돌아가는 프로세스는 무지하게 많고, <br>
그 프로세스들이 사용하는 메모리는 더 많아서 모든 메모리를 올려두면 메모리가 엄청나게 부족해지거나, <br>
이 세상의 모든 프로그래머들이 메모리를 엄청 꼼꼼하게 관리해야한다. <br>
그래서 가상 메모리를 사용한다. <br>
<br>
가상 메모리는 MMU에 의해서 관리되고, MMU는 페이지 테이블을 참조해 해당 페이지가 물리적 공간에 올라가 있지 않으면 물리적 메모리에 올린다. <br>
여기서 물리적 메모리에 페이지가 올라가 있는 상태가 commit 된 상태다. <br>
reserve는 할당은 받았으나, 물리 메모리에 올라가 있지 않은 상태다. <br>
프로그램이 동작하다가 commit 되지 않은 상태의 메모리에 접근하면 접근 실패가 되는데 이게 page fault(페이지 폴트) 다. <br>
<br>
갑자기 든 생각인데 예전에 면접보다가 page fault의 발생을 적게 시킬 수 있는 방법으로 뭐가 있을까? 라는 질문을 받은 적이 있는데 이제 알거 같다.<br>
1. 물리 메모리를 왕창 쓰기.
2. `VirtualAlloc` 을 통해서 메모리를 할당해 사용하는데 이때 commit 상태로 두기
<br>
정도 일거 같고 아래는 내 추측이다.. <br>
<br>
3. SSD나 HDD의 페이지 파일 사용 안하기(?)
<br>
<br>
페이지 폴트가 발생하면 페이지를 교체하는데 <br>
페이지 교체 알고리즘은 FIFO, LRU, 등.. 방법이 있다. <br>
<br>

### 페이지 교체 알고리즘

<br>
<br>
#### FIFO
<br>
생각하는 그거 맞다. <br>
먼저 들어온 페이지를 먼저 교체 <br>
<br>
#### LRU
<br>
Least Recently Uesd 이다. <br>
가장 오랫동안 참조되지 않은 페이지를 교체한다. <br>
마지막으로 참조된 시간 같은 카운터를 별도로 기억을 해야한다. <br>
<br>
#### LFU
<br>
Least Frequntly Uesd 이다. <br>
가장 참조 횟수가 적은 페이지를 교체한다. <br>
직전에 불러온 페이지를 교체할 수 있다.
<br>
<br>
<br>
정도만 알아두면 될거..같..고..?
<br>
<br>


### 또 다시 본론

<br>
또 다시 본론으로 돌아와서..<br>
malloc 과 new의 차이만 간단히 정리 할려고 했는데 어쩌다 보니 가상메모리 까지 가게 됬다. <br>
그럼 마지막으로 색다는 new에 대해서 알아보고 끝내자. <br>

### placement new

<br>
**생성자를 유일하게 다시 호출 하는 방법으로 알고 있다.** <br>
```c++
void* operator new(size_t, void* location);
```
Object Pool 만들때 사용했었다. <br>
이미 할당된 메모리를 반환 받아서 들고만 있다가 원하는 시점에 다시 받은 값으로 재 할당해서 나가기만 하면 되서.. <br>
<br>
<br>

### 마무리

<br>
점점 갈수록 내용이 정리가 안된거 같은데.. <br>
갈수록 머리속에 있는걸 글로 적기만 해서 그런거 같다. <br>
글 쓰는 연습이 안되서.. 더 잘쓰게 노력이나 해야겠다. <br>
<br>
<br>









