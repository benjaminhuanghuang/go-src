
编译好的可执⾏⽂件真正执⾏⼊⼜并⾮我们所写的 main.main 函数，因为编译器
总是会插⼊⼀段引导代码，完成诸如命令⾏参数、运⾏时初始化等⼯作，然后才会进⼊⽤户逻辑

可以debug 一个go程序来发现整个运行环境的入口

关闭编译器代码优化和函数内联，避免断点和单步执⾏⽆法准确对应源码⾏，避免⼩函数和局部变量被优化掉。
```
  go build -gcflags "-N -l" -o test test.go
```

Break到真正的⼊口地址，然后利⽤断点命令就可以找到⽬标源⽂件信息

## gdb 调试 on Linux
```
  gdb test
```
查看信息
```
  (gdb) info files
  Entry point: 0x?????
```
通过打印断点信息，找出 entry 所在源文件
```
(gdb) b *0x44dd00
Breakpoint 1 at 0x44dd00: file /usr/local/go/src/runtime/rt0_linux_amd64.s, line 8.
```

打开 src/runtime/rt0_linux_amd64.s line 8, 可以看到
```
  MOVQ $runtime·rt0_go(SB), AX
```
在 runtime.rt0_go 上设置断点
```
(gdb) b runtime.rt0_go
Breakpoint 2 at 0x44a780: file /usr/local/go/src/runtime/asm_amd64.s, line 12.
```
可以发现runtime.rt0_go 在 asm_amd64.s 中的代码
```
TEXT runtime·rt0_go(SB),NOSPLIT,$0
  ...
  // -- Init
  CALL runtime·args(SB)
  CALL runtime·osinit(SB)
  CALL runtime·schedinit(SB)
  // -- Create main goroutine for runtime.main
  MOVQ $runtime·mainPC(SB), AX
  PUSHQ AX
  PUSHQ $0
  CALL runtime·newproc(SB)
  POPQ AX
  POPQ AX
  // -- Execute main goroutine
  CALL runtime·mstart(SB)
  RET
DATA runtime·mainPC+0(SB)/8,$runtime·main(SB)
GLOBL runtime·mainPC(SB),RODATA,$8
```
⾄此，由汇编针对特定平台实现的引导过程就全部完成。后续内容基本上都是由 Golang 代码实现。
```
(gdb) b runtime.main
Breakpoint 3 at 0x423250: file /usr/local/go/src/runtime/proc.go, line 28.
```
