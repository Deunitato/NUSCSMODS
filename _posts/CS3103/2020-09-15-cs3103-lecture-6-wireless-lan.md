---
layout: post
published: true
title: 'CS3103 - Lecture 6 : Wireless Lan'
---


# MAC Sublayer - CSMA/CA
#### Characteristics
![CS3103-6-1.PNG]({{site.baseurl}}/img/CS3103-6-1.PNG)

Application (5G)
- IOT devices might need a few bytes of data
- Need to done very fast
- Low latency
- Bandwidth
#### Standards
![CS3103-6-2.PNG]({{site.baseurl}}/img/CS3103-6-2.PNG)



# MAC Sublayer = DCF and PCF

##### Ethernet MAC sublayer - CSMA/CD

- Listen while you talk
![CS3103-6-3.PNG]({{site.baseurl}}/img/CS3103-6-3.PNG)

- Send packet
- Listens
	- If detect collision:
    	- Jam channel
        - Back off
        - Wait
        - Attemp again, increase attempt counter
        - Discard packet
    - Else:
    	- Packet?

## MAC layer Protocol (CSMA/CA)

![CS3103-6-4.PNG]({{site.baseurl}}/img/CS3103-6-4.PNG)

> What is the purpose of IFS
>  Make sure those just start transmission time is detected
> - If someone already started transmission, it might look idle but actually someone is still transmitting. So we will wait
> 
> What is the purpose of cotention window
> - Avoid multiple channels transmitting the same time
> Then is collision avoidance achieved

### Why collision avoidance
- CSMA/CA looks more inefficient than CSMA/CD (Waiting times and ack)
- But we cant use CSMA/CD for wireless

> Why is collision avoidance harder for wireless? (If cannot see each other, do they no its collision)
> - Hidden nodes

Why cant we use CSMA/CD for wireless?


### Collision detection is hard in wireless networks
- Energy level during transmission, idleness or collision

![CS3103-6-5.PNG]({{site.baseurl}}/img/CS3103-6-5.PNG)

If the sender and the reciever are at far right, by time the signal reach them, it might not be strong enough for the reciever to detect it.



### Hidden node problem
- CSMA/CA suffers from this
- Picture:
![CS3103-6-6.PNG]({{site.baseurl}}/img/CS3103-6-6.PNG)


Station B and C are hidden from each other wrt A
-  B transmission to A may overlap with C transmission to A
- What happens while B is transmitting to A, C senses the carrier to start transmission?
	- There will be a collision in the yellow area

> Hidden nodes reduce the capacity of the network as they increase the possibility of collision


#### MAC LAYER (CSMA/CA)

![CS3103-6-7.PNG]({{site.baseurl}}/img/CS3103-6-7.PNG)

#### Reservation scheme (CTS) prevents hidden station problem

1. Source Send a RTS to Dest
2. Dest CTS (clear to send) to source to both side
3. The other nodes would go into NAV time where they will not send anything
4. Send the data
5. Return back to 1

> The other ndoes will know that something else is happening in the link even if the RTS didnt reach 


![CS3103-6-8.PNG]({{site.baseurl}}/img/CS3103-6-8.PNG)

Sending the data and return back to 1:

![CS3103-6-9.PNG]({{site.baseurl}}/img/CS3103-6-9.PNG)


** RTS/CTS **
![CS3103-6-10.PNG]({{site.baseurl}}/img/CS3103-6-10.PNG)

Set a timer and wait, if did not receive ack before time out then wait with increment of k. If k > limit, abort.


#### Interframe space types (IFS)
There is different priority for each reply

![CS3103-6-11.PNG]({{site.baseurl}}/img/CS3103-6-11.PNG)


Values:
- DIFS (Distributed coordination function IFS)
	- Longest IFS
    - Used as minimum delay of asynchronous frames contending for access
    - Used for all ordinary asynchronous traffic
- Short IFS (SIFS)
	- Shortest IFS
    - is used for immediate response action. 
    - Priority is given for response messsages over another new message from other stations
    - Clear to send (CTS)
    - Ack
    - Poll response
- Point coordination function IFS (PIFS)
	- Mid length IFS
    - Used by centralised controller in PCF scheme when using poll
    - Takes precedence over normal contentin traffic (DIFS)
  

## Exposed node problem
![CS3103-6-12.PNG]({{site.baseurl}}/img/CS3103-6-12.PNG)

1. A send RTS to B and C
2. B send CTS
3. Data transmitted

![CS3103-6-14.PNG]({{site.baseurl}}/img/CS3103-6-14.PNG)


Why cant C send data to D? C can send to D because the area is free but it will erronously think that it cannot send because of the received RTS
- Waste the channel capacity

