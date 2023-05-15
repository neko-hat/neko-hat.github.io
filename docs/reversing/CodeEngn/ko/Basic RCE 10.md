---
layout: post
title: Basic RCE 10
parent: CodeEngn
grand_parent: Reversing
nav_order: 8
---

# Basic RCE 10

![image](/assets/images/CodeEngn/Basic RCE 10/Basic RCE 10.png)

해당 파일을 디버거에서 실행해 보았다.

![image](/assets/images/CodeEngn/Basic RCE 10/Basic RCE 101.png)

UPX 패킹된 것으로 보인다.

![image](/assets/images/CodeEngn/Basic RCE 10/Basic RCE 102.png)

Exeinfo 에서 실행 해본 결과, Aspack으로 패킹된 것을 확인 할 수 있다. unpack을 진행해 보자.

# AspackDie로 unpack하기

![image](/assets/images/CodeEngn/Basic RCE 10/Basic RCE 103.png)

AsPack Die 1.4를 통해 Unpack 하였다.

![image](/assets/images/CodeEngn/Basic RCE 10/Basic RCE 104.png)

unpack이 성공적으로 되어 OEP가 00445834임을 구했다.

이후 문자열 검색으로 다음처럼 등록되었다는 문자열을 찾았다

![image](/assets/images/CodeEngn/Basic RCE 10/Basic RCE 105.png)

![image](/assets/images/CodeEngn/Basic RCE 10/Basic RCE 106.png)

위의 점프 구문이 분기점으로 보인다. 따라서 해당 명령 코드가 답이 될 것이다.

![image](/assets/images/CodeEngn/Basic RCE 10/Basic RCE 107.png)

![image](/assets/images/CodeEngn/Basic RCE 10/Basic RCE 108.png)

문제가 해결되었다.

# 디버거에서 unpack하기