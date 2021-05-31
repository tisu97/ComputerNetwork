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



