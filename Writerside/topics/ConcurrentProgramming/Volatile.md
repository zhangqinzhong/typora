# volatile原理
volatile 的底层实现原理是内存屏障, Memory Barrier( Memory Fence)

对 volatile变量的写指令后会加入写屏障 

对 volatile变量的读指令前会加入读屏障

<img src="../../images/ConcurrentProgramming/images/volatile原理1.jpg" alt="img" style="zoom:50%;" />

<img src="../../images/ConcurrentProgramming/images/volatile原理2.jpg" style="zoom:40%;" />

<img src="../../images/ConcurrentProgramming/images/volatile原理3.jpg" style="zoom:50%;" />

