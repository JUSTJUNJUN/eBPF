eBPF简介
    eBPF全称扩展的伯克利包过滤器技术，是对BPF（现称为cBPF）的扩展。
    cBPF由来：早期已通过捕获网卡驱动的包，并拷贝到用户空间进行流量统计与分析。在用户空间对包进行过滤，则需要将所有包从内核空间拷贝到用户空间，十分耗费CPU资源，因此提出BPF框架（即cBPF），将filter置于内核空间中，若包的特征符合filter的规则，再将数据包放至接收队列中。
    tcpdump就是基于cBPF实现，由libpcap库对过滤表达式进行解析，翻译为BPF指令集的过滤规则，然后创建raw/packet类型的套接字，将网卡设置为混杂模式。通过setsockopt函数将BPF字节码拷贝到内核，并attach到相关联的socket套接字上。早期BPF的应用场景非常局限，因此没有单独的编译器。
    eBPF出现：eBPF是对cBPF思想的实际扩充，想法是将用户空间的代码注入内核空间，内核是一个基于事件的系统，所有工作可以由事件来描述和执行，例如打开文件、执行CPU指令与收发网络数据包。eBPF发展为一个内核子系统，是一种灵活高效的类虚拟机组件，检测内核事件源，并触发执行对应的eBPF程序。校验器Verifier保证eBPF程序是安全合规的，防止注入导致系统崩溃的程序和并阻止程序的恶意行为，更贴切的说，eBPF是“通用目的执行引擎”。eBPF从内核3.18开始加入，eBPF会将加载的cBPF程序透明地转换成eBPF再执行。