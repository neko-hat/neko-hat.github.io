---
layout: post
title: Abexcm02
parent: Abexcm
grand_parent: Reversing
nav_order: 2
---

# Abexcm2

해당 프로그램을 그냥 실행시켜 보자.

![image](/assets/images/Abexcm02/Abexcm02.png)

이름과 시리얼을 입력해서 통과하는 프로그램으로 보인다.

나는 이름, 옳은 시리얼 값을 찾는 것을 목표로 두지 않고, check버튼을 눌렀을 때 바로 통과하도록 목표를 잡았다.

![image](/assets/images/Abexcm02/Abexcm021.png)

해당 프로그램의 디스어셈 코드다. 아래 코드에 main구조체 호출이 있는 것으로 보아, 해당 프로그램은 VB로 작성된 코드이다. 따라서 버튼 이벤트 코드를 찾아 문제를 해결하면 될 것으로 보인다.

![image](/assets/images/Abexcm02/Abexcm022.png)

사용된 문자열을 통해 분기점에 접근해보자.

문자열중에 이름 4글자 입력하라는 문자열이 있는 것으로 보아, 구조는 다음과 같을 것으로 예상된다.

1. Check버튼 이벤트 발생
2. 이름 문자열 길이 검사
3. 시리얼 검사

그러면 처음 이벤트 발생 할 떄인 문자열 길이 검사 부분을 찾기 위해 해당 에러 문구로 가보자.

![image](/assets/images/Abexcm02/Abexcm023.png)

해당 코드 위쪽에 이벤트 발생 함수가 있을 것으로 예상 되므로, 살펴보자.

![image](/assets/images/Abexcm02/Abexcm024.png)

위로 계속 올리다 보면 있다.

또한, 글자수 비교한 후 분기는 내려가다 보면 찾아 볼 수 있다.

![image](/assets/images/Abexcm02/Abexcm025.png)

해당 부분을 test문을 cmp eax, eax로 바꿔주고, je문의 주소를 옳은 시리얼 값을 입력했을 때 출력되는 메시지 박스 호출 문 전의 분기문 다음 주소로 바꿔주면 된다.

![image](/assets/images/Abexcm02/Abexcm026.png)

해당 목표 주소이다.

![image](/assets/images/Abexcm02/Abexcm027.png)

패치 이후 실행해 보면 다음과 같다.

![image](/assets/images/Abexcm02/Abexcm028.png)

이로써 문제는 해결되었다.