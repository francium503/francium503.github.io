---
layout: post
title:  "map 과 unordered_map"
date:   2022-03-23 16:15:59 +0900
categories: 읽을거리
---

한동안 업데이트와 버그픽스로 회사일이 바빠서 포스팅을 못했다... <br>
이번에는 최근에 발견한 글중 또 재밌는 글을 찾아서 가져왔다. <br>
map과 unordered_map hashmap 의 속도에 대한 글이다. <br>
<br>
(https://gracefulprograming.tistory.com/3)[https://gracefulprograming.tistory.com/3] <br>
<br>
일반적으로 STL map 은 레드블랙 트리를 사용해서 자료를 저장한다고 알고 있고, <br>
unordered_map 은 해쉬 테이블 기반으로 알고 있다. <br>
<br>
레드블랙 트리는 밸런스드 트리의 일종으로 자료 삽입 삭제 할 때 노드 색을 맞추기 위한 회전을 계속 해야하고, <br>
최대 깊이가 2배까지 차이나고, 최악의 경우에 logN 이라고 알고 있다. <br>
자세한건 레드블랙트리 구글 검색 후 참고..<br>
<br>
<br>
근데 이제 문자열을 키로 사용 할 경우 map 과 unordered_map의 속도에 대해서 해쉬 성능과 길이, 키로 잡은 데이터의 분포에 따라서 <br>
map이 unordered_map 보다 빠를수 있다고 한다. <br>
<br>
근데 해쉬함수가 해당 블로그에 적힌것 처럼 앞 몇글자만 비교해서 해쉬를 하지는 않을거 같은데.. <br>
<br>
그래도 문자열 길이가 16일때 unordered_map이 다시 map과 근접한 성능을 내는 경우를 제외한다면, <br>
unordered_map이 필요할때 성능을 위해서 map을 사용 할 일은 없을것 같다. <br>
<br>
<br>


