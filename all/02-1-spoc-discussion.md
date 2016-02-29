#lec 3 SPOC Discussion

##**提前准备**
（请在周一上课前完成）

 - 完成lec3的视频学习和提交对应的在线练习
 - git pull ucore_os_lab, v9_cpu, os_course_spoc_exercises  　in github repos。这样可以在本机上完成课堂练习。
 - 仔细观察自己使用的计算机的启动过程和linux/ucore操作系统运行后的情况。搜索“80386　开机　启动”
 - 了解控制流，异常控制流，函数调用,中断，异常(故障)，系统调用（陷阱）,切换，用户态（用户模式），内核态（内核模式）等基本概念。思考一下这些基本概念在linux, ucore, v9-cpu中的os*.c中是如何具体体现的。
 - 思考为什么操作系统需要处理中断，异常，系统调用。这些是必须要有的吗？有哪些好处？有哪些不好的地方？
 - 了解在PC机上有啥中断和异常。搜索“80386　中断　异常”
 - 安装好ucore实验环境，能够编译运行lab8的answer
 - 了解Linux和ucore有哪些系统调用。搜索“linux 系统调用", 搜索lab8中的syscall关键字相关内容。在linux下执行命令: ```man syscalls```
 - 会使用linux中的命令:objdump，nm，file, strace，man, 了解这些命令的用途。
 - 了解如何OS是如何实现中断，异常，或系统调用的。会使用v9-cpu的dis,xc, xem命令（包括启动参数），分析v9-cpu中的os0.c, os2.c，了解与异常，中断，系统调用相关的os设计实现。阅读v9-cpu中的cpu.md文档，了解汇编指令的类型和含义等，了解v9-cpu的细节。
 - 在piazza上就lec3学习中不理解问题进行提问。

## 第三讲 启动、中断、异常和系统调用-思考题

## 3.1 BIOS
 1. 比较UEFI和BIOS的区别。
 1. 描述PXE的大致启动流程。

## 3.2 系统启动流程
 1. 了解NTLDR的启动流程。
 1. 了解GRUB的启动流程。
 1. 比较NTLDR和GRUB的功能有差异。
 1. 了解u-boot的功能。
    - NTLDR是windows 2000/xp/2003使用的bootloader. NTLDR由两部分构成, 前面是16位代码, 后部分是标准的PE格式文件, NTLDR被MBR代码读入, 此时还是16位实模式, 他负责切换到保护模式, 并且建立页表使前16MB物理内存能够通过分页访问. 而且它需要调用BIOS中断是需要暂时切换回实模式. 接下来它读取boot.ini文件, 从而得到Windows文件夹的位置. 接下来执行ntdetect.com检测硬件信息. 最后加载ntoskrnl.exe, 也就是真正的内核文件, hal, 硬件抽象层, 加载注册表, 驱动程序等并跳转到ntoskrnl.exe中执行.
    - grub主要工作就是加载内核, 之后所有检测,加载模块等任务都是内核来完成的

## 3.3 中断、异常和系统调用比较
 1. 什么是中断、异常和系统调用？
 2. 中断、异常和系统调用的处理流程有什么异同？
 2. 举例说明Linux中有哪些中断，哪些异常？
 3. 以ucore lab8的answer为例，uCore的时钟中断处理流程。
 1. Linux的系统调用有哪些？大致的功能分类有哪些？  (w2l1)

 ```
  + 采分点：说明了Linux的大致数量（上百个），说明了Linux系统调用的主要分类（文件操作，进程管理，内存管理等）
  - 答案没有涉及上述两个要点；（0分）
  - 答案对上述两个要点中的某一个要点进行了正确阐述（1分）
  - 答案对上述两个要点进行了正确阐述（2分）
  - 答案除了对上述两个要点都进行了正确阐述外，还进行了扩展和更丰富的说明（3分）
 ```

 1. 以ucore lab8的answer为例，uCore的系统调用有哪些？大致的功能分类有哪些？(w2l1)

 ```
  + 采分点：说明了ucore的大致数量（二十几个），说明了ucore系统调用的主要分类（文件操作，进程管理，内存管理等）
  - 答案没有涉及上述两个要点；（0分）
  - 答案对上述两个要点中的某一个要点进行了正确阐述（1分）
  - 答案对上述两个要点进行了正确阐述（2分）
  - 答案除了对上述两个要点都进行了正确阐述外，还进行了扩展和更丰富的说明（3分）
 ```
 1. 进程管理
    - exit退出进程
    - fork复制进程
    - wait等待进程退出
    - exec加载可执行文件并且跳转,
    - yield重新调度
    - kill杀死进程
    - getpid获得进程id
    - sleep进程睡眠
  2. I/O
    - putc终端输出一个字符
    - pgdir终端打印页表
    - gettime获得当前时间
    - open,close,read,write,seek,fstatfsync文件操作
    - getcwd获得当前工作目录
    - getdirentry目录下文件
    - dup复制文件

