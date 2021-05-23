# Network Layer  

### 1. Introduction

##### **Main** **functions**

*   transport segment from sending to receiving host
*    on sending side
    *   encapsulates segments into datagrams
*   on receiving side
    *   delivers segments to transport layer
*   network layer protocols
    in every host, router
*   router examines header fields in all IP datagrams passing through it  



#### 1.1 Forwarding and Routing (转发和路由选择) 

##### **Two key network-layer functions**  

*   **Forwarding** refers to the router-local action of transferring a packet from an input link interface to the appropriate output link interface. (将分组从输入链路移动至合适的输出链路)
*   **Routing** refers to the network-wide process that determines the end-to-end paths that packets take from source to destination. (找路径)

*analogy:*

*   **routing**: process of planning trip from source to destination
*   **forwarding**: process of getting through single interchange



##### **Forwarding table (转发表)**

<img src="imgs\forwarding_table.png" alt="forwarding_table" style="zoom:67%;" />



#### 1.2 Connection Setup (建立连接)

*   important function in some network architectures:
    *   ATM (Asynchronous Transfer Mode) from telecom world
    *   not in the Internet
*   before datagrams flow, two end hosts and intervening routers establish virtual connection
    *   routers get involved
    *   must also keep state for each connection
    *   not in the Internet



#### 1.3 Network Service Model

*   Guaranteed delivery
*   Guaranteed delivery with bounded delay
*   In-order packet delivery
*   Guaranteed minimal bandwidth
*   Guaranteed maximum jitter
*   Security services



### 2 Virtual Circuit and Datagram Networks (虚拟电路和数据报网络)

Computer networks that provide only a **connection service** at the network layer are called **virtual-circuit (VC) networks**; computer networks that provide only a **connectionless service** at the network layer are called **datagram networks.**   



#### 2.1 Virtual Circuit

While the Internet is a datagram network, many alternative network architectures use connections at the network layer. These network-layer connections are called **virtual circuits (VCs)**.  

**A VC consists of:**

1.  **a path** (that is, a series of links and routers) between the source and destination hosts

2.  **VC numbers**, one number for each link along the path

3.  **entries** in the forwarding table in each router along the path (沿着该路径的每台路由器的转发表的表项)

    A packet belonging to a virtual circuit will carry a VC number in its header. Because a virtual circuit may have a different VC number on each link, each intervening router (中间路由器) must replace the VC number of each traversing packet with a new VC number. The new VC number is obtained from the forwarding table.



#### 2.2 Datagram Networks

In a datagram network, each time an end system wants to send a packet, it stamps the packet with the address of the destination end system and then pops the packet into the network (加上目的端系统的地址).



##### 2.2.1 Datagram forwarding table  

As a packet is transmitted from source to destination, it passes through a series of routers. Each of these routers uses the packet’s destination address to forward the packet. 

Specifically, each router has a forwarding table that maps destination addresses to link interfaces; when a packet arrives at the router, the router uses the packet’s destination address to look up the appropriate output link interface in the forwarding table. The router then intentionally forwards the packet to that output link interface.  

<img src="imgs\forwarding_table.png" alt="forwarding_table" style="zoom:67%;" />

**Longest prefix matching** (最长前缀匹配原则)  

when looking for forwarding table entry for given destination address, use **longest address prefix** that matches destination address.



### 3. The Internet Protocol (IP) 网际协议

<img src="imgs\The_Internet_network_layer.png" alt="The_Internet_network_layer" style="zoom:67%;" />

**Internet’s network layer has three major components.** 

1.  IP protocol 

2.  Routing component, which determines the path a datagram follows from source to destination. 

    Routing protocols compute the forwarding tables that are used to forward packets through the network.  

3.  A facility (ICMP protocol) to report errors in datagrams and respond to requests for certain network-layer information



#### 3.1 Datagram Format 数据报格式

**Take IPv4 datagram format for example:**

<img src="imgs\Datagram_Format.png" alt="Datagram_Format" style="zoom: 80%;" />

**The key fields in the IPv4 datagram:**

*   **Version number (4 bits) 版本号**

    specify the IP protocol version of the datagram

    By looking at the version number, the router can determine how to interpret the remainder of the IP datagram.   

*   **Header length (bytes) 首部长度**

*   **Type of service**  

*   **Datagram length**  

    This is the total length of the IP datagram (header plus data), measured in bytes. 

    Since this field is 16 bits long, the theoretical maximum size of the IP datagram is 65,535 bytes.

*   **Identifier, flags, fragmentation offset**  

*   **Time-to-live**  

*   **Protocol**  

    The value of this field indicates the specific transport-layer protocol to which the data portion of this IP datagram should be passed.   

*   **Header checksum**

    aids a router in detecting bit errors in a received IP datagram

*   **Source and destination IP addresses**  

*   **Options**  

    The options fields allow an IP header to be extended. IP options were dropped in the IPv6 header.  

*   **Data (payload)**  

    In most circumstances, the data field of the IP datagram contains the transport-layer segment (TCP or UDP) to be delivered to the destination.



#### 3.2 IP Datagram Fragmentation, Reassembly (IP数据报分片组装)

*   network links have MTU (max.transfer unit), largest possible link-level frame
    *   different link types, different MTUs
*   large IP datagram divided (“fragmented”) within net
    *   one datagram becomes several datagrams
    *   “reassembled” only at final destination
    *   IP header bits used to identify and order fragments to reassemble  

**Example:**

<img src="imgs\eg_IP_Datagram_Fragmentation_Reassembly_cn.png" alt="eg_IP_Datagram_Fragmentation_Reassembly_cn" style="zoom: 80%;" />



<img src="imgs\eg_IP_Datagram_Fragmentation_Reassembly_en.png" alt="eg_IP_Datagram_Fragmentation_Reassembly_en" style="zoom: 67%;" />



At the destination, the payload of the datagram is passed to the transport layer only after the IP layer has fully reconstructed the original IP datagram. If one or more of the fragments does not arrive at the destination, the incomplete datagram is discarded and not passed to the transport layer. 

However, if TCP is being used at the transport layer, then TCP will recover from this loss by having the source retransmit the data in the original datagram.  



#### 3.3 IPv4 Addressing  编址

