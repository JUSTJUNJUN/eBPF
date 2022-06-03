# 关于eBPF

eBPF全称扩展的伯克利包过滤器技术，是对BPF（现称为cBPF）的扩展。

## cBPF由来

- 在用户空间进行数据包过滤（packet filter），需要将数据包从内核空间拷贝到用户空间，十分耗费CPU资源，因此提出BPF框架（即cBPF），将filter逻辑置于内核空间中，若符合filter的规则，才将数据包放至接收队列中，减少数据包拷贝发生的资源消耗。
- tcpdump/wireshark等网络监控领域的软件工具基于cBPF实现，编写BPF指令集的过滤规则，然后创建raw/packet类型的套接字，将网卡设置为混杂模式，通过setsockopt函数将BPF字节码拷贝到内核，并attach到相关联的socket套接字上。
- tcpdump命令行参数中使用的过滤表达式使用libpcap库进行解析，并翻译为BPF指令。BPF指令集数量少，应用场景不多，因此无单独的编译器。


## eBPF出现

- 在内核中安全得运行用户程序被证实为是一个创造性的设计决策，cBPF最初是用于提高网络报文的过滤效率，经过重新设计，不再局限于网络协议栈，eBPF已演变为基于代码注入技术的通用目执行引擎。eBPF是内核中一个灵活高效的类虚拟机组件，由内核事件源触发执行对应的eBPF程序。
- 从kernel3.18引入eBPF，cBPF程序指令将会被透明得转换成eBPF程序指令再执行。

## eBPF对cBPF的扩充

	• 增加了寄存器数量，扩展寄存器的宽度，配有栈空间及MAP，支持复杂的程序流程设计。
	• 提供帮助函数（BPF_CALL）调用内核功能，资源消耗小。
	• 支持附着在更多的代码路径上。
	• eBPF指令集更容易映射到本地机器指令，支持JIT，提升程序的运行性能。

## eBPF in kernel_src_4.19.136

	• ./drivers/net/ethernet/netronome/nfp/bpf
	• ./tools/lib/bpf（libbpf）
	• ./tools/bpf（bpftool）
	• ./tools/testing/selftests/tc-testing/bpf
	• ./tools/testing/selftests/bpf
	• ./tools/perf/include/bpf
	• ./tools/perf/examples/bpf
	• ./kernel/bpf（syscall）
	• ./Documentation/bpf
	• ./samples/bpf（示例代码）
	• ./net/bpf
