---
layout: post
title: Basic RCE 15
tags: [reversing, Study]
parent: CodeEngn
grand_parent: Reversing
nav_order: 13
lang: ko
lang-ref: CodeEngn - Basic RCE 15
---

# Basic RCE 15

![image](/assets/images/CodeEngn/Basic RCE 15/Basic RCE 15.png)

![image](/assets/images/CodeEngn/Basic RCE 15/Basic RCE 151.png)

![image](/assets/images/CodeEngn/Basic RCE 15/Basic RCE 152.png)

그냥 실행하면 위와 같이 나온다. Name에 따라 어떤 과정을 거쳐 그에 맞는 Serial값이 만들어 지고, 그 값을 찾으면 되는 것으로 예상된다.

디버거에서 실행 해 보았다.

![image](/assets/images/CodeEngn/Basic RCE 15/Basic RCE 153.png)

호출 구역에서, 많은 문자열 관련 함수들이 보인다. 그 중에 문자열 비교함수가 있어, 해당 부분에 BP를 걸고 문제를 진행 해 보겠다.

![image](/assets/images/CodeEngn/Basic RCE 15/Basic RCE 154.png)

위처럼 입력하고 실행해보자.

![image](/assets/images/CodeEngn/Basic RCE 15/Basic RCE 155.png)

문자열 비교 전, 시리얼 값은 정수고, 정수인지 확인 하는 부분이 있을 것이라는 예상이 되는 MessageBox가 나왔다.

시리얼 값 비교는 문자열 비교 함수가 아닌, 정수 처리 부분일 것이므로, 시리얼 값을 만드는 곳을 다시 찾아 BP를 걸어야 될 것 같다. 다시 모듈간 호출을 살펴보자.

![image](/assets/images/CodeEngn/Basic RCE 15/Basic RCE 156.png)

실행 결과, 해당 부분은 serial값을 만드는 부분이 아니였다.

![image](/assets/images/CodeEngn/Basic RCE 15/Basic RCE 157.png)

따라서, 잘못 된 시리얼 값을 입력 했을 때 출력되는 문자열을 찾아, 분기문을 탐색해 보았다.

![image](/assets/images/CodeEngn/Basic RCE 15/Basic RCE 158.png)

쉽게 분기점을 찾아 볼 수 있었는데, 시리얼 값을 만드는 과정을 알아보기 위해, 다시 분기문 이전의 함수들을 따라가보며 로직을 탐색해 보자.

![image](/assets/images/CodeEngn/Basic RCE 15/Basic RCE 159.png)

![image](/assets/images/CodeEngn/Basic RCE 15/Basic RCE 1510.png)

시리얼 값을 만드는 부분을 찾았다. 해당 로직은 다음과 같다.

```csharp
private int makeSerial(string name)
{
	int key = 0;
	int len = name.Length;
	foreach(char c in name)
	{
		key += c << 3;
	}	
	key += len << 3;
	key = key << 2;

	return key;
}
```

해당 로직을 통해 다음처럼 name이 CodeEngn일 경우의 serial값을 확인해 보자. 

```csharp
namespace Basic_RCE_15_Serial_Logic
{
    public class Serial_Logic
    {
        private static void Main()
        {
			string? name = "";
			int? key;
			Console.WriteLine("Input nane to make seral...");
			name = Console.ReadLine();

            try
            {
				if (name != null)
				{
					key = makeSerial(name);
					Console.WriteLine("Serial Val : {0}", key);
                    Console.WriteLine("Press Enter to EXIT Program...");
					Console.Read();
					return;
				}

            }
			catch (Exception ex)
            {
                Console.WriteLine(ex.Message);
                Console.WriteLine("Program Error! try again...");
                Console.WriteLine("Press Enter to EXIT Program");
				Console.Read();
				return;
            }
        }

		private static int makeSerial(string name)
		{
			int key = 0;
			int len = name.Length;
			foreach (char c in name)
			{
				key += c << 3;
			}
			key += len << 3;
			key = key << 2;

			return key;
		}
	}
}
```

![image](/assets/images/CodeEngn/Basic RCE 15/Basic RCE 1511.png)

![image](/assets/images/CodeEngn/Basic RCE 15/Basic RCE 1512.png)

문제를 해결하였으며, 해당 프로그램이 곧 name별 Key Gen 프로그램이다.

![image](/assets/images/CodeEngn/Basic RCE 15/Basic RCE 1513.png)

![image](/assets/images/CodeEngn/Basic RCE 15/Basic RCE 1514.png)

모두 완료되었다.