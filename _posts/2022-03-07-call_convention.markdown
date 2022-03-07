---
layout: post
title:  "호출 규약"
date:   2022-03-07 16:47:59 +0900
categories: cpp Windows
---

이번 시간에는 함수 호출 규약에 대해서 정리를 해볼려고 한다. <br>
<br>
학교 다닐때 `__cdecl` 이 뭔지 궁금해서 학교 선배한테 물어봤지만 모른다는 대답이 돌아왔었던 적이 아직도 기억난다. <br>
그때는 왜 구글에 검색할 생각을 못했을까.. <br>
<br>
<br>
<br>
### cdecl 과 stdcall

<br>
`__cdecl` 과 `__stdcall` 둘다 x86 함수 호출 규약이다. <br>
두가지의 가장 큰 차이점은 함수의 스택 정리를 호출자가 하는지 피호출자가 하는지이다. <br>
`cdecl` 은 호출자가 정리하고, `stdcall` 은 피호출자가 정리한다. <br>
함수 호출의 기본값은 `cdecl` 이다. <br>
이는 가변 인자 함수를 지원한다. 예를 들면 `printf` 같은 <br>
조금 생각해보면 `stdcall` 아니 피호출자가 정리하는 호출 규약에서 가변인자 함수를 지원하지 못하는 이유를 알 수 있다. <br>
`cdecl` 에서는 스택의 크기를 알 수 있지만 ( 내부에서 정리하므로 ) `stdcall` 에서는 외부에서 정리하기 때문에 스택의 크기를 알 수가 없다. <br>
참고로 WINAPI 쓸때 자주 보이던 `CALLBACK` 매크로나 `WINAPI` 매크로 둘다 `stdcall` 이다. <br>
<br>
<br>
<br>
`thiscall` 같은것도 있지만, 이건 js 에서 `bind` 로 this를 같이 넘기는 Microsoft-specific 한 호출 규약이고, 피호출자가 정리한다. <br>
[__thiscall](https://docs.microsoft.com/en-us/cpp/cpp/thiscall?view=msvc-170)<br>
<br>
<br>
사실 공부목적으로 쓰는 글은 전부 면접관이 물어봤을때 질문한다는 내용으로 적고 있기는 한데.. <br>
이걸로 면접관이 만족할지에 대해서 생각하면 글쌔다 싶은 내용들이 많은거 같다. <br>
쓰다보면.. 실력이 늘겠지.. 언젠가.. <br>


