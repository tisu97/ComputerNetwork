# Internet Basics

### 1. What's the Internet?

#### 1.1 A Nuts-and-Bolts Description

*   All the devices which are connected to the Internet are **called hosts (主机)** or **end systems (端系统)**.

*    End systems are connected together by **a network of communication links** and **packet switches (分组交换机)**.  

     *   communication links, which are made up of different types of **physical media**, including coaxial cable, copper wire, optical fiber, and radio spectrum. 
     *   Different links can transmit data at different rates, with the **transmission rate** of a link measured in **bits/second**.
     
     *   When one end system has data to send to another end system, the sending end system segments the data and adds header bytes to each segment. The resulting packages of information, known as **packets (分组)**, are then sent through the network to the destination end system, where they are reassembled into the original data.
     
     *   A packet switch takes a packet arriving on one of its incoming communication links and forwards that packet on one of its outgoing communication links. The two most prominent types in today’s Internet are **routers (路由器)** and **link-layer switches (链路层交换机).**
     
*   End systems access the Internet through **Internet Service Providers (ISPs)**. 

*   End systems, packet switches, and other pieces of the Internet run **protocols (协议) that control the sending and receiving of information within the Internet**.

    *   The **`Transmission Control Protocol (TCP)`** and the **`Internet Protocol (IP)`** are two of the most important protocols in the Internet.  The `IP protocol` specifies the format of the packets that are sent and received among routers and end systems. The Internet’s principal protocols are collectively known as **`TCP/IP`**. 

*   **Internet standards** are developed by the **Internet Engineering Task Force (IETF)[IETF 2012]**. The **IETF standards documents** are called **requests for comments (RFCs)**.   

    

#### 1.2 What’s the Internet: a service view

##### *The infrastructure that provides services to applications*

*   These applications: electronic mail, Web surfing, social networks, instant messaging, Voiceover-IP (VoIP), video streaming, distributed games, peer-to-peer (P2P) file sharing, television over the Internet, remote login, and much, much more. 

    The applications are said to be **distributed applications**, since they involve multiple end systems that exchange data with each other.  

*   **End systems** attached to the Internet provide an **Application Programming Interface (API)** that specifies how a program running on one end system asks the Internet infrastructure to deliver data to a specific destination program running on another end system. 

    

#### 1.3 What’s a protocol? 

*A **protocol** defines the format and the order of messages exchanged between two or more communicating entities, as well as the actions taken on the transmission and/or receipt of a message or other event.*



### 2. Protocol layers, service models(协议层次及其服务模型) 

* **Protocol Layering (协议分层)** 

    * Network designers organize protocols—and the network hardware and software that implement the protocols— in **layers (分层)**. 

    * We are interested in **the services that** **a layer offers to the layer above**—the so-called **service model of a layer**. 

        Each layer provides its service by **(1) performing certain actions within that layer** and by **(2) using the services of the layer directly below it**. 

The protocols of the various layers are called the **protocol stack (协议栈)**. The Internet protocol stack consists of five layers: the **physical layers (物理层)**, **link layers (链路层)**, **network layers (网络层)**, **transport layers (传输层)**, and **application layers (应用层)**. 

