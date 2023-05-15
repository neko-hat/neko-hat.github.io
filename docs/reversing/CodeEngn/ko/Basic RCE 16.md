---
layout: post
title: Basic RCE 16
tags: [reversing, Study]
parent: CodeEngn
grand_parent: Reversing
nav_order: 14
---

# Basic RCE 16

![image](/assets/images/CodeEngn/Basic RCE 16/Basic RCE 16.png)

![image](/assets/images/CodeEngn/Basic RCE 16/Basic RCE 161.png)

이번엔 콘솔 프로그램이다. 15번 처럼 이름 입력 후, 이름에 맞는 Password를 생성하여 해당 값을 검사하는 것으로 예상된다.

![image](/assets/images/CodeEngn/Basic RCE 16/Basic RCE 162.png)

프로그램을 디버거에서 실행한 후 문자열 비교 함수가 있어, 해당 부분에 BP를 걸고 프로그램을 진행하여 보았다.

진행하여 보았지만, 해당 부분은 Password 비교 함수가 아니였다.

![image](/assets/images/CodeEngn/Basic RCE 16/Basic RCE 163.png)

다시 틀린 Password를 입력 했을 시 출력되는 문구를 찾아 분기문을 찾아보자.

![image](/assets/images/CodeEngn/Basic RCE 16/Basic RCE 164.png)

해당 부분은 어렵지 않게 찾아볼 수 있었다.

![image](/assets/images/CodeEngn/Basic RCE 16/Basic RCE 165.png)

또한 그 위에, 함수에서 name을 입력 받은 후,  입력 받은  name 문자열의 길이에 따라 다음의 코드처럼 암호화 된다.

# Password에 문자열 입력시

![image](/assets/images/CodeEngn/Basic RCE 16/Basic RCE 166.png)

추가적으로, Password에 문자열이 들어갈 경우, 다음처럼 EAX값에는 입력한 Password가 아닌, 포인터 주소로 저장되며, 항상 같은 주소를 가지고 있게 된다. 입력한 Password 문자열은 덤프에서 확인해 보았지만, 아래의 주소에 저장되어 있는 것을 확인하였다.

![image](/assets/images/CodeEngn/Basic RCE 16/Basic RCE 167.png)

![image](/assets/images/CodeEngn/Basic RCE 16/Basic RCE 168.png)

그리고 해당 주소를 참조하는 명령어는 확인하지 못했다. esp, ebp로 해당 주소를 참조하는 것으로 보이는데, 해당 값을 참조하는 것을 찾기위해 HW BP를 걸고 실행해 보았는데, kernel.dll애서 해당 값을push 하는 것이 확인 되었다(입력한 문자열 말고). 이후 확인은 너무 힘들어서 나중에 해보겠다.

```csharp
internal class keyGen
    {
        public DWORD genPassword(string name)
        {
            DWORD key = 0;
            DWORD len = (DWORD)name.Length;

            key = len * 3;
            key = key << 2;
            key *= key * key;
            key += 0x17;
            DWORD temp = (DWORD)(key * 0xACE80);

            key += temp;
            return key;
        }
    }
```

위의 코드대로 name을 Password로 암호화 하고 있다.

![image](/assets/images/CodeEngn/Basic RCE 16/Basic RCE 169.png)

성공하였다. 참고로 name문자열 길이로 암호화 하고 있기 때문에 name을 위처럼 하든 CodeEngn으로 하든 답은 같다.

![image](/assets/images/CodeEngn/Basic RCE 16/Basic RCE 1610.png)

![image](/assets/images/CodeEngn/Basic RCE 16/Basic RCE 1611.png)

해결되었다.