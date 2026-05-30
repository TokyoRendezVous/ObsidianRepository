> `struct proc` 是 xv6 内核里“进程控制块”，也就是 PCB。每个进程在内核中都有一个 `struct proc`，所有进程放在全局进程表：

```c
struct proc {
  struct spinlock lock;

  enum procstate state;
  struct proc *parent;
  void *chan;
  int killed;
  int xstate;
  int pid;

  uint64 kstack;
  uint64 sz;
  pagetable_t pagetable;
  struct trapframe *trapframe;
  struct context context;
  struct file *ofile[NOFILE];    
  struct inode *cwd;
  char name[16];
};
```

**记录了一个进程在内核中的所有关键状态**，包括：
- 进程是否可运行、正在运行、睡眠或僵尸
- 进程号 pid
- 父子关系
- 用户地址空间
- 内核栈
- 陷入时保存的用户寄存器
- 上下文切换需要的内核寄存器
- 打开的文件
- 当前工作目录
- 调试用进程名

## lock 锁

## state 进程状态

## parent 父进程

## chan 睡眠通道

## xstate 退出状态

## pid 进程号

## kstack 内核栈

## sz 用户内存大小

## pagetable 用户页表

## trapframe 陷阱帧

## context 内核上下文

## ofile 打开文件表

```c
struct file *ofile[NOFILE];
```

ofile 是一个数组，数组里的每个元素都是 struct file * 指针。
用户态看到的 fd，比如 0、1、2、3...，其实就是这个数组的下标。

```c
struct file {
  enum { FD_NONE, FD_PIPE, FD_INODE, FD_DEVICE } type;    // 这个fd对应的是普通文件，管道还是设备
  int ref;    //引用计数
  char readable;    //是否可读
  char writable;    //是否可写
  struct pipe *pipe;    //如果是管道，是哪个管道
  struct inode *ip;    // 如果是文件，对应哪个inode
  uint off;    //当前文件偏移量
  short major;
};
```

`fd`是用户进程看到的整数
`ofile[fd]` 是进程表里的指针
`struct file` 是内核里真正描述“文件打开状态”的对象
`inode` 是文件系统里描述“磁盘文件本身“的对象

**为什么fd不直接指向文件，而是通过ofile中保存的struct file指针**
如果多个文件描述符指向同一个打开的文件
例如
```c
fd1 = open()
fd2 = dup(fd1)
```
如果这时`read(fd1,...)` ，会更新偏移量，fd2会从新的位置开始。
> 文件偏移量offset表示**下一次读写操作应该从文件的哪个字节开始。**




## cwd 当前工作目录

## name 进程名

