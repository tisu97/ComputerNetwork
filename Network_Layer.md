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

    The time-to-live (**TTL**) field is included to ensure that datagrams do not circulate forever (due to, for example, a long-lived routing loop) in the network. **This field is decremented by one each time the datagram is processed by a router. If the TTL field reaches 0, the datagram must be dropped.**  

*   **Protocol**  

    The value of this field indicates the specific transport-layer protocol to which the data portion of this IP datagram should be passed.   

*   **Header checksum**

    aids a router in detecting bit errors in a received IP datagram

*   **Source and destination IP addresses**  

*   **Options**  

    The options fields allow an IP header to be extended. 

    the amount of time needed to process an IP datagram at a router can vary greatly. These considerations become particularly important for IP processing in high-performance routers and hosts. 

    For these reasons and others, IP options were dropped in the IPv6 header.  

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

A host typically has only a single link into the network; when IP in the host wants to send a datagram, it does so over this link. 

The boundary between the host and the physical link is called an **interface**.   

*   host typically has one or two interfaces (e.g., wired Ethernet, wireless 802.11)  

The boundary between the router and any one of its links is also called an **interface**.  

*   routers typically have multiple interfaces

**IP addresses are associated with each interface.**  

**Each IP address is 32 bits long (4 bytes).** In total, there are 2^32 possible IP addresses.

These addresses are typically written in **dotted-decimal notation (点分十进制记方法)**.   

**e.g.** 

*223.1.1.1 = 11011111 00000001 00000001 00000001*  



##### 3.3.1 Subnet (子网)

<img src="imgs\IP_subnet.png" alt="IP_subnet" style="zoom:67%;" />

The three hosts in the upper-left portion of the figure above, and the router interface to which they are connected, all have an IP address of the form **223.1.1.xxx**. That is, they all have the **same leftmost 24 bits** in their IP address.  

In IP terms, this network interconnecting three host interfaces and one router interface forms a **subnet** . (A subnet is also called an IP network or simply a network in the Internet literature.)  

IP addressing assigns an address to this subnet: 223.1.1.0/24, where the **/24 notation**, sometimes known as a **subnet mask (子网掩码)**, indicates that the **leftmost 24 bits of the 32-bit quantity define the subnet address**.   



*   IP address
    *   subnet part: high order bits
    *   host part: low order bits
*   what’s a subnet ?
    *   device interfaces with same subnet part of IP address
    *   can physically reach each other **without intervening router**  



##### 3.3.2 Classless Interdomain Routing (CIDR)  无类别域间路由选择

**CIDR is the Internet’s address assignment strategy.**  

*   subnet portion of address of arbitrary length

*   address format: a.b.c.d/x, where x is # bits in subnet portion of address  



**Example:**

<img src="imgs\eg_CIDR.png" alt="eg_CIDR" style="zoom:67%;" />



##### 3.3.3 IP addresses: how to get one?  

<img src="imgs\get_oneIPAddress.png" alt="get_oneIPAddress" style="zoom:67%;" />

In order to obtain a block of IP addresses for use within an organization’s subnet, a network administrator might first contact its ISP, which would provide addresses from a larger block of addresses that had already been allocated to the ISP. For example, the ISP may itself have been allocated the address block 200.23.16.0/20. The ISP, in turn, could divide its address block into eight equal-sized contiguous address blocks and give one of these address blocks out to each of up to eight organizations that are supported by this ISP. (We have underlined the subnet part of these addresses for your convenience.)  



##### 3.3.4 Hierarchical addressing: route aggregation  

**Example:**

<img src="imgs\route aggregation.png" alt="route aggregation" style="zoom:67%;" />

This example of an ISP that connects eight organizations to the Internet nicely illustrates how carefully allocated CIDRized addresses facilitate routing. Suppose, as shown in the Figure, that the ISP (which we’ll call Fly-By-Night-ISP) advertises to the outside world that it should be sent any datagrams whose first 20 address bits match 200.23.16.0/20. The rest of the world need not know that within the address block 200.23.16.0/20 there are in fact eight other organizations, each with its own subnets. This ability to use a single prefix to advertise multiple networks is often referred to as address aggregation (also route aggregation or route summarization).  



**How does an ISP get a block of addresses?**
ICANN: Internet Corporation for Assigned Names and Numbers

*   allocates addresses
*   manages DNS
*   assigns domain names, resolves disputes  



#### 3.4 Network Address Translation (NAT) 网络地址转换

**LAN** (local area network)&**WAN** (wide area network) 

从局域网到广域网

 <img src="imgs\NAT1.png" alt="NAT1" style="zoom:67%;" />



*   range of addresses not needed from ISP: just one IP address for all devices
*   can change addresses of devices in local network without notifying outside world
*   can change ISP without changing addresses of devices in local network
*   devices inside local net not explicitly addressable, visible by outside world  



*   **outgoing datagrams:** replace (source IP address, port #) of every outgoing datagram to (NAT IP address, new port #)
    . . . remote clients/servers will respond using (NAT IP address, new port #) as destination addr
*   **remember (in NAT translation table)** every (source IP address, port #) to (NAT IP address, new port #) translation pair
*   **incoming datagrams: replace** (NAT IP address, new port #) in dest fields of every incoming datagram with corresponding (source IP address, port #) stored in NAT table  



**Example:**

<img src="imgs\NAT2.png" alt="NAT2" style="zoom:67%;" />

