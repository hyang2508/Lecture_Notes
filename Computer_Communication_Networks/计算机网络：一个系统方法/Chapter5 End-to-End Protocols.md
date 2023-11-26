demultiplexing: allowing multiple application processes on each host to share the network
# 5.1 Simple Demultiplexor (UDP)
UDP: The Internet's User Datagram Protocol
a process is really identified by a port on some particular host: a (port, host) pair.
the form of the address used to identify the target process: _indirectly_ identify each other using an abstract locator, usually called a _port_ 

There is no flow-control mechanism in UDP to tell the sender to slow down.
The basic UDP checksum algorithm is the same one used for IP—that is, it adds up a set of 16-bit words using ones' complement arithmetic and takes the ones' complement of the result.
The UDP checksum takes as input the UDP header, the contents of the message body, and something called the _pseudoheader_. The pseudoheader consists of three fields from the IP header—protocol number, source IP address, and destination IP address—plus the UDP length field.

# 5.2 Reliable Byte Stream (TCP)
a full-duplex protocol
a flow-control mechanism
a demultiplexing mechanism
- **Flow control** involves preventing senders from over-running the capacity of receivers. 
- **Congestion control** involves preventing too much data from being injected into the network, thereby causing switches or links to become overloaded. 
- Thus, flow control is an end-to-end issue, while congestion control is concerned with how hosts and networks interact.

## 5.2.1 End-to-End Issues
sliding window algorithm
the timeout mechanism that triggers retransmissions must be adaptive.
Certainly, the timeout for a point-to-point link must be a settable parameter, but it is not necessary to adapt this timer for a particular pair of nodes.
