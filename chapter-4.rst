四、SLDB：Slime调试器
==================

slime有一个自己的基于Emacs的调试器，SLDB。Lisp系统里的状况（Condition）发出的信号会在Emacs里通过Lisp符号*DEBUGGER-HOOK*触发SLDB。

当有状况发出信号时，SLDB会生成一个新的缓冲区。这个缓冲区会显示对状况的描述、一系列重启选项和调用栈。可以通过提供的命令来出发重启、检查调用栈和在堆栈调用窗口里移动。

4.1 检查窗口
----------

用来查看光标处的堆栈调用窗口的命令。

* t 或 M-x sldb-toggle-details

  出发显示本地变量和CATCH标签

* v 或M-x sldb-show-source

  查看窗口处的当前代码表达式。此表达式会被展现在Lisp源代码的缓冲区里。

* e 或 M-x sldb-eval-in-frame

  对窗口里的表达式求值。表达式可以引用窗口里可用的局部变量。

* d 或 M-x sldb-pprint-eval-in-frame
  
  对窗口里的表达式求值，并且在一个临时缓冲区里打印出来。

* D 或 M-x sldb-disassemble

  拆解窗口处的函数。包括例如窗口对指令指针等信息。

* i 或 M-x sldb-inspect-in-frame

  查看对窗口里一个函数求值后的结果。

* C-c C-c 或 M-x sldb-recompile-frame-source

  重新编译窗口。C-u C-c C-c 是以最大调试模式重新编译。

4.2 重启
----------

* a 或 M-x sldb-abort

  触发ABORT重启。

* q 或 M-x sldb-quit

  “QUIT”——THROW一个top-level的Slime请求循环可以捕获的标签。

* c 或 M-x sldb-continue

  触发CONTINUE重启。

* 0 ... 9

  通过数字触发重启。

重启也可以通过在缓冲区里按RET或者Mouse-2来触发。

4.3 在不同的窗口间操作
----------

* n 或 M-x sldb-down， p 或 M-x sldb-up

  在不同窗口间移动。

* M-n 或 M-x sldb-details-down， M-p 或 M-x sldb-details-up

  有技巧地在不同窗口间移动：隐藏之前窗口的详细信息，显示下一个窗口的详细信息和源代码。这些小技巧让你只看到当前窗口的详细信息和源代码。

* > 或 M-x sldb-end-of-backtrace

  获取所有的堆栈调用并且到最后一个窗口。

* < 或 M-x sldb-beginning-of-backtrace 

  到第一个窗口。

4.4 单步调试
----------

单步调试并非在所有的Lisp实现里都是可用的，在那些可用的Lisp实现中，它的表现也很不同。

* s 或 M-x sldb-step

  单步运行窗口中的下一个表达式。对CMUCL来说，即时在从当前位置可到达的所有代码的位置上设置断点。

* x 或 M-x sldb-next

  单步运行函数里的下一个形式。

* o 或 M-x sldb-out

  暂时停止单步调试，当前函数一返回则恢复单步调试。

4.5 其它命令
----------

* r 或 M-x sldb-restart-frame

  以当前窗口最先运行时的参数为参数重启。（此命令并非在所有Lisp实现里都可用）

* R 或 M-x sldb-return-from-frame

  在minibuffer里输入一个值，并以这个值为返回值回到窗口。（此命令并非在所有Lisp实现里都可用）

* B 或 M-x sldb-break-with-default-debugger

  退出SLDB并且以Lisp默认的调试器调试状况。

* C 或 M-x sldb-inspect-condition

  查看当前正在调试的状况。

* : 或 M-x slime-interactive-eval

  在minibuffer里输入一个表达式并求值。
