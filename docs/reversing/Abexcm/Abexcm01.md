---
layout: default
title: Abexcm01
parent: Abexcm
grand_parent: Reversing
nav_order: 1
---

# Abexcm01

![image](/assets/images/Abexcm01/Abexcm.png)

프로그램을 그냥 실행하면 HD를 CD-ROM으로 인식하도록 하라 한다. 

즉, 실행시킨 기기가 HD인지 CD-ROM인지 판단해서 메세지 박스를 띄우는 프로그램 인 것 같다.

![image](/assets/images/Abexcm01/Abexcm1.png)

프로그램의 내부 디스어셈코드이다. 프로그램이 시작되고, 메세지 박스를 출력 한 뒤, cmp, je명령어로 분기를 나누어 실행 된다. 따라서 해당 코드를 변형 시켜 CD-ROM 메세지 박스를 출력시켜보자.

나는 cmp문이 항상 참이 되도록, 해당 cmp문을 CMP EAX, EAX로 변형 시켰다.

![image](/assets/images/Abexcm01/Abexcm2.png)

해당 패치를 적용시키면, 항상 참이 되기 떄문에 je문이 실행되어 다음과 같이 문제가 해결된다.

![image](/assets/images/Abexcm01/Abexcm3.png)