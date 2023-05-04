---
layout: default
title: Basic RCE 04
parent: CodeEngn
grand_parent: Reversing
nav_order: 3
---

# Basic RCE 04

![image](/assets/images/CodeEngn/Basic RCE 04/Basic RCE 04.png)

디버거에서 실행하지 않아서, 그냥 실행하면 다음처럼 정상 문구를 반복적으로 출력하는 콘솔 앱으로 보인다.

![image](/assets/images/CodeEngn/Basic RCE 04/Basic RCE 041.png)

디버거에서 실행하면, 다음처럼 디버깅 당함 문구를 반복적으로 출력한다. 이제 디버깅 중인지 판단하는 함수를 찾아보자.

![image](/assets/images/CodeEngn/Basic RCE 04/Basic RCE 042.png)

문자열을 따라가면, 다음처럼 ISDebuggerPresent함수가 보인다. 해당 함수가 디버깅 여부를 판단하는 함수이다. 로직을 알아보기 위해, 해당 함수 내부로 들어가보자.

![image](/assets/images/CodeEngn/Basic RCE 04/Basic RCE 043.png)

다음처럼, kernelbase.dll 즉, 커널에서 디버깅 여부를 판단 하는 것으로 보인다.

![image](/assets/images/CodeEngn/Basic RCE 04/Basic RCE 044.png)

위처럼, 패치를 한다면, 디버깅 중에도 정상 문구를 출력하게 할 수 있다.

![image](/assets/images/CodeEngn/Basic RCE 04/Basic RCE 045.png)

![image](/assets/images/CodeEngn/Basic RCE 04/Basic RCE 046.png)

![image](/assets/images/CodeEngn/Basic RCE 04/Basic RCE 047.png)

정답은 찾았다.