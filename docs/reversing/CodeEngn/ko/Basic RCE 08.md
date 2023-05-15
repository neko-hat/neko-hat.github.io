---
layout: post
title: Basic RCE 08
parent: CodeEngn
grand_parent: Reversing
nav_order: 6
---

# Basic RCE 08

![image](/assets/images/CodeEngn/Basic RCE 08/Basic RCE 08.png)

OEP를 구해보자.

![image](/assets/images/CodeEngn/Basic RCE 08/Basic RCE 081.png)

해당 파일은 upx로 패킹되어있다. 따라서 unpack 한 후 OEP를 구해보자.

![image](/assets/images/CodeEngn/Basic RCE 08/Basic RCE 082.png)

unpack이 완료되었다. 이제 디버거에서 실행하여 확인해 보자.

![image](/assets/images/CodeEngn/Basic RCE 08/Basic RCE 083.png)

OEP는 0x01012475이다.

![image](/assets/images/CodeEngn/Basic RCE 08/Basic RCE 084.png)

![image](/assets/images/CodeEngn/Basic RCE 08/Basic RCE 085.png)

문제를 해결하였다.