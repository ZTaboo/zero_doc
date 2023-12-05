> GMP是go的并发模型，G是Goroutine,M是thread（线程）Processor是调度器；其中P（调度器）和M（thread线程）是一对一的关系，


## CSP

CSP 模型是“以通信的方式来共享内存”，不同于传统的多线程通过共享内存来通信。用于描述两个独立的并发实体通过共享的通讯 channel (管道)进行通信的并发模型。

## GMP

G：Goroutine，实际上我们每次调用go func就是生成了一个 G。 P：Processor，处理器，一般 P 的数量就是处理器的核数，可以通过GOMAXPROCS进行修改。 M：Machine，系统线程。 在 GPM 模型，有一个全局队列（Global Queue）：存放等待运行的 G，还有一个 P 的本地队列：也是存放等待运行的 G，但数量有限，不超过 256 个。
