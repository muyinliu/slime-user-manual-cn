二、开始
========

本章告诉你如何配置和启动SLIME。

2.1 支持平台
------------

SLIME广泛地支持多种操作系统和Lisp实现。SLIME可以在类Unix系统、Mac OSX和Microsoft Windows上运行。GNU Emacs 21、22和23以及XEmacs 21都可以运行SLIME。

粗略地根据支持的良好程度来排序的话，所有支持的Lisp实现为：

* CMU Common Lisp（CMUCL），19d版或更新
* Steel Bank Common Lisp（SBCL），1.0版或更新
* Clozure Common Lisp（CCL），1.3版或更新
* LispWorks，4.3版或更新
* Allegro Common Lisp（ACL），6版或更新
* CLISP，2.35版或更新
* Armed Bear Common Lisp（ABCL）
* Corman Common Lisp，2.51版或更新，需要 `http://www.grumblesmurf.org/lisp/corman-patches <http://www.grumblesmurf.org/lisp/corman-patches>`_ 的补丁
* Scieneer Common Lisp（SCL），1.2.7版或更新
* Embedded Common Lisp（ECL）

绝大部分特性在不同实现上的表现都是一致的，但是有些可能会有所不同。这些包括放置编译信息的注释的精度、XREF支持以及调试器命令（例如“重启缓冲区”）。

2.2 下载SLIME
-------------

你可以选择使用发行版本的SLIME或者直接通过CVS仓库使用SLIME。你可以从我们的网站下载最新发布版本： `http://www.common-lisp.net/project/slime/ <http://www.common-lisp.net/project/slime/>`_ 。

我们建议加入了slime-dev邮件列表的用户使用CVS版本的代码。

2.2.1 从CVS下载
^^^^^^^^^^^^^^^

可以从common-lisp.net的CVS仓库取得SLIME。你可以选择使用最新版本的代码或者是带有FAIRLY-STABLE标签的快照版本。

跟FAIRLY-STABLE版本的代码相比，最新版本的代码可能有更多的特性和更少的BUG，但也有可能因为较大的改动而不稳定。根据经验法则，我们建议如果你加入了slime-dev邮件列表，你最好使用最新版本（当进行主要的变动时，我们会发出通告）。如果你没有加入邮件列表，你就无法得知最新版本代码的情况，所以使用FAIRLY-STABLE或者发布版本是一个安全的选择。

如果你从CVS迁出代码，记得经常更新。经常会有小的改进提交上去，而FAIRLY-STABLE标签也会随时推进。

2.2.2 使用CVS
^^^^^^^^^^^^^

要下载SLIME你要先配置你的CVSROOT并且登录到仓库。

::

   export CVSROOT=:pserver:anonymous@common-lisp.net:/project/slime/cvsroot
   cvs login

最新版的代码可以通过以下方式迁出：

::

   cvs checkout slime

或者可以通过以下方式迁出FAIRLY-STABLE版本：

::

   cvs checkout -rFAIRLY-STABLE slime

如果你想知道最新版本的代码跟你运行的版本有什么新的东西，你可以将本地的ChangeLog和版本仓库的进行对比：

::

   cvs diff -rHEAD ChangeLog      # or: -rFAIRLY-STABLE

2.3 安装
--------

如果你已经有了一个可以从命令行启动的Lisp实现，那么仅需在.emacs文件中添加几行即可安装成功：

