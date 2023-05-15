---
layout: post
title: Basic RCE 13
tags: [reversing, Study]
parent: CodeEngn
grand_parent: Reversing
nav_order: 11
lang: ko
lang-ref: CodeEngn - Basic RCE 13
---

# Basic RCE 13

![image](/assets/images/CodeEngn/Basic RCE 13/Basic RCE 13.png)

![image](/assets/images/CodeEngn/Basic RCE 13/Basic RCE 131.png)

password를 찾아보자

![image](/assets/images/CodeEngn/Basic RCE 13/Basic RCE 132.png)

프로그램을 실행하면 다음처럼 ntdll모듈에서 멈춘다. 프로그램을 실행 후 pause 명령어를 입력한 후 password를 입력하여 디버깅을 시작해보자.

![image](/assets/images/CodeEngn/Basic RCE 13/Basic RCE 133.png)

![image](/assets/images/CodeEngn/Basic RCE 13/Basic RCE 134.png)

R13 레지스터에 다음처럼 입력한 값이 저장 된 것을 볼 수 있다.

![image](/assets/images/CodeEngn/Basic RCE 13/Basic RCE 135.png)

step into 명령어를 통해 진행하다 보면 mscorwks모듈에서 다음처럼 입력한 명령어를 push하는 부분이 나온다. 여기를 집중적으로 확인해 보자.

확인 해본  결과 password 체크 부분은 나오지 않았다.

![image](/assets/images/CodeEngn/Basic RCE 13/Basic RCE 136.png)

![image](/assets/images/CodeEngn/Basic RCE 13/Basic RCE 137.png)

다시 진행하다 보니 mscorlib.ni모듈에서 입력한 값을 비교하는 부분을 찾았다. 문자 하나하나 비교하고 있는 반복문으로, password 비교구문으로 예상된다.

진행하다보니 해당 부분도 분기문은 아니였다.

![image](/assets/images/CodeEngn/Basic RCE 13/Basic RCE 138.png)

분기문을 찾았다. 해당 부분이 printf함수로 추측되며, 인자로 Bad Luck...부분이 출력되는 부분이다.

![image](/assets/images/CodeEngn/Basic RCE 13/Basic RCE 139.png)

그 전의 call 부분으로 들어가 살펴보자.

![image](/assets/images/CodeEngn/Basic RCE 13/Basic RCE 1310.png)

수상한 문자열이 보인다. 하지만 이대로 진행하면 해당 문자열 이 보이는 부분을 점프하기 때문에 천천히 확인해 보자.

![image](/assets/images/CodeEngn/Basic RCE 13/Basic RCE 1311.png)

je문을 다음처럼 패치하였다.

![image](/assets/images/CodeEngn/Basic RCE 13/Basic RCE 1312.png)

![image](/assets/images/CodeEngn/Basic RCE 13/Basic RCE 1313.png)

password를 찾았다. Leteminman이 password다.

![image](/assets/images/CodeEngn/Basic RCE 13/Basic RCE 1314.png)

![image](/assets/images/CodeEngn/Basic RCE 13/Basic RCE 1315.png)

히히ㅣ힣힣히힣히ㅣㅎ