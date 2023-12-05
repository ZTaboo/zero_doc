## 介绍

> Process包用于操作进程，销毁，查看，搜索等。主要依赖`Processes` 和`NewProcess` 函数返回的`Processes`结构体方法来操作。


## Pids函数

### 介绍

> 获取系统当前运行的进程pid

> 返回参数：`[]int32`、`error`


### 示例

```go
func main() {
	pids, err := process.Pids()
	if err != nil {
		return
	}
	fmt.Println(pids)
}
//output:[1 2 3 4 5 7 9 10 11 12 13 14 15 16 18 19 20 21 22 24 25 26 27 28 30 31 32 33 34 36 37]
```

## Processes函数

### 介绍

> 此函数返回一个`[]Process`和`error`，可遍历操作每个或你需要的pid的进程，详细每个方法作用可查看  [Processes方法](#processes%E6%96%B9%E6%B3%95)


### 示例

```go
package main

import (
	"fmt"
	"github.com/shirou/gopsutil/v3/process"
)

func main() {
	processes, err := process.Processes()
	if err != nil {
		return
	}
	for _, item := range processes {
		name, err := item.Name()
		if err != nil {
			return
		}
		fmt.Println("pid:", item.Pid, "name:", name)
	}
}
//output:
pid: 5027 name: Docker Desktop
pid: 5028 name: Docker Desktop
pid: 5031 name: virtiofsd
pid: 5036 name: Docker Desktop
pid: 5048 name: qemu-system-x86_64
pid: 5052 name: kvm-nx-lpage-re
pid: 5056 name: kvm-pit/5048
```

## PidExists函数

### 介绍

> 检测指定pid是否存在


### 示例

```go
package main

import (
	"fmt"
	"github.com/shirou/gopsutil/v3/process"
)

func main() {
	exists, err := process.PidExists(23713)
	if err != nil {
		fmt.Println("err1:", err)
	}
	fmt.Println("stat:", exists)
}
//output:true(存在)or false(pid不存在)
```

## NewProcess函数

### 介绍

> NewProcess 创建一个新的 `Process` 实例，需要指定`pid`并检查该进程是否存在，如果进程不存在，将返回错误。可以使用`Process`上的其他方法获取有关进程的详细信息。


### 示例

```go
//输入一个存在的pid
package main

import (
	"fmt"
	"github.com/shirou/gopsutil/v3/process"
)

func main() {
	newProcess, err := process.NewProcess(11)
	if err != nil {
		fmt.Println("err:", err)
	}
	name, err := newProcess.Name()
	if err != nil {
		fmt.Println("err2:", err)
	}
	fmt.Println(newProcess.Pid, name)
}
//output:
11 rcu_tasks_rude_
//输入一个不存在的pid
package main

import (
	"fmt"
	"github.com/shirou/gopsutil/v3/process"
)

func main() {
	newProcess, err := process.NewProcess(9999)
	if err != nil {
		fmt.Println("err:", err)
	}
	name, err := newProcess.Name()
	if err != nil {
		fmt.Println("err2:", err)
	}
	fmt.Println(newProcess.Pid, name)
}
//output:
err: process does not exist
err2: open /proc/9999/status: no such file or directory
9999
```

### Processes方法

### 介绍

> 通过`Processes`和`NewProcess` 函数可以得到一个结构体方法，内部私有很多信息，如：name，status，parent等，这些我们都用不到，只需要用相应的方法即可，下面会介绍全部方法的作用


### 示例

```go
package main

import (
	"fmt"
	"github.com/shirou/gopsutil/v3/process"
)

func main() {
	newProcess, err := process.NewProcess(11)
	if err != nil {
		fmt.Println("err:", err)
	}
	ppid, err := newProcess.Ppid()
	if err != nil {
		fmt.Println("err2:", err)
	}
	fmt.Printf("pid:%d,ppid:%d", newProcess.Pid, ppid)
}
//output:pid:11,ppid:2
```

> 这里我们测试了获取一个pid获取的ppid信息，下面介绍每个方法


#### ppid

> `ppid`返回进程的父进程ID


#### Background

> `Background`如果进程在后台，则Background返回true，否则返回false。


#### Percent

> `Percent`获取进程的`cpu`占用


#### IsRunning

> `IsRunning`返回进程是否仍在运行。


#### CreateTime

> `CreateTime`返回进程的创建时间（以UTC为单位，以毫秒为单位）。


#### MemoryPercent

> `MemoryPercent` 返回此进程使用的总 RAM 的百分比


#### CPUPercent

> `CPUPercent`返回此进程使用的 CPU 时间的百分比


#### Groups

> `Groups`返回进程的所属组 ID（包括补充组）作为 int 的一部分


> group知识：`Linux` 上的用户属于主组，该组在 `/etc/passwd` 文件中指定，并且可以分配给多个补充组，这些补充组在 `/etc/group` 文件中是特定的。创建给用户后，可以使用 `usermod` 命令将它们分配给其他组。


#### Name

> `Name`返回进程的名称


#### Exe

> `Exe`返回进程的可执行路径


#### Cmdline

> `Cmdline`以字符串形式返回进程的命令行参数，每个参数由 0x20 ASCII 字符分隔。


#### Cwd

> `Cwd`返回进程的当前执行目录


#### Parent

> `Parent` 返回进程的父进程pid


#### Status

> `Status` 返回进程状态

> 返回值可以是以下其中之一

> R: `Running`（运行中） S: `Sleep`（休眠中） T: `Stop`（已停止） I: `Idle`（空闲中）

> Z: `Zombie`（僵尸进程） W: `Wait`（等待中） L: `Lock`（锁定）


#### Foreground

> `Foreground`如果进程在前台，则返回true，否则返回false。


#### Uids

> `Uids`返回进程的用户id


#### Gids

> `Gids`返回group id


#### Terminal

> `Terminal`返回与终端关联的终端名称


#### Nice

> `Nice` 返回一个优先级数值


#### IOnice

> `IOnice`返回i/o的优先级数值


#### Rlimit

> `Rlimit`返回资源限制（这里没有搞懂，linux下通过`cat /proc/{pid}/limits`查看资源限制，但并没找到相关信息）


#### IOCounters

> `IOCounters`s返回进程的IO计数信息


#### NumCtxSwitches

> `NumCtxSwitches`返回进程的上下文开关数。


#### NumFDs

> `NumFDs` 返回进程使用的文件描述符数；

> 知识补充：进程所打开的每个文件都有一个符号连接在该进程的子目录里, 以文件描述符命名, 这个名字实际上是指向真正的文件的符号连接


#### NumThreads

> `NumThreads` 返回进程使用的线程数


#### Threads

> `Threads`返回该进程和其子进程（我没有验证是否是子进程，猜测是如此。）的cpu时间使用率，可参考 [CpuTimesstat](/docs/gopsutil/cpu#timesstat)参数详解


#### Times

> `Times` 返回进程的时间使用率，可参考[CpuTimesstat](/docs/gopsutil/cpu#timesstat)


#### TODO：CPUAffinity

> `CPUAffinity`返回cpu的相关性，但作者尚未完善此方法。


#### MemoryInfo

> `MemoryInfo`返回进程的内存信息，单位是：botes


```go
type MemoryInfoStat struct {
	RSS    uint64 `json:"rss"`    // Resident Set Size 实际使用物理内存
	VMS    uint64 `json:"vms"`    // 虚拟内存
	HWM    uint64 `json:"hwm"`    // bytes
	Data   uint64 `json:"data"`   // bytes
	Stack  uint64 `json:"stack"`  // bytes
	Locked uint64 `json:"locked"` // bytes
	Swap   uint64 `json:"swap"`   // bytes
}
```

#### MemoryInfoEx

> `MemoryInfoEx`根据不同操作系统返回不同的内存信息


#### PageFaults

> `PageFaults`返回进程的页面错误计数器


#### Children

> `Children`返回指向 `Process` 类型的指针切片，可使用`Process`方法操作所有子进程


#### OpenFiles

> `OpenFiles`返回由当前进程打开的文件信息列表

> `OpenFilesStat`参数包括文件路径和文件描述符。关于文件描述符在：`/proc/{pid}/fd`内（Linux）


#### Connections

> `Connections`返回进程的网络连接状态信息，会返回所有的连接信息，包括TCP、UDP、或unix


#### ConnectionsMax

> `ConnectionsMax`输入数值限制返回的网络连接信息最大切片长度


#### MemoryMaps

> `MemoryMaps`获取进程的内存映射，linux下可查看`/proc/(pid)/smaps`信息

> 此方法需要一个`grouped`（bool类型）参数，如果为true的话：如果存在smaps_rollup（需要内核 >= 4.15），那么将使用它来预汇总进程的内存信息。


#### Tgid

> `Tgid`返回进程的线程组 ID。


#### SendSignal

> `SendSignal`向进程发送一个`Signal`信号，可参考：[Signal信号统计](/docs/%E7%B3%BB%E7%BB%9F%E6%9D%82%E8%AE%B0/Signal%E4%BF%A1%E5%8F%B7%E8%AE%B0%E5%BD%95)


#### Suspend

> `Suspend`向进程发送`SIGSTOP`信号（停止进程(不能被捕获、阻塞或忽略)）


#### Resume

> `Resume`向进程发送`Resume`信号（继续执行已经停止的进程(不能被阻塞)）


#### Terminate

> `Terminate`向进程发送`SIGTERM`信号（结束程序(可以被捕获、阻塞或忽略)）


#### Kill

> `Kill`向进程发送`SIGKILL`信号（无条件结束程序(不能被捕获、阻塞或忽略)）


#### Username

> `Username`返回进程的用户名


#### Environ

> `Environ`返回进程的环境变量列表

