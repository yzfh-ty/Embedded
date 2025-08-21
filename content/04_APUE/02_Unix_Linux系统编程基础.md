---
{"publish":true,"permalink":"/04_APUE/02_Unix_Linux系统编程基础.md","created":"2025-08-21T19:08:33.993+08:00","modified":"2025-08-21T20:27:49.513+08:00","cssclasses":""}
---

## APUE - Unix/Linux系统编程基础

### 文件描述符（File Descriptor）

**概念：**
- 非负整数，用于标识进程打开的文件
- 内核为每个进程维护一个文件描述符表
- 默认范围通常是 [0-1023]，可通过 `ulimit` 调整

**标准文件描述符：**
- `0`: `STDIN_FILENO`  - 标准输入
- `1`: `STDOUT_FILENO` - 标准输出  
- `2`: `STDERR_FILENO` - 标准错误输出

**文件描述符分配规则：**
- 总是分配当前可用的最小文件描述符号
- 保证同一时间最小值优先被使用

**相关系统调用和函数：**

```c
#include <unistd.h>
#include <fcntl.h>

// 获取文件描述符标志
int flags = fcntl(fd, F_GETFD);

// 设置文件描述符标志（如 FD_CLOEXEC）
fcntl(fd, F_SETFD, flags | FD_CLOEXEC);

// 复制文件描述符
int new_fd = dup(old_fd);           // 分配最小可用fd
int new_fd = dup2(old_fd, new_fd);  // 指定新的fd
```

**代码示例：查看当前进程的文件描述符**

```c
#include <stdio.h>
#include <unistd.h>
#include <fcntl.h>

int main() {
    int fd1, fd2;
    
    // 打开一个临时文件
    fd1 = open("/tmp/test.txt", O_CREAT | O_WRONLY | O_TRUNC, 0644);
    if (fd1 == -1) {
        perror("open");
        return 1;
    }
    
    printf("Opened file with fd: %d\n", fd1);
    
    // 再打开一个文件
    fd2 = open("/tmp/test2.txt", O_CREAT | O_WRONLY | O_TRUNC, 0644);
    if (fd2 == -1) {
        perror("open");
        return 1;
    }
    
    printf("Opened file with fd: %d\n", fd2);
    
    // 关闭第一个文件描述符
    close(fd1);
    
    // 再打开一个文件，应该会复用最小的fd
    int fd3 = open("/tmp/test3.txt", O_CREAT | O_WRONLY | O_TRUNC, 0644);
    printf("Reused fd: %d\n", fd3);  // 应该输出和fd1相同的值
    
    close(fd2);
    close(fd3);
    
    return 0;
}
```

**查看系统文件描述符限制：**

```bash
# 查看当前shell的文件描述符限制
ulimit -n

# 查看系统级文件描述符限制
cat /proc/sys/fs/file-max

# 查看特定进程打开的文件描述符
ls -l /proc/{pid}/fd/
```

### 原子操作（Atomic Operations）

**概念：**
原子操作是指由多个步骤组成的操作，如果该操作原子地执行，则要么执行完所有的步骤，要么一步也不执行，不允许被其他操作中断。这样保证了操作的完整性和数据的一致性。

**特性：**
- **不可分割性**：操作要么全部执行，要么完全不执行
- **一致性**：操作完成后系统保持一致性状态
- **隔离性**：并发执行时各操作互不干扰

**常见的原子操作场景：**

1. **文件操作的原子性：**
```c
#include <unistd.h>
#include <fcntl.h>

// 原子性的创建文件（O_CREAT | O_EXCL）
int fd = open("file.txt", O_WRONLY | O_CREAT | O_EXCL, 0644);
if (fd == -1) {
    if (errno == EEXIST) {
        printf("文件已存在\n");
    }
} else {
    printf("成功创建新文件\n");
    close(fd);
}
```

2. **内核级原子操作：**
```c
#include <stdatomic.h>

// C11 原子类型
atomic_int counter = ATOMIC_VAR_INIT(0);

// 原子自增
int new_value = atomic_fetch_add(&counter, 1) + 1;

// 原子比较并交换
int expected = 5;
int desired = 10;
if (atomic_compare_exchange_strong(&counter, &expected, desired)) {
    printf("交换成功\n");
} else {
    printf("交换失败，当前值为: %d\n", expected);
}
```

