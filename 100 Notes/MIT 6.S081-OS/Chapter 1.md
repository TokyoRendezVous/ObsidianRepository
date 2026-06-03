### 前置知识
#### read & write函数
```c
ssize_t read(int fd, void *buf, size_t count);
ssize_t write(int fd, const void *buf, size_t count);
```

**fd** : 文件描述符
> **文件描述符是用户程序和内核中 I/O 对象之间的“句柄”。程序通过整数 fd 找到内核维护的打开对象，再用统一的** **`read()`** **/** **`write()`** **接口进行操作。**



```c
int p[2];
char *argv[2];

argv[0] = "wc";
argv[1] = 0;

pipe(p);

if(fork()==0){
	close(0);
	dup(p[0]);
	close(p[0]);
	close(p[1]);
	exec("/bin/etc", argv);
} else {
	close(p[0]);
	write(p[1], "hello world!", 12);
	close(p[1]);
}
```

fork()会复制当前进程，创建子进程。两个进程都从fork返回处继续执行。所以不是一个进程同时进入 if 和 else，而是# **两个不同进程分别进入不同分支**。

fd是进程fd table的索引，每个进程内都有fd table。例如：

| index | object   |
| ----- | -------- |
| 0     | keyboard |
| 1     | screen   |
| 2     | stderr   |
| 3     | file.txt |
close(0)真正做的是**释放fd table**的第0项。

对于dup[p[0]]：**复制** **`p[0]`** **这个文件描述符，并让新文件描述符指向同一个 pipe 读端。**

从pipe(p)开始，假设fd table如下：
```c
fd 0 -> keyboard / console input
fd 1 -> screen / console output
fd 2 -> stderr
fd 3 -> pipe read end   // p[0]
fd 4 -> pipe write end  // p[1]
```

close(0)释放了fd table的第0项
```c
fd 0 -> empty
fd 1 -> screen / console output
fd 2 -> stderr
fd 3 -> pipe read end   // p[0]
fd 4 -> pipe write end  // p[1]
```

然后dup(p[0])，`dup` 会做两件事。第一，它找到当前最小的空闲 fd。这里最小空闲 fd 是 `0`。第二，它让这个新 fd 指向和 `p[0]` 相同的内核 I/O 对象，也就是同一个 pipe read end。
```c
fd 0 -> pip read end
fd 1 -> screen / console output
fd 2 -> stderr
fd 3 -> pipe read end   // p[0]
fd 4 -> pipe write end  // p[1]
```

fd 0成功指向pip read end后，原来的p[0]就不需要了，所以`close(p[0])`，因为子进程只负责读，不负责写，所以`close(p[1])`。

---
如果不执行`close(p[1])`会怎么样？

首先，pipe的EOF（End of File）机制与普通文件不同，普通文件的指针指向末尾时，read（）会直接返回0；但pipe本质是一个动态数据流，内核无法通过“缓冲区是否为空”判断是否还有数据要输入，**只有所有 write end 都被关闭时，read() 才会返回 0。**

因此即使父进程执行`close(p[1])`，如果子进程中p[1]不删除的话，这说明系统中仍然存在任何一个指向 pipe write end 的 file descriptor，内核就认为未来仍可能有数据写入，因此读端不会收到 EOF，而会继续阻塞等待数据。

因此，在 pipe 编程中：每个进程必须关闭自己不使用的 pipe 端。关闭 unused write end 的真正意义，不只是“释放 fd”，更重要的是向内核声明： **“这个进程以后不会再向 pipe 写数据了。”** 只有当所有 writer 都完成这一动作后，pipe reader 才能正确收到 EOF。

---
