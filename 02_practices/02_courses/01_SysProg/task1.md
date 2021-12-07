1. write实现如下
  
  ssize_t write(int fd, const void *buf, size_t nbytes){
      if (nbytes == 0)
          return 0;
      ssize_t ret = syscall(SYS_write,fd,buf,nbytes);
      if(ret<0){errno = -ret; ret=-1;}
      return ret;
  }

  返回值是ssize_t，syscall参数直接传入，但syscall的返回值需要转换一下以使得和实际read/write返回值一致。

2. write assembly 版本如下

    ssize_t ret;
    asm volatile(
        "syscall"
        : "=a" (ret)                                       //output param
        //                 RDI      RSI       RDX
        : "a"(__NR_write), "D"(fd), "S"(buf), "d"(nbytes)  //input param
        : "rcx", "r11", "memory"                           //RCX and R11 to save the user-space RIP and RFLAGS 
    );//arch: amd64
    if(ret<0){
        errno = -ret;
        return -1;
    }
    return ret;

    `rax`传入宏__NR_write，`rdi`,`rsi`和`rdx`分别传入对应变参，返回值赋值回给变量ret，同理，需要转换syscall返回值。

3. ptrace
    ptrace使得tracer能够监测和控制tracee的运行，具体来说可以监测且改变tracee的内存和寄存器，常被用作断点的实现。
    tracer首先要attach tracee，附着和命令都是基于线程的。且每个指令都基于形式`ptrace(PTRACE_foo, pid, ...)`
    其过程一般为:

    - tracer fork subprocess，子进程执行`ptrace(PTRACE_TRACEME, 0, 0, 0); execvp(argv[1], argv + 1);`系列语句
    - tracee将会在每次给出信号后停止。此时tracer的调用`waitpid(pid, &status, 0)`将会返回。
    - 在本任务中用到的指令有俩，即`ptrace(PTRACE_SYSCALL, pid, 0, 0);`使在下一次系统调用时返回，与`ptrace(PTRACE_GETREGS, pid, 0, &regs);`得到其用户寄存器。
    - 注意，当判断`regs.orig_rax`为期望的`__NR_read`或`__NR_write`后，可以直接打出系统调用及其入参，之后还需要紧接着再次激活`ptrace(PTRACE_SYSCALL, pid, 0, 0);`，以得到系统调用的返回值`regs.rax`

