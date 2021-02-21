初始化要完成: 命令⾏参数整理，环境变量设置，以及内存分配器、垃圾回收器和并发调度器的⼯作现场准备

初始化代码 在 asm_amd64.s 中被调用
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

利用 breakpoint 找出所在源文件
```
  (gdb) b runtime.args
  Breakpoint 7 at 0x42ebf0: file /usr/local/go/src/runtime/runtime1.go, line 48.
  
  (gdb) b runtime.osinit
  Breakpoint 8 at 0x41e9d0: file /usr/local/go/src/runtime/os1_linux.go, line 172.
  
  (gdb) b runtime.schedinit
  Breakpoint 9 at 0x424590: file /usr/local/go/src/runtime/proc1.go, line 40.
```