3. **文件锁的原子性：**
```c
#include <sys/file.h>

int fd = open("lockfile.txt", O_RDWR | O_CREAT, 0644);
if (fd != -1) {
    // 原子性的获取文件锁
    if (flock(fd, LOCK_EX | LOCK_NB) == 0) {
        printf("成功获取排他锁\n");
        // 执行临界区代码
        
        // 释放锁
        flock(fd, LOCK_UN);
    } else {
        printf("无法获取锁\n");
    }
    close(fd);
}
```

**文件系统原子操作示例：**

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <fcntl.h>
#include <string.h>

// 安全的写入文件函数
int safe_write_file(const char* filename, const char* data) {
    char temp_name[256];
    int fd;
    
    // 创建临时文件名
    snprintf(temp_name, sizeof(temp_name), "%s.tmp", filename);
    
    // 以独占模式创建临时文件
    fd = open(temp_name, O_CREAT | O_EXCL | O_WRONLY, 0644);
    if (fd == -1) {
        perror("open temp file");
        return -1;
    }
    
    // 写入数据
    if (write(fd, data, strlen(data)) == -1) {
        perror("write");
        close(fd);
        unlink(temp_name);
        return -1;
    }
    
    close(fd);
    
    // 原子性的重命名文件（替换原文件）
    if (rename(temp_name, filename) == -1) {
        perror("rename");
        unlink(temp_name);
        return -1;
    }
    
    return 0;
}

int main() {
    if (safe_write_file("test.txt", "Hello, Atomic World!\n") == 0) {
        printf("文件写入成功\n");
    } else {
        printf("文件写入失败\n");
    }
    
    return 0;
}
```

**原子操作的重要性：**

在多进程或多线程环境中，原子操作是保证数据一致性的关键：

```c
#include <sys/wait.h>
#include <unistd.h>
#include <stdio.h>
#include <stdlib.h>
#include <fcntl.h>

// 演示非原子操作的问题
void non_atomic_increment(const char* filename) {
    int fd = open(filename, O_RDWR);
    if (fd == -1) {
        perror("open");
        return;
    }
    
    // 读取值
    char buffer[32];
    lseek(fd, 0, SEEK_SET);
    int count = read(fd, buffer, sizeof(buffer) - 1);
    buffer[count] = '\0';
    int value = atoi(buffer);
    
    // 模拟处理时间（增加竞态条件发生的可能性）
    usleep(1000);
    
    // 写入新值
    lseek(fd, 0, SEEK_SET);
    char new_value[32];
    int len = snprintf(new_value, sizeof(new_value), "%d\n", value + 1);
    write(fd, new_value, len);
    
    close(fd);
}

// 使用文件锁的原子版本
void atomic_increment(const char* filename) {
    int fd = open(filename, O_RDWR);
    if (fd == -1) {
        perror("open");
        return;
    }
    
    // 获取文件锁
    if (flock(fd, LOCK_EX) == -1) {
        perror("flock");
        close(fd);
        return;
    }
    
    // 读取值
    char buffer[32];
    lseek(fd, 0, SEEK_SET);
    int count = read(fd, buffer, sizeof(buffer) - 1);
    buffer[count] = '\0';
    int value = atoi(buffer);
    
    // 处理
    usleep(1000);
    
    // 写入新值
    lseek(fd, 0, SEEK_SET);
    ftruncate(fd, 0);  // 清空文件
    char new_value[32];
    int len = snprintf(new_value, sizeof(new_value), "%d\n", value + 1);
    write(fd, new_value, len);
    
    // 释放锁
    flock(fd, LOCK_UN);
    close(fd);
}
```

**扩展知识点：**

1. **信号量原子操作：**
```c
#include <semaphore.h>

sem_t semaphore;
sem_init(&semaphore, 0, 1);  // 初始化为1

// 原子性的P操作（等待）
sem_wait(&semaphore);

// 临界区代码

// 原子性的V操作（释放）
sem_post(&semaphore);
```

2. **内存屏障和原子性：**
```c
#include <stdatomic.h>

atomic_thread_fence(memory_order_acquire);  // 获取屏障
// 读操作
atomic_thread_fence(memory_order_release);  // 释放屏障
// 写操作
```

