---
layout: post
title: Basic RCE 03
tags: [reversing, Study]
parent: CodeEngn
grand_parent: Reversing
nav_order: 2
lang: ko
lang-ref: CodeEngn - Basic RCE 03
---

# Basic RCE 03

프로그램을 그냥 실행해 보았다.

![image](/assets/images/CodeEngn/Basic RCE 03/Basic RCE 03.png)

오류를 해결하고 크랙을 시작해야겠다....

해당 문제는 VB Runtime을 새로 설치해서 해결 할 수 있다.

![image](/assets/images/CodeEngn/Basic RCE 03/Basic RCE 031.png)

프로그램이 정상적으로 실행 되었다.

근데 메세지 박스의 문구가 독일어다. 해석하면, Nag를 제거하거나, 올바른 password를 구하라고 나온다.

![image](/assets/images/CodeEngn/Basic RCE 03/Basic RCE 032.png)

이제 두가지 방법으로 문제를 해결해보자.

# 1. Password 구하기

디버거에서 프로그램을 실행 후, 문자열 구역을 살펴보자.

![image](/assets/images/CodeEngn/Basic RCE 03/Basic RCE 033.png)

패스워드가 틀렸다고 띄워주는 메세지 박스 문구를 타고 가면 다음과 같다.

![image](/assets/images/CodeEngn/Basic RCE 03/Basic RCE 034.png)

해당 부분에 문자열 비교 함수  vbaStrCmp가 있다. 이 함수에다가 BP를 걸고 패스워드로 예상되는 "2G83G35Hs2”를 입력해보자.

![image](/assets/images/CodeEngn/Basic RCE 03/Basic RCE 035.png)

입력 후, BP에서 멈췄다. Step over를 통해 하나씩 실행해보자.

![image](/assets/images/CodeEngn/Basic RCE 03/Basic RCE 036.png)

패스워드가 맞다... 크랙에 성공했다.

# 2. Nag 지우기(패치)

![image](/assets/images/CodeEngn/Basic RCE 03/Basic RCE 037.png)

Massage Box 호출 부분에 BP를 걸고 실행하였다.

![image](/assets/images/CodeEngn/Basic RCE 03/Basic RCE 038.png)

![image](/assets/images/CodeEngn/Basic RCE 03/Basic RCE 039.png)

해당 부분에서 메세지 박스가 호출 된다. 이 부분을 지워주자.

해당 메세지 박스가 호출 된 후, 확인 버튼을 눌렀을 때 레지스터 값들은 다음처럼 변한다.

![image](/assets/images/CodeEngn/Basic RCE 03/Basic RCE 0310.png)

![image](/assets/images/CodeEngn/Basic RCE 03/Basic RCE 0311.png)

메세지 박스 호출 부분을 다음처럼 바꾸어, 메세지 박스를 지우자.

![image](/assets/images/CodeEngn/Basic RCE 03/Basic RCE 0312.png)

해당 패스워드 분기 부분의 je 부분을 nop으로 바꿔주자.

![image](/assets/images/CodeEngn/Basic RCE 03/Basic RCE 0313.png)

![image](/assets/images/CodeEngn/Basic RCE 03/Basic RCE 0314.png)

![image](/assets/images/CodeEngn/Basic RCE 03/Basic RCE 0315.png)

크랙이 완료되었다.

[03_patched2.zip](Basic%20RCE%2003%20d361a3e914944459ae52f208ae4efd83/03_patched2.zip)

![image](/assets/images/CodeEngn/Basic RCE 03/Basic RCE 0316.png)

![image](/assets/images/CodeEngn/Basic RCE 03/Basic RCE 0317.png)

codeengn에서 요구하는 답을 찾아 등록하였다.