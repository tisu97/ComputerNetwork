# Link Layer

### 1. Introduction

#### 1.1 Terminology:  

**Node (结点):** any device that runs a link-layer protocol as a node. Nodes, including **hosts, routers, switches**

**Link (链路):** the communication channels (通信信道) that **connect adjacent nodes along the communication path**



#### 1.2 Link layer: context  

Over a given link, a transmitting **node** **encapsulates** the **datagram** in a link-layer **frame** (帧) and transmits the **frame** into the **link**.

*   datagram transferred by different link protocols over different links:  

    e.g., Ethernet on first link, frame relay on intermediate links, 802.11 on last link  

*   each link protocol provides different services  

    e.g., may or may not provide reliable delivery over link  
    
    

**transportation analogy:** 类比

*   trip from Princeton to Lausanne
    *   limo: Princeton to JFK
    *   plane: JFK to Geneva
    *   train: Geneva to Lausanne
*   tourist = datagram
*   transport segment = communication link
*   transportation mode = link layer protocol
*   travel agent = routing algorithm  



#### 1.3 Link layer services  

*   **Framing 成帧**

    Almost all link-layer protocols **encapsulate** each network-layer **datagram** within a link-layer  **frame** before transmission over the link. 

    A frame consists of a data field, in which the network-layer datagram is inserted, and a number of header fields. The structure of the frame is specified by the link-layer protocol. 

*   **Link access 链路接入**

    A medium access control (**`MAC`)** **protocol** specifies the rules by which a frame is transmitted onto the link.  

    The MAC protocol serves to coordinate the frame transmissions of the many nodes.  

*   **Reliable delivery 可靠交付** 

    Link-layer protocol provides reliable delivery service  to guarantee to move each network-layer datagram across the link without error.

    A link-layer reliable delivery service is often used for links that are **prone to high error rates**, such as a **wireless link**, with **the goal of correcting an error locally**—**on the link where the error occurs**—rather than forcing an end-to end retransmission of the data by a transport- or application-layer protocol.

*   **Error detection and correction 差错检测与纠正**

    having the transmitting node include error-detection bits in the frame, and having the receiving node perform an error check.   
    
    

#### 1.4 Where Is the Link Layer Implemented?  

The link layer is implemented in **a network adapter** (网络适配器), also sometimes known as a **network interface card (NIC)** (网络接口卡). 

At the heart of the network adapter is the **link-layer controller** 链路层控制器, usually a single, special-purpose chip that implements many of the link-layer services (**framing**, **link access**, **error detection**, and so on).  

*   link layer implemented in “adaptor” or on a chip
    *   Ethernet card, 802.11 card; Ethernet chipset
    *   implements link, physical layer
    *   802.15.4: software
*   attaches into host’s system buses
*   combination of hardware and software  

<img src="imgs\Network_adapter.png" alt="Network_adapter" style="zoom:67%;" />

### 2. Error detection  

**error-detection and -correction bits (EDC)** 差错检测和纠正比特

EDC = Error Detection and Correction
D = Data protected by error checking, may include header
Error detection not 100% reliable!  

<img src="imgs\Error-detection and -correction scenario.png" alt="Error-detection and -correction scenario" style="zoom:67%;" />

At the sending node, data, D, to be protected against bit errors is augmented with error-detection and -correction bits (EDC). Typically, the data to be protected includes not only the datagram passed down from the network layer for transmission across the link, but also link-level addressing information, sequence numbers, and other fields in the link frame header. 

Both D and EDC are sent to the receiving node in a link-level frame. At the receiving node, a sequence of bits, D and EDC is received. Note that D and EDC may differ from the original D and EDC as a result of in-transit bit flips.  

The receiver’s challenge is to determine whether or not D is the same as the original D, given that it has only received D and EDC. The exact wording of the receiver’s decision in Figure (we ask whether an error is detected, not whether an error has occurred!) is important. 

Error-detection and -correction techniques allow the receiver to sometimes, but not always, detect that bit errors have occurred. Even with the use of error-detection bits there still may be **undetected bit errors**; that is, the receiver may be unaware that the received information contains bit errors. As a consequence, the receiver might deliver a corrupted datagram to the network layer, or be unaware that the contents of a field in the frame’s header has been corrupted. We thus want to choose an error-detection scheme that keeps the probability of such occurrences small.  



##### Three techniques for detecting errors in the transmitted data  

*   parity checks (to illustrate the basic ideas behind error detection and correction)
*   checksumming methods (which are more typically used in the transport layer)
*   cyclic redundancy checks (which are more typically used in the link layer in an adapter)



#### 2.1 Parity checking 奇偶校验

##### single parity bit (奇偶校验位)

