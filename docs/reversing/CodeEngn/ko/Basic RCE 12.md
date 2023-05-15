---
layout: post
title: Basic RCE 12
tags: [reversing, Study]
parent: CodeEngn
grand_parent: Reversing
nav_order: 10
---

# Basic RCE 12

![image](/assets/images/CodeEngn/Basic RCE 12/Basic RCE 12.png)

먼저 key 값을 찾기 위해 디버거 상에서 위와 같이 실행해 보았다

![image](/assets/images/CodeEngn/Basic RCE 12/Basic RCE 121.png)

![image](/assets/images/CodeEngn/Basic RCE 12/Basic RCE 122.png)

cmp eax, 7A2896BF에서 다이렉트로 입력한 값을 비교하는 것을 확인 할 수있다. 따라서 찾는 key 값은 7A2896BF이다

![image](/assets/images/CodeEngn/Basic RCE 12/Basic RCE 123.png)

 Hex Editor에서는 해당 영역을 키값으로 overwrite하면된다. 위의 영역을 키값을 10진수로 바꾼 2049480383로 채워야 하는데(문자열로), 해당 글자수는 10이고, 마지막을 NULL로 채워야하기 때문에, D3B부터 11바이트가 필요하다. 따라서 ??????는 D3B, D45이다.

![image](/assets/images/CodeEngn/Basic RCE 12/Basic RCE 124.png)

![image](/assets/images/CodeEngn/Basic RCE 12/Basic RCE 125.png)

히ㅣ히히히히히히히히히히히힣힣힣ㅎㅎ히