*   **Application Layer, supporting network applications**

    *   **Protocols:** **`HTTP`** *(which provides for Web document request and transfer)*, **`SMTP`** *(which provides for the transfer of e-mail messages)*, **`FTP`** *(which provides for the transfer of files between two end systems)*, **`CoAP`** *(“HTTP" for the IoT)*
    *   **Network functions:** **`Domain Name System (DNS), 域名系统`**
    *   An application-layer protocol is distributed over multiple end systems, with the application in one end system using the protocol to exchange **packets** **of information** with the application in another end system. We’ll refer to this **packet of information at the application layer** as a **message (报文)**.  

*   **Transport Layer, process-process data transfer**  

    *   The Internet’s transport layer transports application-layer messages between **application endpoints (应用程序端点)**.  

    *   Transport protocols, **`TCP`** and **`UDP`**

        *   **TCP**

            **`TCP`** provides a **connection-oriented** service to its applications. This service includes **guaranteed delivery** of application-layer messages to the destination and **flow control**. **`TCP`** also **breaks long messages into shorter segments** and provides a **congestion-control mechanism**. 

        *   **UDP**

            The **`UDP`** protocol provides a **connectionless service** to its applications. This is a no-frills service that provides **no reliability**, **no flow control**, and **no congestion control**.   

        *   **Segment (报文段)**:  transport-layer packet(传输层分组)

*   **Network Layer, routing of datagrams from source to destination** 

    *   Network layer moves network-layer packets known as **datagrams (数据报)** from one host to another. 

    *   **Internet transport-layer protocol (`TCP` or `UDP`)** in a source host passes a **transport-layer segment** and a **destination address** to the **network layer**.

    *   **IP Protocol**

        It defines the **fields (字段)** in the **datagram** as well as how the **end systems** and **routers** act on these fields.  

        There is **only one** **`IP protocol`**, and all Internet components that have a network layer must run the **`IP protocol`**.  

    *   The network layer also contains **routing protocols** that determine the routes that datagrams take between sources and destinations.

*   **Link Layer, data transfer between neighboring network elements**   

    The Internet’s **network layer routes a datagram** **through a series of routers** between the source and destination. To move a packet from one node (host or router) to the next node in the route, the network layer relies on the services of the **link layer**.

    *   **Protocol**: Ethernet; WiFi; DOCSIS protocol
    *   **Frames**: link-layer packets(链路层分组)

*   **Physical Layer, bits “on the wire”**

*   ![protocol_service_layer](F:\Code\ComputerNetwork\imgs\protocol_service_layer.png)

### 3. Encapsulation

![encapsulation](F:\Code\ComputerNetwork\imgs\encapsulation.png)



### 4. Network Edge (网络边缘)

#### 4.1 Network edge (网络边缘) 

*   **hosts**: **clients** and **servers**

*   **servers**, often in data centers – the “Cloud”

*   **things** (sensors, actuators) 

    

#### 4.2 Access Networks (接入网)

***Access network***—the network that **physically (物理链路)** connects an end system to the **first router** (also known as the “**edge router**”) on a path from the end system to any other distant end system.

*   home networks
    *   WiFi or Ethernet
*   institutional access networks (school, company)
    *   WiFi or Ethernet
*   wide-area wireless access networks
    *   3G, 4G
    *   upcoming: 5G



### 5. Network Core (网络核心)

the network core—**the mesh of packet switches and links that interconnects the Internet’s** **end systems**.  

There are two fundamental approaches to moving data through a network of links and switches: **circuit switching** and **packet switching**.



#### 5.1 Packet Switching (分组交换)

**To send a message from a source end system to a destination end system**, the source breaks long messages into smaller chunks of data known as **packets**. Between source and destination, each packet **travels through communication links and packet switches** ( routers or link-layer switches). 

*tips: packet is a generic term,segment is at the transport layer, datagram is at the network layer,...*



##### 5.1.1 Packet switching: store-and-forward transmission

Store and forward: **entire packet must  arrive at router before it can be transmitted over next link.**

**(存储转发机制：交换机能够开始向输出链路传输分组的第一个比特之前，必须接收到整个分组!).**



##### 5.1.2 Packet switching: queuing delay and packet loss

if the arrival rate (in bits/sec) exceeds transmission rate of the next link for a period of time:

1.  packets will queue, waiting to be transmitted further

2.  packets can be dropped (lost) if memory (buffer) fills up

    

#### 5.2 Circuit switching (电路交换)

A circuit in a link is implemented with either **frequency-division multiplexing (FDM)** or **time-division multiplexing (TDM)**.   

![FDM_vs_TDM](F:\Code\ComputerNetwork\imgs\FDM_vs_TDM.png)

#### 5.3 Packet Switching vs Circuit Switching  

Packet switching allows more users to use the network!  



#### 5.4 Internet structure: network of networks 





####  5.5 Delay, loss, throughput in networks  

Three critical performance measure in computer networks: **end-to-end throughput (吞吐量)**, **delay (时延)** and **packet loss (丢包)**.

##### 5.1 Overview of Delay in Packet-Switched Networks  

As a packet travels from one node (host or router) to the subsequent node (host or router) along this path, the  packet suffers from several types of delays at each node along the path. The most important of these delays are the **nodal processing delay (结点处理时延)**, **queuing delay (排队时延)**, **transmission delay (传输时延)**, and **propagation delay (传播时延)**; together, these delays accumulate to give a **total nodal delay (结点总时延)**. 

*   **processing delay**
    *   The time required to **examine the packet’s header**, **choose where to direct the packet** or **check bit errors**  
    *   typically on the order of **microseconds** or less

*   **queuing delay**

    *   At the queue, the packet experiences a queuing delay as it waits to be transmitted onto the link. 
    *   The length of the queuing delay of a specific packet will depend on the number of earlier-arriving packets that are queued and waiting for transmission onto the link.  
    *   typically on the order of **microseconds or milliseconds**  

*   **transmission delay**

    *   Packet can be transmitted only after all the packets that have arrived before it have been transmitted.   

    *   Denote the **length of the packet** by L bits, and denote the **transmission rate** of the link from router A to router B by R bits/sec. 

        For example, for a 10 Mbps Ethernet link, the rate is R = 10 Mbps; for a 100 Mbps Ethernet link, the rate is R = 100 Mbps. 

    *   The **transmission delay** is L/R.

    *   typically on the order of **microseconds to milliseconds** in practice

*   **propagation delay**

    *   Once a bit is pushed into the link, it needs to propagate to router B. The time required to propagate from the beginning of the link to router B is the **propagation delay**.  

    *   The bit propagates at the propagation speed of the link.  

    *   The propagation speed depends on the physical medium of the link, in range of    

        **2 * (10 ^ 8) meters/sec to 3 * (10 ^ 8) meters/sec**  




##### 5.2 Queuing Delay and Packet Loss  

*   ![queing_delay](F:\Code\ComputerNetwork\imgs\queing_delay.png)

    **traffic intensity**: 流量强度

*   **Packet Loss**  

    *   buffers in routers have finite capacity
    *   packet arriving to full queue dropped (aka lost)
    *   lost packet may be retransmitted by previous node, by source end system, or not at all  



##### 5.3 End-to-End Delay





##### 5.4 Throughput

Throughput: rate (bits/time unit) at which bits transferred between sender/receiver

*   instantaneous: rate at given point in time
*   average: rate over longer period of time

The **throughput** is  the transmission rate of the **bottleneck** **link**.  

