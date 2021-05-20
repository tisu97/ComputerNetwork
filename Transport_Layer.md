# Transport Layer

### 1. Transport services and protocols  

*   **provide** **logical communication** **between** **processes running on different hosts**  

*   transport protocols run in end systems
    *   send side: **breaks** application **messages** into **segments**, passes down to **network layer**
    *   receiving side: **reassembles** segments into messages, passes up to **application layer**  

*   more than one transport protocol available to apps
    Internet: **`TCP`** and **`UDP`**  

    

#### 1.1 Transport layer vs network layer  

The transport layer lies just above the network layer in the protocol stack.  

Whereas a **transport-layer protocol** provides logical communication between **processes running on different hosts**, a **network-layer protocol** provides logical  communication between **hosts**.  



### 2. Internet transport-layer protocols  

*   reliable, in-order delivery (**`TCP`**)
    *   congestion control
    *   flow control
    *   connection setup   
*   unreliable, unordered delivery: **`UDP`**
    *    “best-effort” delivery service, IP  



### 3. Multiplexing and Demultiplexing (多路复用和多路分解)

**`socket API`**: the interface between application and transport layer  

#### Multiplexing/demultiplexing  

<img src="imgs\Multiplexing_demultiplexing.png" alt="Multiplexing_demultiplexing" style="zoom: 50%;" />



#### 3.1 Demultiplexing (多路分解)

##### Definition of demultiplexing:

Use the hearder info to deliver received segments to the correct socket in the receiver.



*   demultiplexing is the process of handing an incoming packet to the right application
    *   …via the right socket

*   demultiplexing task of the transport protocol
    *   decision based on info in packet headers   

* demultiplexing at receiver  

    use header info to deliver received segments to correct socket  

    

#### 3.1.1 How demultiplexing works  

<img src="imgs\How demultiplexing works.png" alt="How demultiplexing works" style="zoom: 50%;" />



#### 3.2 Multiplexing (多路复用)

##### Definition of multiplexing:

*   The transport layer handles data from **source host** from different sockets, encapsulating each data chunk with header information (that will later be used in demultiplexing) to create segments, 

*    **passing the segments to the network layer** to be sent to a receiving host.

     

##### multiplexing at sender

handle data from multiple  sockets, add transport header (later used for demultiplexing)  



#### 3.3 `UDP`: User Datagram Protocol  

##### 3.3.1 characteristics:

*   do just about **as little as** a transport protocol can do. Aside from the multiplexing/demultiplexing function and some light error checking, it adds nothing to IP (bare-bone Internet transport protocol)
*   **best effort service**  
*   UDP segments may be
    *   **lost**
    *   **delivered out-of-order**  

*   **connectionless**  
    *   no handshaking between `UDP` sender, receiver (不建立连接，不需要任何准备即可进行数据传输)
    *   each `UDP` segment handled independently of others  
*   `UDP` used for
    *   streaming multimedia apps (loss tolerant, rate sensitive)
    *   DNS
    *   sensor data  

##### 3.3.2 UDP: segment header  

<img src="imgs\UDP segment header.png" alt="UDP segment header" style="zoom:50%;" />

application data (payload, 也是 **data field, 数据字段**)以外的部分均为**首部字段 (header field)**



*   The **application data** occupies the **data field** of the UDP segment.   

*   The `UDP` **header** has only **four fields**, each consisting of **two bytes**.  

*   the **port numbers** allow the **destination host** to pass the **application data** to the **correct process** running on the **destination end system** (that is, to perform the **demultiplexing** function).  

*   The **length** **field** specifies the **number of bytes** in the UDP segment (**header plus data**).  

*   The **checksum** is used by the **receiving host** to **check** whether **errors** have been introduced into the segment.   

    

##### 3.3.3 UDP: checksum (检验和)  

**goal:** detect “errors” (e.g., flipped bits) in segments. That is, the checksum is used to determine whether bits within the UDP segment have been altered.



*   for sender:
    *   treat segment contents, including header fields, as sequence of 16-bit integers
    *    checksum: bit addition of segment contents, then one complemented   
    *   sender puts checksum value into UDP checksum field  
*   for receiver:
    *   compute checksum of received segment  
    *   then...



##### Why UDP provides a checksum in the first place?

There is no guarantee that all the links between source and destination provide error checking. UDP must provide error detection at the transport layer, on an end-end basis, if the end-end data transfer service is to provide error detection (**end-end principle**).  



#### 3.4 Connection-Oriented Transport: `TCP`  

##### 3.4.1 The TCP Connection  

`TCP` protocol runs only in the **end systems** and not in the intermediate network elements (routers and link-layer switches).  

*   **connection-oriented**

    Before one application process can begin to send data to another, the two processes must **first “handshake”** with each other—that is, they must send some **preliminary segments** to each other to establish the parameters of the ensuing data transfer.   

*   **full-duplex service** (双全工服务)

    If there is a TCP connection between Process A on one host and Process B on another host, then application layer data can flow from Process A to Process B at the same time as application layer data flows from Process B to Process A.

*   **point-to-point**  

    between a single sender and a single receiver 



##### 3.4.2 TCP segment structure  

<img src="imgs\TCP segment structure.png" alt="TCP segment structure" style="zoom: 67%;" />

The **header** (首部) includes: 

*   **source and destination port numbers** 源端口号和目的端口号

    which are used for multiplexing/demultiplexing data from/to upper-layer applications

*   **checksum field** 检查和字段

*   **sequence number field** (32 bit) 序号字段

    used by the TCP sender and receiver in implementing a reliable data transfer service  

*    **acknowledgment number field** (32 bit) 确认号字段

    used by the TCP sender and receiver in implementing a reliable data transfer service  

*   **receive window field** (16-bit) 接收窗口字段

    which is used for flow control

*   **header length field** (4-bit) 首部长度字段

    specifies the length of the TCP header in 32-bit words  

*    **options field** (he optional and variable-length) 选项字段

    used when a sender and receiver negotiate the **maximum segment size (MSS)** or as a window scaling factor for use in high-speed networks

*   **flag field** (6 bit) 标志字段

    *   **ACK** bit is used to indicate that the value carried in the acknowledgment field is valid  
    *   The **RST**, **SYN**, and **FIN** bits are used for connection setup and teardown
    *   Setting the **PSH** bit indicates that the receiver should pass the data to the upper layer immediately   
    *   the **URG** bit is used to indicate that there is data in this segment that the sending-side upper-layer entity has marked as “urgent.” 