> What if C sends an RTS immediately after timeout for CTS from B and no data in channel?
> - C will send and it will collide with A's data
> - From C point, it is getting CTF from D and recieving data from A.. 
> - However, it will not affect A ability to send to B

Unfortunately, there is no good solution that can make C send Data to D while being sent data from A



### RTS/CTS do not solve Exposed node problem]

![CS3103-6-15.PNG]({{site.baseurl}}/img/CS3103-6-15.PNG)

- C hears from RTS from A but not CTS from B
- After timeout period it sends RTS to D
- A is in sending state and not recieving
- D response with CTS
- Now C cannot hear D CTS because of collision
- C has to wait until A completes transmission


## MAC sublayer
- Two types of mac layer protocols:
	- DCF: Distributed coordination function
    	- Contention service (On top)
        - Includes that the access points and the nodes are equal
    - PCF: Polling service
    	- Optinal access method for infrastructure network
        - Used for time sensitive transmission

PCF is implemented on top of DCF
![CS3103-6-16.PNG]({{site.baseurl}}/img/CS3103-6-16.PNG)


### PCF
- Provides a centralised contention free polling access method 
- PC (point coordinator) module at AP performs polling
	- Stations request AP register them on polling list
    - AP regularly polls stations on polling list and delivers traffic
- Both PCF and DCF operate simultaneously

How does PC (AP) gets access to media
- If 10 nodes are registered for polling and refrain from contention
- 4 nodes are not registered and operate in DCF mode 
- Hence AP will be contending with these 4 nodes

Another IFS value is introduce PIFS. It is shorter than DIFS. AP use this to gain access to media
> How PIFS helps AP get access to media?
> - It is shorter


What happens if AP always uses PIFS?
> Starvation of DCF based stations (Since AP always has priority)


#### AP use reptition interval
AP will use PIFS sometimes and then use DIFS sometimes. AP will do this repeatedly.. this is to give chance to other nodes.

![CS3103-6-17.PNG]({{site.baseurl}}/img/CS3103-6-17.PNG)



# Lab Notes

## Basic service set (BSS) or cell
- Building Block of a wireless lan
- A set of station controlled by a **single coordiantion function**, the logical function that determines when a station can transmit or recieve
- A BSS without an AP is called an **ad hoc netowrk or IBSS (independent Basic service set)**
- A BSS with an AP is called **infrastructure network**

![CS3103-6-18.PNG]({{site.baseurl}}/img/CS3103-6-18.PNG)


## Extended service set (ESS)
- A set of **one or more basic service set interconnected by a distributed system(DS)**
- Traffic always flow via Access point
- Diameter of the cell is double the coverage distance between two wireless stations

> Why not a single big bss
> 
> - There will be lots of collision so the efficiency will decrease

A single BSS with access point can be called a ESS

### DS
- A system to interconnect a set of basic service sets
	- Intergrated (Single AP), Wired, Wireless
Wired with the cost is to be low, wireless is quite good also.

![CS3103-6-19.PNG]({{site.baseurl}}/img/CS3103-6-19.PNG)


## SSID
- Service set identifier or Extended SSID
- Network name 
- 32 octets long
- One network (ESS or IBSS) has one SSID

## BSSID
- Basic service set identifier
- cell identifier
- 6 octets long (mac address format)
- one bss has one BSSID
- Value of **BSSID is the same as the MAC address of the radion in the access poinnt**
- IN an IBSS, the BSSID is a locally administered MAC address generated from a **48 bit random number**

## Frame formats
In normal frames, we only have source and destination but for this we have 4

![CS3103-6-20.PNG]({{site.baseurl}}/img/CS3103-6-20.PNG)

## Field type desc
![CS3103-6-21.PNG]({{site.baseurl}}/img/CS3103-6-21.PNG)

> What is the purpose of the beacons?
>
> - To announce the existance of that wireless


> When reassociation is done and how?
>
> - Move to another AP
> - BSS to BSS within an ESS

#### Why 4 adddresses

![CS3103-6-22.PNG]({{site.baseurl}}/img/CS3103-6-22.PNG)

- Ap - 3 Or 4 address 

The addresses changes depending on who is the sender and who is the destination
![CS3103-6-23.PNG]({{site.baseurl}}/img/CS3103-6-23.PNG)



Questions:
- How a node associates with an AP
- How to handle mobility
- What are WEP and WPA2

    
# Slides
<iframe src="https://drive.google.com/file/d/1lFgoBz6_WckurwAxnBiD_aWR4fYjth1H/preview" width="840" height="880"></iframe>
