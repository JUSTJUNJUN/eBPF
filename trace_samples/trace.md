## tracex5
- 功能：系统调用监视器，write()、read()被调用时打印相关信息。
- 说明：动态插桩点为openat2，使用宏PT_REGS_PARM1和PT_REGS_PARM2获取openat2的前两个参数。
- eBPF Loader：bpf_load()
- eBPF helper：bpf_trace_printk()、bpf_tail_call()
