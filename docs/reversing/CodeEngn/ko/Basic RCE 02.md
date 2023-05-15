---
layout: post
title: Basic RCE 02
parent: CodeEngn
grand_parent: Reversing
nav_order: 1
lang: ko
---

# Basic RCE 02

![image](/assets/images/CodeEngn/Basic RCE 02/Basic RCE 02.png)

프로그램을 그냥 실행시켜 보면, 다음처럼 실행이 되지 않는다. 관리자 권한으로 실행 시켜 보면, 다음처럼 실행되지 않는다.이

![image](/assets/images/CodeEngn/Basic RCE 02/Basic RCE 021.png)

어쩔 수 없이 바로 디버거로 파일을 실행 시켜 보았다.

![image](/assets/images/CodeEngn/Basic RCE 02/Basic RCE 022.png)

ollydbg에서도 파일을 실행 할 수 없다. 

그래서 PEView와 Hex Editor로 해당 파일을 열어 보았다.

![image](/assets/images/CodeEngn/Basic RCE 02/Basic RCE 023.png)

PEView에서는 DOS_HEADER밖에 나오지 않았고, 별다른 정보는 찾아 볼 수 없었다.

![image](/assets/images/CodeEngn/Basic RCE 02/Basic RCE 024.png)

Hex Editor에서, 패스워드 비교가 있을 수 있으니, text 섹션을 확인 해보자.

![image](/assets/images/CodeEngn/Basic RCE 02/Basic RCE 025.png)

패스워드는 JK3FJZh인 것으로 보인다...

![image](/assets/images/CodeEngn/Basic RCE 02/Basic RCE 026.png)

![image](/assets/images/CodeEngn/Basic RCE 02/Basic RCE 027.png)

허무하게 풀었다...