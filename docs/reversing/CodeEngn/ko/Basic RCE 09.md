---
layout: post
title: Basic RCE 09
tags: [reversing, Study]
parent: CodeEngn
grand_parent: Reversing
nav_order: 7
---

# Basic RCE 09

![image](/assets/images/CodeEngn/Basic RCE 09/Basic RCE 09.png)

stolen byte는 프로그램의 코드 중에서  패커가 위치를 이동시킨 코드를 말한다.

할당된 메모리 공간으로 이동되어 실행되고 때문에 이를 복구하지 못하고 덤프하게 되면 정상적으로 작동하지 못한다.

일종의 안티 디버깅 기법 중 하나라 할 수 있다.

![image](/assets/images/CodeEngn/Basic RCE 09/Basic RCE 091.png)

해당 문제에 대한 설명은 → [https://www.notion.so/Abexcm3-66760e14374949f681a0b239a931ce51](https://www.notion.so/Abexcm3-66760e14374949f681a0b239a931ce51)

해당 파일을 디버거에서 실행하면 다음과 같다

![image](/assets/images/CodeEngn/Basic RCE 09/Basic RCE 092.png)

upx 패킹된 것으로 보인다. 따라서 unpack을 하고 진행해보자.

![image](/assets/images/CodeEngn/Basic RCE 09/Basic RCE 093.png)

unpack이 완료되었다. 다시 디버거에서 실행해보자

![image](/assets/images/CodeEngn/Basic RCE 09/Basic RCE 094.png)

stolen byte의 영향으로 코드가 이동되어 정상적으로 작동이 되지 않는 모습을 보인다.

![image](/assets/images/CodeEngn/Basic RCE 09/Basic RCE 095.png)

원본을 디버거 상에서 unpack하여 OEP를 구한 후, 분석을 다시 진행 해 보자. 따라서 POPAD에 BP를 걸고 진행해보자

![image](/assets/images/CodeEngn/Basic RCE 09/Basic RCE 096.png)

![image](/assets/images/CodeEngn/Basic RCE 09/Basic RCE 097.png)

call 내부로 들어가 보니, 해당 함수는 MessageBoxA 함수임을 알 수있다.

하지만, MessageBox 호출 이전 함수의 매개변수로 0만을 넣어 준 것을 볼 수 있는데, MessageBoxA의 함수 인자는 hwnd, lpText, lpCation, uType으로 총 4개가 필요하다. 따라서 OEP에서 없는 3개의 인자들이 미리 PUSH된 stolen byte인 것이다.

![image](/assets/images/CodeEngn/Basic RCE 09/Basic RCE 098.png)

unpack했을 때와 마찬가지로, 위에 NOP명령어로 채워진 것을 볼 수 있다.

![image](/assets/images/CodeEngn/Basic RCE 09/Basic RCE 099.png)

다시, POPAD 명령 밑에 빠진 3개의 인자를 push하는 것을 볼 수 있는데, 해당 부분이 stolen byte이다.(6A0068002040006812204000)

![image](/assets/images/CodeEngn/Basic RCE 09/Basic RCE 0910.png)

![image](/assets/images/CodeEngn/Basic RCE 09/Basic RCE 0911.png)

문제를 해결하였다.