## 3.4 linux系统调用分析
 1. 通过分析[lab1_ex0](https://github.com/chyyuu/ucore_lab/blob/master/related_info/lab1/lab1-ex0.md)了解Linux应用的系统调用编写和含义。(w2l1)


 ```
  + 采分点：说明了objdump，nm，file的大致用途，说明了系统调用的具体含义
  - 答案没有涉及上述两个要点；（0分）
  - 答案对上述两个要点中的某一个要点进行了正确阐述（1分）
  - 答案对上述两个要点进行了正确阐述（2分）
  - 答案除了对上述两个要点都进行了正确阐述外，还进行了扩展和更丰富的说明（3分）

 ```
 - objdump是elf格式文件分析的工具, 常用的参数有-f显示头部信息, -d反汇编, -h显示section header细腻等等, -T显示符号表
 - nm是分析符号表, 和objdump -T类似(来自man)
 - file解析文件类型
 - linux系统调用用eax传递系统调用号, ebx,ecx,edx剩下的参数用堆栈传递
 - 使用的系统调用:
    1. 先在地址0处分配内存, 检查/etc/ld.so.nohwcap, /etc/ld.so.preload文件, 加载/etc/ld.so.cache到内存虚拟地址0处, 加载/lib/i386-linux-gnu/libc.so.6库, 这个可能是因为在64位系统上运行32位程序加载的库, 对该库文件的不同的section做内存映射从而使从而使动态链接库能被加载到文件中指定的位置, 最后关闭文件, 设置这些动态链接库被映射到的内存为只读. 最后调用write, 输出到内存.


 1. 通过调试[lab1_ex1](https://github.com/chyyuu/ucore_lab/blob/master/related_info/lab1/lab1-ex1.md)了解Linux应用的系统调用执行过程。(w2l1)


 ```
  + 采分点：说明了strace的大致用途，说明了系统调用的具体执行过程（包括应用，CPU硬件，操作系统的执行过程）
  - 答案没有涉及上述两个要点；（0分）
  - 答案对上述两个要点中的某一个要点进行了正确阐述（1分）
  - 答案对上述两个要点进行了正确阐述（2分）
  - 答案除了对上述两个要点都进行了正确阐述外，还进行了扩展和更丰富的说明（3分）
 ```

## 3.5 ucore系统调用分析
 1. ucore的系统调用中参数传递代码分析。
<<<<<<< HEAD
 1. ucore的系统调用中返回结果的传递代码分析。
 1. 以ucore lab8的answer为例，分析ucore 应用的系统调用编写和含
=======
 1. 以getpid为例，分析ucore的系统调用中返回结果的传递代码。
 1. 以ucore lab8的answer为例，分析ucore 应用的系统调用编写和含义。
>>>>>>> 9b21abd1c0113ce5cf1a68a96e8e4230cede2502
 1. 以ucore lab8的answer为例，尝试修改并运行ucore OS kernel代码，使其具有类似Linux应用工具`strace`的功能，即能够显示出应用程序发出的系统调用，从而可以分析ucore应用的系统调用执行过程。

 - 系统调用在内核的实现
   ```
     int num = tf->tf_regs.reg_eax;
     if (num >= 0 && num < NUM_SYSCALLS) {
         if (syscalls[num] != NULL) {
             arg[0] = tf->tf_regs.reg_edx;
             arg[1] = tf->tf_regs.reg_ecx;
             arg[2] = tf->tf_regs.reg_ebx;
             arg[3] = tf->tf_regs.reg_edi;
             arg[4] = tf->tf_regs.reg_esi;
             tf->tf_regs.reg_eax = syscalls[num](arg);
             return ;
         }
     }
     ```
   tf->tf_regs是用户进程触发系统调用时的寄存器的值, 用户使用eax传递系统调用号, edx,ecx,ebx,edi,esi传递前5个参数, eax传递返回值. 当系统调用完成时, tf_regs载入真正的寄存器, 从而用户进程可以通过eax获得系统调用的返回值.
  - 系统调用在用户应用程序库中的实现
      ```
      static inline int
      syscall(int num, ...) {
          va_list ap;
          va_start(ap, num);
          uint32_t a[MAX_ARGS];
          int i, ret;
          for (i = 0; i < MAX_ARGS; i ++) {
              a[i] = va_arg(ap, uint32_t);
          }
          va_end(ap);

          asm volatile (
              "int %1;"
              : "=a" (ret)
              : "i" (T_SYSCALL),
                "a" (num),
                "d" (a[0]),
                "c" (a[1]),
                "b" (a[2]),
                "D" (a[3]),
                "S" (a[4])
              : "cc", "memory");
          return ret;
      }
      ```
      用fork调用举例
      ```
      int
      sys_fork(void) {
          return syscall(SYS_fork);
      }
      ```
      syscall是个可变参数的函数, 会把用户传来的前5个参数分别存入上面说的几个寄存器中然后执行int 0x80, 然后把从系统调用后eax得到的返回值返回给调用者. 这样就把系统调用封装成了一个函数, 用户就可以把系统调用看做函数来用.

## 3.6 请分析函数调用和系统调用的区别
   1. 请从代码编写和执行过程来说明。
   1. 说明`int`、`iret`、`call`和`ret`的指令准确功能
<<<<<<< HEAD
      1. 系统调用需要切换特权级, 也就是说要切换数据段寄存器, 代码段寄存器, 还要切换堆栈, 还要屏蔽中断, 用寄存器传递参数, 参数个数有限制; 而函数调用以上这些都不需要, 只需要保存通用寄存器, 返回地址. 而且一般的c函数是用堆栈传递参数的, 参数个数可以很多, 只要不发生堆栈溢出就行.
      2. int (保护模式,CPL=3)
          - 是否中断门描述符是否DPL=CPL
          - 检查IDT中对应的门描述符是否有效
          - 指向的代码段是否满足,非一致代码段且DPL<CPL
          - 切换堆栈
          - 装入CS
          - EFLAGS,CS,EIP入栈
          - TF=0,NT=0
          - 如果是中断门,关中断
          - 如果有出错码出错码入栈
          - 跳转到对应的中断处理程序
      3. iret
          - 弹出EIP,CS,EFLAGS
          - 通过CS确定RPL
          - 如果需要切换特权级,从堆栈弹出ss,esp切换堆栈
          - 如果有错误代码的中断则需要弹出错误码
          - 通过堆栈返回

      4. call/ret也可以实现特权级的切换, 介绍不发生特权级切换的情况.call:
          - 返回地址入栈
          - 跳转到指定地址
      ret: 从堆栈中取出eip, ret可以有参数, 这样会自动恢复堆栈, 在windows的调用规范stdcall中是被调用者负责恢复堆栈, 这条指令会用到.
=======
 

## v9-cpu相关题目
---

### 提前准备
```
cd YOUR v9-cpu DIR
git pull 
cd YOUR os_course_spoc_exercise DIR
git pull 
```

### v9-cpu系统调用实现
  1. v9-cpu中os4.c的系统调用中参数传递代码分析。
  1. v9-cpu中os4.c的系统调用中返回结果的传递代码分析。
  1. 理解v9-cpu中os4.c的系统调用编写和含义。

>>>>>>> 9b21abd1c0113ce5cf1a68a96e8e4230cede2502
