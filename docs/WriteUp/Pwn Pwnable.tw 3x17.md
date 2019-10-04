## 0x00 分析

checksec之

```
Arch:     amd64-64-little
RELRO:    Partial RELRO
Stack:    No canary found
NX:       NX enabled
PIE:      No PIE (0x400000)

```

ida分析，发现有大量函数，同时没有加载符号，判断该程序采用静态编译，推测是ROP的题

这时候可以通过工具，如[lscan](https://github.com/maroueneboubakri/lscan)来复原符号文件，不过该题靠自己猜测也能达到同样目的

运行3x17，有"addr"等字符串，通过在ida查找，发现位于`0x0000000000401B6D`上的函数，分析该函数

```c
__int64 __fastcall sub_401B6D(__int64 a1, char *a2, __int64 a3)
{
  __int64 result; // rax
  char *v4; // ST08_8
  __int64 v5; // rcx
  unsigned __int64 v6; // rt1
  char buf; // [rsp+10h] [rbp-20h]
  unsigned __int64 v8; // [rsp+28h] [rbp-8h]

  v8 = __readfsqword(0x28u);
  result = (unsigned __int8)++byte_4B9330;
  if ( byte_4B9330 == 1 )
  {
    sub_446EC0(1u, "addr:", 5uLL);
    sub_446E20(0, &buf, 0x18uLL);
    v4 = (char *)(signed int)sub_40EE70(&buf, &buf);
    sub_446EC0(1u, "data:", 5uLL);
    a2 = v4;
    a1 = 0LL;
    sub_446E20(0, v4, 0x18uLL);
    result = 0LL;
  }
  v6 = __readfsqword(0x28u);
  v5 = v6 ^ v8;
  if ( v6 != v8 )
    sub_44A3E0(a1, a2, a3, v5);
  return result;
}

```

通过调试与系统的调用，猜测各个函数的效果

<font color=d55fde>*教练我想学会看汇编！*</font>

* [__fastcall](#调用)

```c
__int64 __fastcall sub_401B6D(__int64 a1, char *a2, __int64 a3)
{
  __int64 result; // rax
  int v4; // eax
  char *addr; // ST08_8
  char buf; // [rsp+10h] [rbp-20h]
  unsigned __int64 v8; // [rsp+28h] [rbp-8h]

  v8 = __readfsqword(0x28u);
  result = (unsigned __int8)++byte_4B9330;
  if ( byte_4B9330 == 1 )
  {
    my_write(1u, "addr:", 5uLL);
    my_read(0, &buf, 24uLL);
    str2num((__int64)&buf);                     // 返回值保存在eax，即v4
    addr = (char *)v4;
    my_write(1u, "data:", 5uLL);
    my_read(0, addr, 24uLL);
    result = 0LL;
  }
  if ( __readfsqword(0x28u) != v8 )
    printStackDetect();
  return result;
}
```

很明显是输入地址，然后在所输入的地址上写值

如果需要构造rop链来达到想要的目的，首先需要劫持函数的执行流程

### ROP链的构建

[Ropgadget](https://github.com/JonathanSalwan/ROPgadget)


### 函数的劫持


## 参考知识



### <span id="调用">调用协议</span>

调用协议常用场合

* __stdcall： Windows API默认的函数调用协议。
* __cdecl：   C/C++默认的函数调用协议。
* __fastcall：适用于对性能要求较高的场合。


函数参数入栈方式

* __stdcall：  函数参数 **由右向左** 入栈。
* __cdecl：    函数参数 **由右向左** 入栈。
* __fastcall： **从左开始不大于4字节的参数** 放入CPU的ECX和EDX寄存器，其余参数 **从右向左** 入栈。

    __fastcall在寄存器中放入不大于4字节的参数，故性能较高，适用于需要高性能的场合。

栈内数据清除方式

* __stdcall： 函数调用结束后由 **被调用函数** 清除栈内数据。
* __cdecl：   函数调用结束后由 **函数调用者** 清除栈内数据。
* __fastcall：函数调用结束后由 **被调用函数** 清除栈内数据。

注意

不同编译器设定的栈结构不尽相同，跨开发平台时由函数调用者清除栈内数据不可行。
某些函数的参数是可变的，如printf函数，这样的函数只能由函数调用者清除栈内数据。
由调用者清除栈内数据时，每次调用都包含清除栈内数据的代码，故可执行文件较大。

