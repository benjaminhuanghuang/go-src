# 

Go源码阅读 Golang汇编(ASM)简单教程
https://www.bilibili.com/video/BV1u64y1M79t/?spm_id_from=333.788.videocard.5

Go 的 ASM 是一种中间码，有一部分是Go自定义的

## get asm code 有三种方法

针对 .go 生成汇编代码
```
  go tool compile -N -l -S once.go
```

反汇编编译后的 .o file
```
  go tool compile -N -l once.go,

  go tool objdump once.o
```


```
  go build -gcflags -S once.go
```


go tool compile 和 go build -gcflags -S 生成的是过程中的汇编，和最终的机器码的汇编可以通过go tool objdump生成。
https://golang.org/cmd/go/

```
  go build -gcflags -S .
```


Go 定义了4个虚拟寄存器
- FP  Frame pointer
- PC  Program counter，指向下一条指令
- SB  Static base pointer， 指向全局变量
- SP  stack pointer, 指向栈顶