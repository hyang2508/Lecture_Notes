two important metrics used to characterize network performance: *round-trip time* (RTT) and *throughput* 
# 1 Introduction: A Tale of Two Tools
## ping
effect: estimate the **latency** or checking whether nodes are responsive at all
ping uses Internet Control Message Protocal (ICMP) messages to measure the RTT
## iperf
a userspace program used to estimate traffic **throughput** (using either TCP or UDP)