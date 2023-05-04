---
layout: default
title: Basic RCE 14
parent: CodeEngn
grand_parent: Reversing
nav_order: 12
---

# Basic RCE 14

![image](/assets/images/CodeEngn/Basic RCE 14/Basic RCE 14.png)

bruteforce 즉, 무작위 대입 공격이 필요하다....

![image](/assets/images/CodeEngn/Basic RCE 14/Basic RCE 141.png)

![image](/assets/images/CodeEngn/Basic RCE 14/Basic RCE 142.png)

unpack 후 디버거에서 실행해 보자.

![image](/assets/images/CodeEngn/Basic RCE 14/Basic RCE 143.png)

![image](/assets/images/CodeEngn/Basic RCE 14/Basic RCE 144.png)

![image](/assets/images/CodeEngn/Basic RCE 14/Basic RCE 145.png)

분기문을 찾고 탐색해보자.

![image](/assets/images/CodeEngn/Basic RCE 14/Basic RCE 146.png)

다음처럼 입력 후 실행 ㄱㄱ

![image](/assets/images/CodeEngn/Basic RCE 14/Basic RCE 147.png)

해당 부분이 입력된 CodeEngn즉 name으로 serial value를 만드는 부분이다.

![image](/assets/images/CodeEngn/Basic RCE 14/Basic RCE 148.png)

해당 call 부분이 비교하는 함수로 보인다. 내부로 진입해보자.

![image](/assets/images/CodeEngn/Basic RCE 14/Basic RCE 149.png)

해당 함수 내부에선 입력한 serial을 토대로 다시 값을 변형하여 ebx에 저장한다 해당 로직은 한문자마다 A를 곱하여 이루어진다. 이 값이 CodeEngn으로 만들어진 값 0x129A1과 일치해야 한다.

![image](/assets/images/CodeEngn/Basic RCE 14/Basic RCE 1410.png)

해당 부분을 10진수로 바꾸면 답이다. 하지만 brute force를 위해 username을 입력 받고, 로직에 맞게 시리얼 값을 만들어 찾는 프로그램을 만들어 보자.

![image](/assets/images/CodeEngn/Basic RCE 14/Basic RCE 1411.png)

만들었고 성공하였다. 해당 실행 파일, 소스코드는 맨 아래의 파일을 참고바란다.

![image](/assets/images/CodeEngn/Basic RCE 14/Basic RCE 1412.png)

![image](/assets/images/CodeEngn/Basic RCE 14/Basic RCE 1413.png)

해결되었다.

![image](/assets/images/CodeEngn/Basic RCE 14/Basic RCE 1414.png)

![image](/assets/images/CodeEngn/Basic RCE 14/Basic RCE 1415.png)

프로그램은 이런식으로 사용 가능하다.

[BruteForce for Basic RCE 14.zip](/assets/files/CodeEngn/BruteForce_for_Basic_RCE_14.zip)

![image](/assets/images/CodeEngn/Basic RCE 14/Basic RCE 1416.png)

[BruteForce for Basic RCE 14_v2.zip](/assets/files/CodeEngn/BruteForce_for_Basic_RCE_14_v2.zip)

제한 모두 푼 프로그램.

위와 같이 사용 가능하다.