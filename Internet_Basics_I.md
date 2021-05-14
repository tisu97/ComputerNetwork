# Internet Basics I

### What's the Internet?

#### A Nuts-and-Bolts Description

*   All the devices which are connected to the Internet are **called hosts (主机)** or **end systems (端系统)**.

*    End systems are connected together by **a network of communication links** and **packet switches (分组交换机)**.  

     communication links, which are made up of different types of **physical media**, including coaxial cable, copper wire, optical fiber, and radio spectrum. 

     Different links can transmit data at different rates, with the **transmission rate** of a link measured in **bits/second**.

     When one end system has data to send to another end system, the sending end system segments the data and adds header bytes to each segment. The resulting packages of information, known as **packets (分组)**, are then sent through the network to the destination end system, where they are reassembled into the original data.

     A packet switch takes a packet arriving on one of its incoming communication links and forwards that packet on one of its outgoing communication links. The two most prominent types in today’s Internet are **routers (路由器)** and **link-layer switches (链路层交换机).**

*   End systems access the Internet through **Internet Service Providers (ISPs)**. 

*   End systems, packet switches, and other pieces of the Internet run **protocols (协议) that control the sending and receiving of information within the Internet**.

    *   The **`Transmission Control Protocol (TCP)`** and the **`Internet Protocol (IP)`** are two of the most important protocols in the Internet.  The `IP protocol` specifies the format of the packets that are sent and received among routers and end systems. The Internet’s principal protocols are collectively known as **`TCP/IP`**. 

*   **Internet standards** are developed by the **Internet Engineering Task Force (IETF)[IETF 2012]**. The **IETF standards documents** are called **requests for comments (RFCs)**.   

    

#### What’s the Internet: a service view

##### *The infrastructure that provides services to applications*

*   These applications: electronic mail, Web surfing, social networks, instant messaging, Voiceover-IP (VoIP), video streaming, distributed games, peer-to-peer (P2P) file sharing, television over the Internet, remote login, and much, much more. 

    The applications are said to be **distributed applications**, since they involve multiple end systems that exchange data with each other.  

*   **End systems** attached to the Internet provide an **Application Programming Interface (API)** that specifies how a program running on one end system asks the Internet infrastructure to deliver data to a specific destination program running on another end system. 

#### What’s a protocol? 

*A **protocol** defines the format and the order of messages exchanged between two or more communicating entities, as well as the actions taken on the transmission and/or receipt of a message or other event.*



### Protocol layers, service models(协议层次及其服务模型) 

* **Protocol Layering (协议分层)** 

    * Network designers organize protocols—and the network hardware and software that implement the protocols— in **layers (分层)**. 

    * We are again interested in **the services that** **a layer offers to the layer above**—the so-called **service model of a layer**. 

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

        *   **`UDP`**

            The **`UDP`** protocol provides a **connectionless service** to its applications. This is a no-frills service that provides **no reliability**, **no flow control**, and **no congestion control**.   

        *   **Segment (报文段)**:  transport-layer packet(传输层分组)

*   **Network Layer, routing of datagrams from source to destination** 

    *   Network layer moves network-layer packets known as **datagrams (数据报)** from one host to another. 

    *   **Internet transport-layer protocol (TCP or UDP)** in a source host passes a **transport-layer segment** and a **destination address** to the **network layer**.

    *   **`IP Protocol`**

        It defines the **fields (字段)** in the **datagram** as well as how the **end systems** and **routers** act on these fields.  

        There is **only one** **`IP protocol`**, and all Internet components that have a network layer must run the **`IP protocol`**.  

    *   The network layer also contains **routing protocols** that determine the routes that datagrams take between sources and destinations.

*   **Link Layer, data transfer between neighboring network elements**   

    The Internet’s **network layer routes a datagram** **through a series of routers** between the source and destination. To move a packet from one node (host or router) to the next node in the route, the network layer relies on the services of the **link layer**.

    *   **Protocol**: Ethernet; WiFi; DOCSIS protocol
    *   **Frames**: link-layer packets(链路层分组)

*   **Physical Layer, bits “on the wire”**



### Encapsulation



### Network Edge (网络边缘)

#### Network edge (网络边缘) 

*   **hosts**: **clients** and **servers**
*   **servers**, often in data centers – the “Cloud”
*   **things** (sensors, actuators) 

#### Access Networks (接入网络)

***Access network***—the network that physically connects an end system to the **first router** (also known as the “**edge router**”) on a path from the end system to any other distant end system.

