---
layout: post
title:  "IO_PENDING"
date:   2022-04-04 11:03:59 +0900
categories: Windows Network
---

<br>
<br>
<br>
최근에 본 면접에서 IO_PENDING 예외처리에 대한 질문을 들었다. <br>
물논.. 대답을 못했기에 이 글을 적는다.. <br>
분명 배웠었는데.. <br>
<br>
<br>
IO_PEDING 예외는 Overlapped 모델에서 비동기 입출력 작업이 바로 완료되지 않으면 리턴되는 오류 코드다. <br>
작업이 아직 완료가 되지 않았음 이므로 오류는 아니다. <br>
뇌피셜을 돌려보면, <br>
ZeroByte recv 같은걸 걸어두면, 받긴 했지만 받는게 완전히 다 끝난게 아니니까 IO_PENDING 이 리턴되지 않을까 싶다. <br>
<br>
<br>
아무튼 오류지만 실제 작동은 오류로 인한 오류코드가 아닌 경우 이므로 예외처리를 꼭 해줘야 하는걸로.. <br>

```c++
WSAGetLastError() != WSA_IO_PENDING
```
이면 될거 같다. <br>
<br>