::

   (setq inferior-lisp-program "/opt/sbcl/bin/sbcl") ; your Lisp system
   (add-to-list 'load-path "~/hacking/lisp/slime/")  ; your SLIME directory
   (require 'slime)
   (slime-setup)

在README文件里也可以见到上面这些代码。你可以从那里复制粘贴，但要记得替换正确的路径。

这是没有其它杂项的最小化配置。如果基本配置可以工作，那么你可以试附加模块。（8.1 加载扩展包）

我们建议如果你要使用SLIME就不要在Emacs里加载ILISP包。如果你这么做了，那么在编辑Lisp源文件时就会加入许多键绑定，而且这些键绑定可能会跟SLIME启动的Lisp进程发生冲突而无法正常工作。

2.4 启动SLIME
-------------

Emacs命令M-x slime可以启动SLIME。它使用inferior-lisp包来启动一个Lisp进程，加载并启动Lisp端服务器（叫做“Swank”），然后在Emacs和Lisp之间建立一个socket连接。最后会生成一个REPL缓冲区，你可以在这里输入Lisp表达式并求值。

于是现在SLIME启动完成，你可以开始使用了。

2.5 调整设置
------------

这一部分说明了如何减少SLIME的启动时间和如何为多Lisp系统配置SLIME。

在进行本部分之前请确认你的基本配置已经可以工作。如果你对基本配置感到满意，那么请跳过这部分。

关于附加模块请看“8.1 加载扩展包”。

2.5.1 自动加载
^^^^^^^^^^^^^^

基本设置始终会加载SLIME，即使你不使用它。如果你只在需要的时候才加载SLIME，那么Emacs会启动的快一点。要这样，你需要稍微更改你的.emacs文件：

::

   (setq inferior-lisp-program "the path to your Lisp system")
   (add-to-list 'load-path "the path of your slime directory")
   (require 'slime-autoloads)
   (slime-setup)

跟基本配置相比，差别只在这一行(require 'slime-autoloads)。它告诉Emacs当M-x slime或者M-x slime-connect命令第一次执行之后SLIME的其它部分会被自动加载。

2.5.2 多种Lisp
^^^^^^^^^^^^^^

默认情况下，M-x slime命令启动的程序是由inferior-lisp-program指定的。如果你在执行M-x slime命令时添加了一个前缀参数，Emacs会启动参数中指定的程序。如果你需要经常使用它或者命令的名称太长，那么在.emacs文件里设置slime-lisp-implementations变量则较为方便。例如，在这里我们定义了两个程序：

::

   (setq slime-lisp-implementations
         '((cmucl ("cmucl" "-quiet"))
           (sbcl ("/opt/sbcl/bin/sbcl") :coding-system utf-8-unix)))


这个变量包含了一个Lisp程序的列表，如果你通过一个减号前缀参数启动SLIME，M-- M-x sliem，你可以从这个列表里选择一个程序。当不加前缀地启动该命令，slime-default-lisp变量里指定的程序或者是列表中的第一项会被使用。列表的元素应该像这样：

::

   (NAME (PROGRAM PROGRAM-ARGS...) &key CODING-SYSTEM INIT INIT-FUNCTION ENV)


* NAME
  是一个符号，用来指定Lisp程序
* PROGRAM
  是程序的文件名。注意文件名可以包含空格。
* PROGRAM*ARGS
  是一个命令行参数的列表。
* CODING*SYSTEM
  指定了连接的编码系统（见6.1 Emacs端 slime*net*coding*system）。
* INIT
  应该是一个接受两个参数的函数：一个文件名和一个字符编码。这个函数应该返回一个字符串格式的Lisp表达式，来指导Lisp启动Swank服务器并且将端口号写入文件。启动时，SLIME启动一个Lisp进程并将此函数的结果发送给Lisp的标准输入。默认情况下，slime*init*command会被使用。“2.5.3 更快地加载Swank”里有一个例子。
* INIT*FUNCTION
  应该是一个不接受参数的函数。连接建立之后它会被调用。（见 6.1.1 钩子 slime*connected*hook）
* ENV
  一个为子进程指定环境变量的列表。例如：

  ::

     (sbcl-cvs ("/home/me/sbcl-cvs/src/runtime/sbcl"
	            "--core" "/home/me/sbcl-cvs/output/sbcl.core") :env ("SBCL_HOME=/home/me/sbcl-cvs/contrib/"))

在子进程中初始化SBCL_HOME。

2.5.3 更快地加载Swank
^^^^^^^^^^^^^^^^^^^^^

对于SBCL，我们建议你新建一个有socket支持和POSIX绑定的核心配置文件，因为这些模块加载起来很耗时。为了新建一个这样的核心，执行以下的命令：

::

   shell$ sbcl
   (mapc 'require '(sb-bsd-sockets sb-posix sb-introspect sb-cltl2 asdf))
   (save-lisp-and-die "sbcl.core-for-slime")

然后，在你的.emacs文件里加入如下代码：

::

   (setq slime-lisp-implementations
         '((sbcl ("sbcl" "--core" "sbcl.core-for-slime"))))

为了最大化启动速度，你可以在核心文件里直接包含Swank服务器。这样做的缺点是设置的时候比较麻烦，并且当你想升级你的SLIME或者SBCL的时候你要新建一个核心文件。这样做的步骤是：

::

   shell$ sbcl
   (load ".../slime/swank-loader.lisp")
   (swank-loader:dump-image "sbcl.core-with-swank")

然后在.emacs里加入如下代码：

::

   (setq slime-lisp-implementations
         '((sbcl ("sbcl" "--core" "sbcl.core-with-swank")
                 :init (lambda (port-file _)
	             (format "(swank:start-server %S)\n" port-file)))))

类似的配置对其它Lisp实现也适用。
