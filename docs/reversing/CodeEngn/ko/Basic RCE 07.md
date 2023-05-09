---
layout: default
title: Basic RCE 07
parent: CodeEngn
grand_parent: Reversing
nav_order: 5
---

# Basic RCE 07

![image](/assets/images/CodeEngn/Basic RCE 07/Basic RCE 07.png)

문제를 해결하기 위해, 다음과 같이 디버거에서 실행해 보았다.(해당 문제 분석과 풀이는 → [Abexcm05]())

![image](/assets/images/CodeEngn/Basic RCE 07/Basic RCE 071.png)

다음처럼 문자열 비교 함수, strcmp에 BP를 걸어두었다.

![image](/assets/images/CodeEngn/Basic RCE 07/Basic RCE 072.png)

serial value가 보인다. 해당 부분이 어떻게 만들어 지는지를 분석하기 위해 다음처럼 BP를 걸었다.

![image](/assets/images/CodeEngn/Basic RCE 07/Basic RCE 073.png)

serial 값이 시스템 정보를 읽어서 규칙에 따라 만들어 지기 때문에, 다시 실행하여 serial이 만들어 지는 과정을 살펴보자.

![image](/assets/images/CodeEngn/Basic RCE 07/Basic RCE 074.png)

GetVolumeInformationA 함수를 통해, C드라이브 이름을 가져오고, 이를 아래의 반복문을 통해 시리얼 값으로 변경해주고 있다

![image](/assets/images/CodeEngn/Basic RCE 07/Basic RCE 075.png)

위는 반복문을 한번 실행한 것으로, 문자열 처음부터 4총 4개가 하나씩 증가하고 있다.

2반 반복되는 과정으로, c드라이브 이름이 CodeEngn일 경우 EqfgEngn으로 바뀔 것이다!

![image](/assets/images/CodeEngn/Basic RCE 07/Basic RCE 076.png)

![image](/assets/images/CodeEngn/Basic RCE 07/Basic RCE 077.png)

문제 해결에 성공하였다.
