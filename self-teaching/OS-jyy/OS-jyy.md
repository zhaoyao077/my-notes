# OS-jyy

## 1-操作系统概述

- 学术诚信 Academic Integrity

  坚持自己心中的标准，经过长久的编码练习，你将会成为一个很不一样的人。

- 为什么要学操作系统？(为什么要学微积分/离散?)

  - 你体内的编程力量尚未觉醒
  - 《操作系统》给你有关“编程”的全部
  - 使用正确的教材、toolchain、实验

- 什么是操作系统？

  对单一计算机硬件系统作出抽象、支撑程序执行的软件系统。

- 操作系统历史

  - 1940s ENIAC 
    - 不需要操作系统
  - 1950s 批处理系统、Disk Operating System(DOS)
    - 操作系统概念出现
  - 1960s 多道程序处理、Multics
    - IO和CPU速度有差异，充分利用CPU资源
    - 基于中断
  - 1970s PC机、UNIX
  - 今天的操作系统
    - 通过“虚拟化”硬件资源为程序运行提供服务的软件

- <font color=red>三个基本问题</font>

  - 程序 = 状态机
  - 操作系统 = 对象 + API
  - 操作系统 = C程序

- 怎么学操作系统？

  - 是一个合格的操作系统用户
    - STFW/RTFM
    - vim/tmux/grep/gcc/binutils
  - 学会写代码
    - Talk is cheap, show me the code!



## 2-操作系统上的程序

- hanoi-r.c 汉诺塔 递归实现

  ```c
  void hanoi(int n, char from, char to, char via) {
    if (n == 1) printf("%c -> %c\n", from, to);
    else {
      hanoi(n - 1, from, via, to);
      hanoi(1,     from, to,  via);
      hanoi(n - 1, via,  to,  from);
    }
    return;
  }
  ```

- hanoi-nr.c 汉诺塔 非递归（通过栈帧模拟函数调用）

  ```c
  typedef struct {
    int pc, n;
    char from, to, via;
  } Frame;
  
  #define call(...) ({ *(++top) = (Frame) { .pc = 0, __VA_ARGS__ }; })
  #define ret()     ({ top--; })
  #define goto(loc) ({ f->pc = (loc) - 1; })
  
  void hanoi(int n, char from, char to, char via) {
    Frame stk[64], *top = stk - 1;
    call(n, from, to, via);
    for (Frame *f; (f = top) >= stk; f->pc++) {
      switch (f->pc) {
        case 0: if (f->n == 1) { printf("%c -> %c\n", f->from, f->to); goto(4); } break;
        case 1: call(f->n - 1, f->from, f->via, f->to);   break;
        case 2: call(       1, f->from, f->to,  f->via);  break;
        case 3: call(f->n - 1, f->via,  f->to,  f->from); break;
        case 4: ret();                                    break;
        default: assert(0);
      }
    }
  }
  ```

- 什么是程序？

  - 程序 = 计算 + syscall

- 理解操作系统的重要工具

  - gcc, binutils, gdb, strace



## 3-多处理器编程

- 并发

  - 操作系统是最早的并发程序之一
  - 今天遍地是多处理器系统

- 并发程序的每一步都是不确定的

- thread.h 线程库

  ```c
  #include "thread.h"
  
  void Ta(){ while(1){ printf("a"); } }
  void Tb(){ while(1){ printf("b"); } }
  
  int main(){
      create(Ta);
      create(Tb);
  }
  ```

  运行后，可以看到CPU使用率超过了100%

- 原子性的丧失

  - 线程在运行时可能被中断，切换到另一个线程执行。

- 顺序的丧失

  - 并发程序用gcc的-o1优化和-o2优化可能得到不同的结果
  - 编译器优化保证**eventually consistent**

- 可见性的丧失，宽松内存模型

  ```c
  int x = 0, y = 0;
  
  void T1() {
    x = 1;
    asm volatile("" : : "memory"); // compiler barrier
    printf("y = %d\n", y);
  }
  
  void T2() {
    y = 1;
    asm volatile("" : : "memory"); // compiler barrier
    printf("x = %d\n", x);
  }
  ```

  运行该程序可能输出``` 0 0```，```0 1```，```1 0```，```1 1```。

  <img src="C:\Users\ARmi\AppData\Roaming\Typora\typora-user-images\image-20230121095005321.png" alt="image-20230121095005321" style="zoom:50%;" /> 

- 实现顺序一致性
  - Compiler barrier + fence 指令



## 4-理解并发程序执行

- Peterson算法

























