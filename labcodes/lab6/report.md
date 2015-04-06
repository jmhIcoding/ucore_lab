#LAB6

##练习1: 使用 Round Robin 调度算法
RoundRobin就是按时间分配，用完了换下一个进程，比较直观。

##练习2：实现 Stride Scheduling 调度算法
使用堆按Stride大小维护进程队列，每次一个进程进入后就会增加Stride值，从而保证调用次数和priority成正比。实现基本就是调用skew_list的函数，然后更新值即可。

=========
说明很详细，代码实现基本一样。  

主要区别在与alloc_proc时，参考答案设置priority为0，然后在其他地方进行特判（防止出现divide zero），但是我在实现的时候直接将priority设置成了1  

另外输出的结果可以通过，但是输出顺序和文档中的不一样
