# Wireless   

### 1. Introduction

*   wireless (mobile) phone subscribers now exceeds
    \# wired phone subscribers (5-to-1)!
*   # wireless Internet-connected devices equals
    \# wireline Internet-connected devices
    *   laptops, Internet-enabled phones promise anytime untethered
        Internet access
    *   (low-power) wireless sensors and other IoT devices increasing
        rapidly
*   two important (but different) challenges
    *   wireless: communication over wireless link
    *   mobility: handling the mobile user who changes point of
        attachment to network  



#### 1.1 Elements of a wireless network

##### wireless hosts 无线主机

*   laptop, smartphone
*   run applications 
*   may be stationary (non mobile) or mobile 
*   wireless does not always mean mobility

##### base station 基站

*   typically connected to wired network 
*   responsible for sending packets between wired network and wireless host(s) in its “area” 
    *    e.g., cell towers, 802.11 access points

##### wireless link 无线链路

*   used to connect mobile(s) to base station 
*   also sometimes used as backbone link 
*   multiple access protocol coordinates link access 
*   various data rates, transmission distance

##### Infrastructure mode 基础设施模式

*   base station connects mobiles into wired network
*   handoff/handover: mobile changes base station providing connection into wired network  

##### Ad hoc mode 自组织模式

*   no base stations
*   nodes can only transmit to other nodes within link coverage
*   nodes organize themselves into a network: route among themselves  



<img src="imgs\elements_of_wireless_network.png" alt="elements_of_wireless_network" style="zoom:67%;" />



##### Single hop&Multiple hops 单跳与多跳

<img src="imgs\single_hop_multiple_hop.png" alt="single_hop_multiple_hop" style="zoom:67%;" />

##### Characteristics of selected wireless links

<img src="imgs\Characteristics of selected wireless links.png" alt="Characteristics of selected wireless links" style="zoom:67%;" />



### 2. Wireless link characteristics

##### Important differences from wired link 

*   **decreased signal strength:** radio signal attenuates as it propagates through matter (path loss)
*   **interference from other sources:** standardized wireless network frequencies (e.g., 2.4 GHz) shared by other devices (e.g., phone); devices (motors) interfere as well
*   **multipath propagation:** radio signal reflects off objects ground, arriving at destination at slightly different times  

##### They make communication across (even a point to point) wireless link much more difficult.



*   **SNR**: signal-to-noise ratio 信噪比

    *   larger SNR means easier to extract signal from noise (a “good thing”)

*   **BER**: bit error rate 比特差错率

*   **SNR** versus **BER** tradeoffs

    *   given physical layer: increase power -> increase SNR -> decrease BER (bit error rate)

    *   given SNR: choose physical layer that meets BER requirement, giving highest throughput

        *   SNR may change with mobility: dynamically adapt physical layer (modulation technique, rate, …)  

        

<img src="imgs\bit_rate_transmission_rate_SNR.png" alt="bit_rate_transmission_rate_SNR" style="zoom:67%;" />



##### Hidden terminal problem caused by obstacle and fading  

<img src="F:\Code\ComputerNetwork\imgs\Hidden terminal problem.png" alt="Hidden terminal problem" style="zoom: 50%;" />



### 3. Code Division Multiple Access (CDMA)  

*   unique “code” assigned to each user; i.e., code set partitioning
    *   all users share same frequency, but each user has own “chipping” sequence (i.e., code) to encode data
        *   allows multiple users to “coexist” and transmit simultaneously with minimal interference (if codes are “orthogonal”)
*   encoding: (original data) X (chipping sequence)
*   decoding: inner-product of encoded signal and chipping sequence  



##### Coding/decoding example:

<img src="imgs\CDMA encoding eg1.png" alt="CDMA encoding eg1" style="zoom:67%;" />

<img src="imgs\CDMA encoding eg2.png" alt="CDMA encoding eg2" style="zoom:67%;" />



### 4. IEEE 802.11 wireless LAN WiFi

##### 802.11b  

*   2.4 GHz unlicensed spectrum

*   up to 11 Mbps  

*   direct sequence spread spectrum (DSSS) in physical layer  
    *   all hosts use same chipping code  

##### 802.11a

*   5-6 GHz range
*   up to 54 Mbps  

##### 802.11g

*   2.4-5 GHz range
*   up to 54 Mbps  

##### 802.11n: multiple antennae

*   2.4-5 GHz range
*   up to 200 Mbps  



They all use CSMA/CA for multiple access.
They all have base-station and ad-hoc network versions.



#### 4.1 802.11 LAN architecture  

<img src="imgs\80211 LAN architecture.png" alt="80211 LAN architecture" style="zoom: 50%;" />



#### 4.2 Channels and Association  



#### 4.3 802.11: passive/active scanning  

##### passive scanning: 被动扫描

1.  beacon frames sent from APs
2.  association request frame sent: H1 to selected AP
3.  association Response frame sent from selected AP to H1  

##### active scanning: 主动扫描

1.  probe request frame broadcast from H1
2.  probe response frames sent from APs
3.  association request frame sent: H1 to selected AP
4.  association response frame sent from selected AP to H1  