<img src="imgs\One-bit even parity.png" alt="One-bit even parity" style="zoom:50%;" />

Suppose that the information to be sent, D in Figure, has d bits. 

In an **even parity scheme**, the sender simply includes **one additional bit** and chooses its value such that **the total number of 1s in the d + 1 bits (the original information plus a parity** **bit) is even**. 

For **odd parity schemes**, the parity bit value is chosen such that there is an **odd number of 1s**. 

Figure illustrates an even parity scheme, with the single parity bit being stored in a separate field.

Receiver operation is also simple with a single parity bit. The receiver need only count the number of 1s in the received d + 1 bits. 

If an odd number of 1-valued bits are found with an even parity scheme, the receiver knows that at least one bit error has occurred. More precisely, it knows that some odd number of bit errors have occurred.

But what happens if an even number of bit errors occur? You should convince yourself that this would result in an **undetected error**. (导致未检测出的差错!) 



Measurements have shown that, rather than occurring independently, errors are often clustered together in “bursts.” Under burst error conditions, the probability of undetected errors in a frame protected by single-bit parity can approach 50 percent.

如果差错聚集地突发，那么未检测出差错的概率高达50%

Clearly, a more robust error-detection scheme is needed!



##### A two-dimensional generalization of the single-bit parity scheme  

<img src="imgs\Two-dimensional even parity.png" alt="Two-dimensional even parity" style="zoom:67%;" />

#### 2.2 Cyclic redundancy check 循环冗余检测

An error-detection technique used widely in today’s computer networks is based on **cyclic redundancy check (CRC) codes**.  CRC codes are also known as **polynomial codes** since it is possible to view the bit string to be sent as a polynomial whose coefficients are the 0 and 1 values in the bit string, with operations on the bit string interpreted as polynomial arithmetic.  

**To be continued...**



#### 2.3 Multiple access links, protocols  

##### Two types of links

*   point-to-point
    *   point to point protocol (PPP) for dial-up access
    *   point-to-point link between Ethernet switch, host
*   broadcast (shared wire or medium)
    *   old-fashioned Ethernet
    *   802.11 wireless LAN  
    
    

##### Multiple access protocol  多路访问协议

*   single shared broadcast channel
*   two or more simultaneous transmissions by nodes: interference
    *   collision if node receives two or more signals at the same time  

multiple access protocol

*   distributed algorithm that determines how nodes share channel, i.e., determine when node can transmit

*   communication about channel sharing must use channel itself

    *   no out-of-band channel for coordination  

    

##### An ideal multiple access protocol  

…given broadcast channel of rate R bps
**desired properties:**

1.  when one node wants to transmit, it can send at rate R

2.  when M nodes want to transmit, each can send at average rate R/M

3.  fully decentralized 协议是分散的!

    *   no special node to coordinate transmissions
    *   no synchronization of clocks, slots

    *   there is no master node that represents a single point of failure for the network.  

4.  simple  



**3 types of multiple access protocol **   

**broadcast 广播**

*   channel partitioning protocols (信道划分协议)
    *   divide channel into smaller “pieces” (time slots, frequency, code)
    *   allocate piece to node for exclusive use  

*   random access protocols (随机接入协议)
    *   channel not divided, allow collisions
    *   “recover” from collisions  

*   taking-turns protocols (轮流协议)
    *   nodes take turns, but nodes with more to send can take longer turns  



#### 2.4 Channel partitioning Protocols 信道划分协议  

*   Time-division multiplexing (TDM) 多路复用

*   Frequency-division multiplexing (FDM) 频分多路复用

**Time-division multiplexing (TDM)** and **frequency-division multiplexing (FDM)** are two techniques that can be used to **partition a broadcast channel’s bandwidth** among all nodes sharing that channel.   



**Suppose** the channel supports N nodes and that the transmission rate of the channel is R bps. 

##### 2.4.1 TDM 

TDM divides time into time frames and further divides each time frame into N time slots.   

Whenever a node has a packet to send, it transmits the packet’s bits during its assigned time slot in the revolving TDM frame. 

Typically, slot sizes are chosen so that a single packet can be transmitted during a slot time.   

<img src="imgs\TDM_example2.png" alt="TDM_example2" style="zoom: 80%;" />

*   access to channel in "rounds"
*   each station gets fixed length slot (length = pkt trans time) in each round
*   unused slots go idle (不被使用，等他的时长结束)
*   example: 6-station LAN, 1,3,4 have pkt, slots 2,5,6 idle  

<img src="imgs\TDM_example1.png" alt="TDM_example1" style="zoom:67%;" />

类比: 一个采用TDM规则的鸡尾酒会允许每个人在固定的时间段内发言同样时长，且不断重复这种模式。



