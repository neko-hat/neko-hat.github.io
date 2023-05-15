---
layout: post
title: Basic RCE 05
parent: CodeEngn
grand_parent: Reversing
nav_order: 4
---

# Basic RCE 05

![image](/assets/images/CodeEngn/Basic RCE 05/Basic RCE 05.png)

프로그램을 실행하면 다음처럼 등록되지 않았다고 하고, 등록 버튼이 있다.

![image](/assets/images/CodeEngn/Basic RCE 05/Basic RCE 051.png)

이제 옳은 시리얼 넘버를 찾아보자.

![image](/assets/images/CodeEngn/Basic RCE 05/Basic RCE 052.png)

디버거에서 실행하였더니 pushad가 보인다. UPX 패킹 된 것으로 보인다.

Exeinfo PE에서 해당 프로그램을 분석해보자.

![image](/assets/images/CodeEngn/Basic RCE 05/Basic RCE 053.png)

역시 UPX 패킹되어있다. 해당 프로그램을 언패킹 한 후, 리버싱을 진행해보자.

![image](/assets/images/CodeEngn/Basic RCE 05/Basic RCE 054.png)

![image](/assets/images/CodeEngn/Basic RCE 05/Basic RCE 055.png)

언패킹이 완료되었다.

![image](/assets/images/CodeEngn/Basic RCE 05/Basic RCE 056.png)

정상적으로 코드가 보인다.

# 시리얼 찾기

![image](/assets/images/CodeEngn/Basic RCE 05/Basic RCE 057.png)

등록시킬 시리얼 문자열과 입력한 문자열을 비교해서 등록 할 것이라 예상하여, 문자열 비교 함수에 BP를 걸어두었다.

![image](/assets/images/CodeEngn/Basic RCE 05/Basic RCE 058.png)

실행 전, 문자열 검색하여, 분기 위치를 찾고, 혹시 정적으로 시리얼이 등록되어 있을 가능성을 염두하여 분기 주변을 탐색해보자.

![image](/assets/images/CodeEngn/Basic RCE 05/Basic RCE 059.png)

비교를 위해, 아무 값이나 넣고 실행해보았다.

![image](/assets/images/CodeEngn/Basic RCE 05/Basic RCE 0510.png)

시리얼 값을 비교 하기에 앞서, Unregistered...를 Registered User와 비교한 후 시리얼 값을 비교한다. 따라서 해당 부분을 Registered User로 바꾼 후 진행해보자.

![image](/assets/images/CodeEngn/Basic RCE 05/Basic RCE 0511.png)

해당 부분은 통과되었고, 시리얼 값 비교 부분이 나온다. GFX-754-IER-954 이 값이 시리얼 값이다.

![image](/assets/images/CodeEngn/Basic RCE 05/Basic RCE 0512.png)

크랙이 완료되었다.

![image](/assets/images/CodeEngn/Basic RCE 05/Basic RCE 0513.png)

![image](/assets/images/CodeEngn/Basic RCE 05/Basic RCE 0514.png)

모두 완료되었다.