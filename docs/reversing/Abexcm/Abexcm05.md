---
layout: post
title: Abexcm05
tags: [reversing, Study]
parent: Abexcm
grand_parent: Reversing
nav_order: 5
lang: ko
lang-ref: Abexcm05
---

# Abexcm5

![image](/assets/images/Abexcm05/Abexcm05.png)

먼저, 프로그램을 그냥 실행시켜 보았다. 시리얼 값을 입력받아 Check버튼으로, 해당 시리얼 값을 검사하는 프로그램으로 보인다.

![image](/assets/images/Abexcm05/Abexcm051.png)

프로그램의 문자열 부분에서, 시리얼 값으로 예상되는 문자열을 발견했다. 해당 부분으로 넘어가보자.

![image](/assets/images/Abexcm05/Abexcm052.png)

프로그램을 실행시키고, 그대로 버튼을 눌러 진행을 해보니, 문자열 비교 함수 파라미터로 입력한 값과 비교하는 문자열이 있고, 분기점이 있는 것으로 보아, 해당 문자열이 시리얼 값으로 보인다. 이 값을 입력하여 실행시켜보자.

![image](/assets/images/Abexcm05/Abexcm053.png)

성공하였다. 이제 이 시리얼 값이 어떻게 생성되는지 알아보자.

![image](/assets/images/Abexcm05/Abexcm054.png)

모듈간 호출에서, 현재 정보를 불러오는 함수들을 발견하여, 해당 값으로 시리얼 값을 조합 하는 것으로 예상되어 해당 부분에 BP를 걸고 실행해 보았다.

![image](/assets/images/Abexcm05/Abexcm055.png)

해당 함수를 통해, 시리얼 값이 형성되는 것을 확인할 수 있다.

![image](/assets/images/Abexcm05/Abexcm056.png)

시리얼 값 중 Ykpfows부분은, OS정보 WIndows부분을, 반복문을 통해 +1을 하여 암호화 된 것임을 확인 할 수 있다.

진행해보면, strcat함수, strcopy함수를 통해, 암호화 된 문자열과 미리 저장된 문자열을 합쳐 시리얼 값을 만드는 것을 확인해 볼 수 있다.

이로써, 크랙이 완료되었다.
