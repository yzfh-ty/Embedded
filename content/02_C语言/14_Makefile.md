---
{"publish":true,"permalink":"/02_C语言/14_Makefile.md","created":"2025-08-02T16:46:05.187+08:00","modified":"2025-08-02T17:08:26.486+08:00","cssclasses":""}
---

**Makefile** 适用于自动化编译和构建项目的工具（通过make命令调用），其语法定义编译和连接的顺序和规则。

安装

```shell
sudo apt install make
```

基本语法

```
目标文件: 依赖文件
<制表符>    命令 
```

如果`.o` 文件不存在，则查找生成`.o`的命令进行编译
如果`.o` 的时间戳比myprog新也会执行

示例

```shell
myprog : main.o prime.o  
    gcc -o myprog main.o prime.o  
  
main.o: main.c  
    gcc -c -o main.o main.c  
  
prime.o: prime.c  
    gcc -c -o prime.o prime.c
```

运行

```shell
make 目标
```

变量:

```
变量 = 值
```

变量的取值

```shell
$(变量)
```

内置变量:

```
$@ 目标文件
$^ 所有依赖文件
$< 第一个依赖文件
```

模式规则

```
%.o : %.c
	$(CC) $(CFLAG) -c -o $@ $<
```

最终的 Makefile文件

```makefile
# 定义编辑器  
CC = gcc  
# 定义编辑器 选项  
CFLAG = -g  
SRCS = main.c prime.c  # 需要编译的文件
OBJS = $(SRCS:.c=.o)  # OBJS = main.o prime.o  # 转成 .o 文件
TARGET=myprog  # 生成的可执行文件名
  
$(TARGET): $(OBJS)  
 $(CC) $(CFLAG) -o myprog main.o prime.o  
  
%.o: %.c  
 $(CC) $(CFLAG) -c -o $@ $<   
  
# 伪目标  用来清理，编译失败时通常会执行一边make clean，然后再次执行
clean:  
 rm $(OBJS) $(TAGET)  
# main.o: main.c  
# $(CC) $(CFLAG) -c -o $@ $<   
#   
# prime.o: prime.c  
# $(CC) $(CFLAG) -c -o $@ $<
```
