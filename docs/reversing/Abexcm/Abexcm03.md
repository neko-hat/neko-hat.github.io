---
layout: post
title: Abexcm03
tags: [reversing, Study]
parent: Abexcm
grand_parent: Reversing
nav_order: 3
---

# Abexcm3

먼저 프로그램을 먼저 실행시켜 보자.

![image](/assets/images/Abexcm03/Abexcm03.png)

![image](/assets/images/Abexcm03/Abexcm031.png)

해당 메세지 박스 출력문을 보아 구조는 다음으로 예측 할 수 있다.

1. key file이 있는가?
2. 해당 key file이 옳은 key file인가?

즉, 2가지 방법의 패치로 해당 문제를 해결 할 수 있을 것이다.

해당 프로그램을 디스어셈 하면 다음과 같다.

![image](/assets/images/Abexcm03/Abexcm032.png)

CreateFileA함수 호출 이전 “abex.l2c”라는 문자열을 스택에 입력하는 걸로 보아, 해당 문자열은 해당 함수의 파라미터이며, 여기에서 존재 여부를 점검하는 파일 명임을 알 수 있다.

또한, 파일명만 존재함으로, 파일 경로는 프로그램 실행과 같은 위치임을 알 수 있다.

## key file 존재 점검에서 crack 패치

![image](/assets/images/Abexcm03/Abexcm033.png)

해당 함수 호출 이후, 분기문이 있다. 따라서 이 분기문을 조작해서 crack 패치를 해보면 다음과 같다.

![image](/assets/images/Abexcm03/Abexcm034.png)

![image](/assets/images/Abexcm03/Abexcm035.png)

패치는 완료되었으며, 실행해보면 다음과 같다.

![image](/assets/images/Abexcm03/Abexcm036.png)

![image](/assets/images/Abexcm03/Abexcm037.png)

## key file 점검 crack

우선 위에서 점검 대상의 key file명이 abex.12c임을 알았다. 이후에 GetFileSize함수 호출 후, 

cmp eax, 12를 통해 옳은 파일인지를 검사하는 걸로 보아, 파일 사이즈 16진수 12를 맞추기만 하면

옳은 key file로 인식 할 것으로 보인다.

![image](/assets/images/Abexcm03/Abexcm038.png)

파일을 만들었다.

이후 프로그램을 실행 해보면

![image](/assets/images/Abexcm03/Abexcm039.png)

파일 존재 여부 판단은 넘어 간 것으로 된다.

여기서 두가지 방법이 또 생긴다.

## key file을 옳은 파일로 인식하도록 crack

![image](/assets/images/Abexcm03/Abexcm0310.png)

해당 점검 부분에서, jne문을 사용 중이므로, cmp가 거짓일 때 작동한다.

따라서 항상 참이 되도록 해당 명령문을 수정해보자.

![image](/assets/images/Abexcm03/Abexcm0311.png)

이후 실행해보면 다음과 같다.

![image](/assets/images/Abexcm03/Abexcm0312.png)

## key file을 옳은 파일로 수정

해당 파일이 옳은 파일인지 검사 할 때, 사이즈를 사용했으므로, 0x12사이즈 만큼 키파일을 수정해보자.

0x12 = 18이므로, 18바이트만큼 파일을 수정해보자.

![image](/assets/images/Abexcm03/Abexcm0313.png)

a문자 18개 입력으로 18byte만큼의 파일을 만들었다.

이후 프로그램을 실행하면 다음과 같다.

![image](/assets/images/Abexcm03/Abexcm0314.png)

이로서, 3가지 방법으로 이 문제를 해결해 보았다.