*   Pros:
    *   TDM eliminates collisions and is perfectly fair: Each node gets a dedicated transmission rate of R/N bps during each frame time. However,
*   Cons: 
    *   a node is limited to an average rate of R/N bps even when it is the only node with packets to send.
    *   a node must always wait for its turn in the transmission sequence—again, even when it is the only node with a frame to send. 

类比: Imagine the partygoer who is the only one with anything to say (and imagine that this is the even rarer circumstance where everyone wants to hear what that one person has to say). Clearly, TDM would be a poor choice for a multiple access protocol for this particular party.



##### 2.4.2 FDM

*   channel spectrum divided into frequency bands
*   each station assigned fixed frequency band
*   unused transmission time in frequency bands go idle
*   example: 6-station LAN, 1,3,4 have pkt, frequency bands 2,5,6 idle  



#### 2.5 Random access protocols  

*   when node has packet to send
    *   transmit at full channel data rate R 传输结点总以信道全速率进行发送
    *   no a priori coordination among nodes  

When there is a collision, each node involved in the collision repeatedly retransmits its frame (that is, packet) until its frame gets  

*   two or more transmitting nodes ➜ collision

*   random access MAC protocol specifies

    *   how to detect collisions
    *   how to recover from collisions (e.g., via delayed retransmissions)

*   examples of random access MAC protocols

    *   slotted ALOHA
    *   ALOHA
    *   CSMA, CSMA/CD, CSMA/CA  

    

##### 2.5.1 Slotted ALOHA  时隙ALOHA

We assume:

*   All frames consist of exactly L bits.
*   Time is divided into slots of size L/R seconds (that is, a slot equals the time to transmit one frame).
*   Nodes start to transmit frames only at the beginnings of slots.
*   The nodes are synchronized so that each node knows when the slots begin.
*   If two or more frames collide in a slot, then all the nodes detect the collision event before the slot ends  

<img src="imgs\SLOT_ALOHA_1.png" alt="SLOT_ALOHA_1" style="zoom: 33%;" />

<img src="imgs\SLOT_ALOHA_EFFICIENCY.png" alt="SLOT_ALOHA_EFFICIENCY" style="zoom:50%;" />

##### 2.5.2 (Pure) ALOHA

<img src="imgs\ALOHA.png" alt="ALOHA" style="zoom:67%;" />

##### 2.5.3 Carrier Sense Multiple Access (CSMA) 载波侦听多路访问

CSMA listen before transmit:

*   if channel sensed idle, transmit entire frame
*   if channel sensed busy, defer transmission

human analogy: don’t interrupt others!  



##### 2.5.4 CSMA/CD (collision detection)  

**CSMA/CD: carrier sensing as in CSMA**

*   collisions detected within short time
*   colliding transmissions aborted, reducing channel wastage

**collision detection**

*   easy in wired LANs: measure signal strengths, compare
    transmitted, received signals
*   difficult in wireless LANs: received signal strength overwhelmed by local transmission strength
*   human analogy: the polite conversationalist  



**Ethernet CSMA/CD algorithm**  

1.  NIC receives datagram from network layer, creates frame  
2.  if NIC senses channel idle, starts frame transmission; if NIC senses channel busy, waits until channel idle, then transmits  
3.  If NIC transmits entire frame without detecting another transmission, NIC is done with frame 
4.  If NIC detects another transmission while transmitting, aborts and sends jam signal   
5.  After aborting, NIC enters binary (exponential) backoff:  
    *   after mth collision, NIC chooses K at random from {0,1,2, …, 2m-1}. NIC waits K·512 bit times, returns to Step 2  
    *   longer backoff interval with more collisions  



**CSMA/CD efficiency**  

<img src="imgs\CDMACD_efficiency.png" alt="CDMACD_efficiency" style="zoom:67%;" />

#### 2.6 Taking-Turns Protocols 轮流协议 

##### Polling 轮询协议

<img src="imgs\polling.png" alt="polling" style="zoom:67%;" />



##### token passing  令牌传递协议

<img src="imgs\tokenpassing.png" alt="tokenpassing" style="zoom:67%;" />



#### 2.7 Summary of Multiple Access Protocols

*   channel partitioning, by time, frequency or code
    *   Time Division, Frequency Division
*   random access (dynamic),
    *   ALOHA, S-ALOHA, CSMA, CSMA/CD
    *   carrier sensing: easy in some technologies (wire), hard in others (wireless)
    *   CSMA/CD used in Ethernet
    *   CSMA/CA used in 802.11
*   taking turns
    *   polling from central site, token passing
    *   bluetooth, FDDI, token ring  

