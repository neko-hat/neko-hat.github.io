---
layout: post
title: Basic RCE 11
parent: CodeEngn
grand_parent: Reversing
nav_order: 9
---

# Basic RCE 11

![image](/assets/images/CodeEngn/Basic RCE 11/Basic RCE 11.png)

![image](/assets/images/CodeEngn/Basic RCE 11/Basic RCE 111.png)

해당 프로그램이 UPX로 패킹 된 것을 확인하였다. 일단 디버거로 바로 실행해보자.

![image](/assets/images/CodeEngn/Basic RCE 11/Basic RCE 112.png)

![image](/assets/images/CodeEngn/Basic RCE 11/Basic RCE 113.png)

OEP 부터 아래 13바이트가 모두 NOP으로 채워진 것을 확인 할 수 있다.  call 40109F를 따라가다 보면 MessageBoxA함수를 호출 하는 것을 알 수 있는데, 해당 부분의 매개변수가 0 하나뿐이므로, 나머지 3개가 미리 PUSH된 stolen byte임을 알 수 있다.

![image](/assets/images/CodeEngn/Basic RCE 11/Basic RCE 114.png)

?? 9번이랑 중복같은데

![image](/assets/images/CodeEngn/Basic RCE 11/Basic RCE 115.png)

![image](/assets/images/CodeEngn/Basic RCE 11/Basic RCE 116.png)

...