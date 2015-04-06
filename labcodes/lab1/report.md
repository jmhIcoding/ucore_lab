#LAB1#
##练习1
#####1. 操作系统镜像文件ucore.jmg是如何一步一步生成的？
观察`Makefile` 和`make "V="`命令的输出结果 ： 
最开始是寻找能生成i386-elf文件格式的工具，然后是一些包括路径和编译参数等信息的宏定义（在OS X中进行了一些修改）   
kernel是编译出的目标文件，再通过`dd`命令将kernl和bootblock生成ucore.img

#####2. 一个被系统认为是符合规范的硬盘主引导扇区的特征？
根据sign.c，有512字节，最后两个字节分别为0x55和0xAA



##练习2：使用qemu执行并调试lab1中的软件
使用make debug命令即可，在gdbinit事先写入初始命令

##练习3：分析bootloader进入保护模式的过程
####初始化
cli禁止中断，cld清除direcation flag，并将段符号清空
####A20开启
```
		# Enable A20:
		#  For backwards compatibility with the earliest PCs, physical
		#  address line 20 is tied low, so that addresses higher than
		#  1MB wrap around to zero by default. This code undoes this.
		seta20.1:
			inb $0x64, %al                                  # Wait for not busy(8042 input buffer empty).
			testb $0x2, %al
			jnz seta20.1

			movb $0xd1, %al                                 # 0xd1 -> port 0x64
			outb %al, $0x64                                 # 0xd1 means: write data to 8042's P2 port

		seta20.2:
			inb $0x64, %al                                  # Wait for not busy(8042 input buffer empty).
			testb $0x2, %al
			jnz seta20.2

			movb $0xdf, %al                                 # 0xdf -> port 0x60
			outb %al, $0x60                                 # 0xdf = 11011111, means set P2's A20 bit(the 1 bit) to 1
```
####初始化GDT表
lgdt gdtdesc
####使能和进入保护模式
* 设置CR0中的PE为1开启保护模式
* ljmp跳至protcseg，设置段寄存器，并设置好ebp,esp后跳到bootmain

##练习4：分析bootloader加载ELF格式的OS的过程
####读取硬盘
* 读取0x1f7，等待硬盘准备
* 0x1f2：读写的扇区数
* 0x1f3-0x1f6：扇区偏移
* 0x1f7 状态和命令寄存器
####加载ELF
* 加载ELF头
* 载入每一段到相应的地址
* 跳到e_entry

##练习5：实现函数调用堆栈跟踪函数
循环跳至ebp为0即可，和答案的实现略有不同

##练习6：完善中断初始化和处理
填入__vectors[]即可，注意SYSCALL是TRAP

























