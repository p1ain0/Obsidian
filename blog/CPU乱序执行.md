先看一个例子：

```c
#include <stdlib.h>
#include <stdio.h>
#include <string.h>
#include <stdatomic.h>
#include <assert.h>
#include <unistd.h>
#include <pthread.h>

int x, y;
atomic_int flag;
#define FLAG atomic_load(&flag)
#define FLAG_XOR(val) atomic_fetch_xor(&flag, val)
#define WAIT_FOR(cond) while (!(cond)) ;

 __attribute__((noinline))
void write_x_read_y() {
  x=1;
  // __sync_synchronize();
  printf("%d ", y);
}

 __attribute__((noinline))
void write_y_read_x() {
  y=1;
  // __sync_synchronize();
  printf("%d ", x);
}

void T1(int id) {
  while (1) {
    WAIT_FOR((FLAG & 1));
    write_x_read_y();
    FLAG_XOR(1);
  }
}

void T2() {
  while (1) {
    WAIT_FOR((FLAG & 2));
    write_y_read_x();
    FLAG_XOR(2);
  }
}

void Tsync() {
  while (1) {
    x = y = 0;
    //__sync_synchronize(); // full barrier
    //usleep(1);            // + delay
    // assert(FLAG == 0);
    FLAG_XOR(3);
    // T1 and T2 clear 0/1-bit, respectively
    WAIT_FOR(FLAG == 0);
    printf("\n"); fflush(stdout);
  }
}

int main() {
  pthread_t tid1;
  pthread_t tid2;
  pthread_t tid3;
  int ret;
  void *ptr = &ret;
  ret = pthread_create(&tid1, NULL, T1, ptr);

  ret = pthread_create(&tid2, NULL, T2, ptr);

  ret = pthread_create(&tid3, NULL, Tsync, ptr);

  pthread_join(tid1, NULL);
  pthread_join(tid2, NULL);
  pthread_join(tid3, NULL);
  while(1){}
  return 0;
  
}
```

这段代码创建了三个线程，T1， T2，Tsync。
Tsync的作用是协同T1和T2线程的，先将x，y重新初始化为0，再把T1和T2继续执行的条件FLAG复位，等T1和T2都打印完一遍后，输出换行符。
T1先将x赋值为1，再打印y的值。
T2先将y赋值为1，再打印x的值。
理论上打印的结果不可能出现 0 0 的情况。但是试验结果：

```shell
gcc mem-ordering.c -o test
./test | head -n 1000000 | sort | uniq -c

987382 0 0 
10216 0 1 
2396 1 0 
   6 1 1 
```

这证明CPU确实实在乱序执行的。
但是如果将`__sync_synchronize();`这两行注释打开。结果就不一样了：

```shell
366378 0 1 
30491 1 0 
603128 1 1 
```

这次阻止了乱序执行。打开反汇编代码一窥究竟，

```assembly

0000000100003c90 <_write_x_read_y>:
100003c90: 55                           pushq   %rbp
100003c91: 48 89 e5                     movq    %rsp, %rbp
100003c94: 48 83 ec 10                  subq    $16, %rsp
100003c98: 48 8d 05 65 43 00 00         leaq    17253(%rip), %rax       ## 0x100008004 <_x>
100003c9f: c7 00 01 00 00 00            movl    $1, (%rax)
100003ca5: 0f ae f0                     mfence
100003ca8: 48 8d 05 59 43 00 00         leaq    17241(%rip), %rax       ## 0x100008008 <_y>
100003caf: 8b 30                        movl    (%rax), %esi
100003cb1: 48 8d 3d f4 02 00 00         leaq    756(%rip), %rdi         ## 0x100003fac <_pthread_join+0x100003fac>
100003cb8: b0 00                        movb    $0, %al
100003cba: e8 db 02 00 00               callq   0x100003f9a <_pthread_join+0x100003f9a>
100003cbf: 48 83 c4 10                  addq    $16, %rsp
100003cc3: 5d                           popq    %rbp
100003cc4: c3                           retq
100003cc5: 66 2e 0f 1f 84 00 00 00 00 00        nopw    %cs:(%rax,%rax)
100003ccf: 90                           nop

0000000100003cd0 <_write_y_read_x>:
100003cd0: 55                           pushq   %rbp
100003cd1: 48 89 e5                     movq    %rsp, %rbp
100003cd4: 48 83 ec 10                  subq    $16, %rsp
100003cd8: 48 8d 05 29 43 00 00         leaq    17193(%rip), %rax       ## 0x100008008 <_y>
100003cdf: c7 00 01 00 00 00            movl    $1, (%rax)
100003ce5: 0f ae f0                     mfence
100003ce8: 48 8d 05 15 43 00 00         leaq    17173(%rip), %rax       ## 0x100008004 <_x>
100003cef: 8b 30                        movl    (%rax), %esi
100003cf1: 48 8d 3d b4 02 00 00         leaq    692(%rip), %rdi         ## 0x100003fac <_pthread_join+0x100003fac>
100003cf8: b0 00                        movb    $0, %al
100003cfa: e8 9b 02 00 00               callq   0x100003f9a <_pthread_join+0x100003f9a>
100003cff: 48 83 c4 10                  addq    $16, %rsp
100003d03: 5d                           popq    %rbp
100003d04: c3                           retq
100003d05: 66 2e 0f 1f 84 00 00 00 00 00        nopw    %cs:(%rax,%rax)
100003d0f: 90                           nop

```

原来 `__sync_synchronize()` 函数在中间添加了`mfence`指令起到内存屏障的作用。
假设现在写x读y。正常情况下当cpu写x的时候，发现不在缓存里，而下一条读指令的y恰好在缓存里，然后他就想：“反正这俩有没有关系，我就先读y了，先让x加载着缓存，等我读完y了再写x一样，还更快”。但是加了mfence就不一样了，mfence相当于告诉cpu你不准这么干，前边的那条指令的操作数不在缓存里，你就给我等着。必须把前边干完了，再干下一条。


每天一个无用小技巧。今天你学废了吗？