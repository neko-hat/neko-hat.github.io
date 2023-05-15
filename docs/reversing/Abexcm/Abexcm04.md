---
layout: post
title: Abexcm04
tags: [reversing, Study]
parent: Abexcm
grand_parent: Reversing
nav_order: 4
lang: ko
lang-ref: Abexcm04
---

# Abexcm4

먼저 그냥 프로그램을 실행해 보자.

![image](/assets/images/Abexcm04/Abexcm04.png)

여기 까지 보았을 때, 올바른 시리얼 값을 입력하는 문제임을 알 수 있다.

그런데, Registered버튼이 활성화 되어있지 않다. 따라서 문자를 입력할 때 마다 시리얼 값을 판단하는 것으로 예측 가능하다.

구조를 예측해 보면 다음과 같을 것이다.

1. 문자열 입력
2. 시리얼 값 판단
3. 버튼 활성화

![image](/assets/images/Abexcm04/Abexcm041.png)

확인을 위해, 모듈 호출 부분을 살펴보자.

아래에 문자열 비교 함수가 있기 때문에, 해당 모듈에 BP를 걸고 진행해 보았다.

![image](/assets/images/Abexcm04/Abexcm042.png)

문자 하나를 입력 할 때마다 해당 BP에서 멈추는 것으로 보아, 한글자 입력 받을 때 마다, 시리얼 값을 체크 하는 것으로 보인다. 비교 대상의 문자열이 시리얼 값일 것으로 예상되므로, 해상 함수 내부로 들어가 보자.

![image](/assets/images/Abexcm04/Abexcm043.png)

해당 함수 내부에서, 문자열 비교 함수의 인자로 2193870을 넘겨 주는 것으로 보아, 해당 값이 시리얼 값으로 예상된다. 이 값을 넣어 프로그램을 실행시켜 보자.

![image](/assets/images/Abexcm04/Abexcm044.png)

Registered버튼이 켜졌다. 

![image](/assets/images/Abexcm04/Abexcm045.png)

시리얼 값을 찾았다. 그럼 해당 시리얼 값은 어떻게 만들어 지는지 살펴 보자.

![image](/assets/images/Abexcm04/Abexcm046.png)

모듈간 호출에서, 현재 시간을 가져오는 함수들이 있는 것으로 보아, 해당 시리얼은 시간의 조합으로 예상된다. 해당 함수에 BP를 걸고 실행해보았다.

![image](/assets/images/Abexcm04/Abexcm047.png)

해당 함수들이 실헹되고, 시리얼 값이 조합되는 것을 확인 할 수 있다.
