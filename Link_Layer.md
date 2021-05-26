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

