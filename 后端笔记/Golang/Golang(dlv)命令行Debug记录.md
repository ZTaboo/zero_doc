## 工具

Gtihub地址：[github.com/derekparker/delve](http://github.com/derekparker/delve/)

Gitee地址：[https://gitee.com/zero_clown/delve](https://gitee.com/zero_clown/delve)
## 安装
```bash
go get github.com/derekparker/delve/cmd/dlv
```
> 注意这里如果是linux需要保证GOPATH的路径在profile中，否则查询不到此参数

## 启动参数
```bash
1）dlv attach pid：类似与gdb attach pid，可以对正在运行的进程直接进行调试(pid为进程号)。

2）dlv debug：运行dlv debug test.go会先编译go源文件，同时执行attach命令进入调试模式，该命令会在当前目录下生成一个名为debug的可执行二进制文件，退出调试模式会自动被删除。

3）dlv exec executable_file ：直接从二进制文件启动调试模式。如果要带参数执行需要添加--，如dlv exec executable_file -- -f xxx.conf

4）dlv core executable_file core_file：以core文件启动调试，通常进行dlv的目的就是为了找出可执行文件core的原因，通过core文件可直接找出具体进程异常的信息。
```
## 调试参数
```bash
break [name:line] / b [name:line]：设置断点，当需要设置多个断点时，为了断点可识别可进行自定义命名。进入调试模式后先打断点，b test.go:14，然后执行c（类似于gdb的run）运行到断点处。

bp：查看所有断点。

on  ：当运行到某断点时执行相应命令，断点可以是名称（在设置断点时可命名断点）或者编号，例如on 1 p a表示运行到断点1时打印变量a。

condition   ：有条件的中断断点，当expression为true时指定的断点将被中断，程序将继续执行。

next|step：逐行执行代码，区别和gdb类似，next遇到函数调用时不会进入该函数，step则会进入函数，如果需要查看函数的具体执行过程则用step，否则用next，调试过程中一般这两个命令会结合使用，对于用户自定义的函数可能需要进入函数内部查看每步执行情况，对于系统函数则没有必要。

step-instruction |si：单步单核执行代码，如果不希望多协程并发执行可以使用该命令，这在多协程调试时极为方便。

stepout：当使用s命令进入某个函数后又想退出时可用此命令。

args：查看函数参数，调试时可以用此命令查看被调用函数所传入的参数值。

locals：查看所有局部变量，locals var_name：查看具体某个变量，var_name可以是正则表达式。

clear：清除单个断点；clearall：清除所有断点。

list：打印当前断点位置的源代码，list后面加行号可以展示该行附近的源代码，如list test.go:10将会展示test.go文件第10行附近的代码，值得注意的是该行必须是代码行而不能是空行。

bt：打印当前栈信息。

frame：切换栈。

goroutines：显示所有协程。编号为1的是主协程，编号为5、6的为自己启动的协程。

regs：打印寄存器内容。

goroutine：协程切换。dlv默认会一直在主协程上执，为打印Func函数中的临时变量a，需要进行协程切换，先执行

sources：打印所有源代码文件路径。

source：执行一个含有dlv命令的文件，source命令允许将dlv命令放在一个文件中，然后逐行执行文件内的命令。

threads：显示所有线程，thread：线程切换，这两个命令与goroutines、goroutine类似，不过在go语言中一般很少使用。

trace：类似于打断点，但程序运行到该处时不会中断而是继续执行，同时会输出一行提示信息。
```
## dlv输出内容不完整问题

`config -list` 命令查看全部配置

`config max-string-len 99999` 设置长度
