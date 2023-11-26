---

---
---
Reading
# 1.5 Performance
the various factors that impact network performance
## 1.5.1 Bandwidth and Latency
two fundamental ways: *bandwidth* (throughput) and *latency* (delay)

-  bandwidth: two meaning
	- Hz: the range of signals that can be accommodated
	- communication link: bps, data rate
- throughput: the _measured performance_ of a system
	- e.g. because of various inefficiencies of implementation, a pair of nodes connected by a link with a bandwidth of 10 Mbps might achieve a throughput of only 2 Mbps.

- latency: 3 components
	- the speed-of-light propagation delay
		- vacuum: $3.0\times {10^8}\mathrm{m/s}$; copper cable: $2.3\times 10^{8} \mathrm{m/s}$; optical fiber: $2.0\times 10^8\mathrm{m /s}$
	- transmit a unit of data: a function of the network bandwidth and the size of the packet 
	- queuing delays

the total latency:
$$
\begin{align}
Latency&=Propagation + Transmit + Queue \\
Propagation&=Distance/SpeedOfLight \\
Transmit&=Size/Bandwidth
\end{align}
$$
this is a single link (as opposed to a whole network), then the Transmit and Queue terms are not relevant, and the latency corresponds to the propagation delay only.

## 1.5.2 Delay $\times$ Bandwidth Product
Example  Delay $\times$ Bandwidth Product: Wireless LAN, Satellite, Cross-country fiber

## 1.5.3 High-Speed Networks
What is the impact on network design of having infinite bandwidth available?
Think from a different perspective: what does it not change as BW increases? the speed of light. In other words, "high speed" does not mean that latency improves at the same rate as bandwidth.

$$
\begin{align}
Throughput&=TransferSize/TransferTime \\
TransferTime&=RTT+1 / Bandwidth \times TransferSize \\

\end{align}

$$
for example:
>fetch a 1-MB file across a 1-Gbps with a round-trip time of 100 ms
> $TransferTime=100ms + 1MB/1Gbps=108ms$
> $Throughput=1MB / 108ms=74.1Mbps$ It is not 1Gbps!

## 1.5.4 Application Requirements
The unstated assumption: application programs want as much bandwidth as the network can provide. However, some applications are able to state an upper limit on how much bandwidth they need.
for example
>Video applications: The compressed video does not flow at a constant rate, but varies with time according to factors.

jitter

# 2.1 Technology Landscape
link: wireless (Wi-Fi), Ethernet link, fiber optic link, cellular network, copper wire...
![](https://book.systemsapproach.org/_images/f02-01-9780123850591.png)


