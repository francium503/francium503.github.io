---
layout: post
title:  "해쉬 테이블과 소수"
date:   2022-03-16 10:56:59 +0900
categories: 읽을거리
---

<br>
<br>
얼마전에 프라우드넷 서버 엔진을 만드신 배현직 사장님 블로그를 보다가.. <br>
이런 글을 봤다. <br>
<br>
<br>
["디자인패턴이나 C++17 새로운 기능 하나 더 아는 것보다, hash map에서 bucket 크기를 prime number로 잡고 key 값에 가블링 가능한 값 하나 더 넣어서 hash collision 줄이는 요령 아는 것이 더 값지다."](https://blog.naver.com/imays/221070373413)<br>
<br>
<br>
나름 오래된 글인데 자료구조의 내부에 대해서 잘 고민해본적이 없는거 같아서 찾아보기로 했다. <br>
<br>
좀더 오래된 글에서 예전 Java 내부의 hashmap에서 31을 선택한 내용에 대해서 나와 있었다. <br>
<br>
<br>
[Why do hash functions use prime numbers?](https://computinglife.wordpress.com/2008/11/20/why-do-hash-functions-use-prime-numbers/)<br>
<br>
<br>
결론은.. <br>
리만 가설이 증명되고 소수의 규칙이 밝혀지고나면 증명이 될거 같지만.. <br>
아무튼 버킷에 골고루 분포되서.. 인걸로.. <br>
<br>
<br>

