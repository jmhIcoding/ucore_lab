#LAB4#
##练习1：分配并初始化一个进程控制块
分配一个进程块之后将相应的变量清零即可。

##练习2：为新创建的内核线程分配资源
注释中的说明非常详细： 

1. call alloc_proc to allocate a proc_struct  
2. call setup_kstack to allocate a kernel stack for child process 
3. call copy_mm to dup OR share mm according clone_flag 
4. call copy_thread to setup tf & context in proc_struct
5. insert proc_struct into hash_list && proc_list
6. call wakup_proc to make the new child process RUNNABLE
7. set ret vaule using child proc's pid

##练习3：阅读代码，理解 proc_run 函数和它调用的函数如何完成进程切换的
proc_run运行：判断下一个进程，切换栈基址和PDT，switch完成转换


##问题和区别
答案在处理的时候进行了临界处理
