https://www.itranslater.com/qa/details/2582567851114628096

https://lldb.llvm.org/use/map.html


## LLDB的常用命令
1> po:打印对象,会调用对象 description 方法。是 print-object 的简写；命令po跟p很像，p输出的是基本类型，po输出的Objective-C对象。调试器会输出这个 object 的 description。

2> expr:可以在调试时动态执行指定表达式,并将结果打印出来,很有用的命令

3> print:也是打印命令,需要指定类型

4> bt:打印调用堆栈,是 thread backtrace 的简写,加 all 可打印所有thread 的堆栈

5> br l:是 breakpoint list 的简写

6> n:是换行

7> p:是打印这个对象所属的类,即其父类
