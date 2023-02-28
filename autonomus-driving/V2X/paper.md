# Comparison of IEEE 802.11p and LTE-V2X: An Evaluation With Periodic and Aperiodic Messages of Constant and Variable Size

[Comparison of IEEE 802.11p and LTE-V2X: An Evaluation With Periodic and Aperiodic Messages of Constant and Variable Size](https://ieeexplore.ieee.org/document/9133075)

## Introduction

### V2X technologies

| IEEE 802.11p | 3GPP (The Third Generation Partnership Project) |
| --- | --- |
| DSRC(Dedicated Shot Range Communication) | LTE-V2X (based on ‘PC5’) |
| ITS-G5(specified in Europe by ETSI) | Celluar V2X (C-V2X) (based on ‘sidelink LTE radio interface’) |

### Comparison of IEEE 802.11p and LTE-V2X

1. link level : 
    
    LTE-V2X can improve the link budget over IEEE 802.11p by around 7dB and increase the communication range and reliability at the link level
    
2. system level:
    1. The model generating periodic message 
    2. **The model generating aperiodic messages of variable size**
        
        ⇒ IEEE 802.11p outperforms LTE-V2X
        

## V2X Technologies

### IEEE 802.11p

- simpler and more flexible MAC
- use an OFDM(Orthogonal Frequency Division Multiplexing)-bsed PHY(physical) layer with a channel bandwidth of 10MHz.
- **DCF(Distributed Coordination Function)** of IEEE 802.11
    - **CSMA/CA(Carrier Sense Multiple Access with Collision Avoidance)**
    - **CCA(Clear Channel Assistant) threshold**
- **capture effect**
    - decoding receiver
        - when receiver capture a sharp increase(e.g. by 10dB)
        - stop current decoding
        - start decoding the new packet
    - prevent higher signal from being interferer
    - strong impact on hidden terminal problem
    - improve the packet reception probability at short distances.

### LTE-V2X

- operate with 10MHz or 20MHz channel
- **time-frequency resource structure**

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5876e94e-f547-4e8e-ae73-45b9fb7c4dcd/Untitled.png)

$$
⁍
$$

- $1\cdot\text{RB} = 180\text{kHz}$
- $1 \cdot \text{sub-carrier} = 15\text{kHz}$
- data → TB로 encapsuled
- control information → SCI 로 encapsuled
- parameters
    - the number of RBs per sub-channel
    - the number of sub-channel per sub-frame
- **sensing based SPS(Semi-Persistent Scheduling) scheme**
    - sensing based
        - identify and select sub-channels that are not occupied by other vehicles
        - **RRI (Resource Reservation Interval)**
            - utilize sub-channel by $t+\text{RRI}$ and notify other vehicles it
    - **Reselection Counter**
        - reselect sub-channel period
        - decemented by 1 after each transmission
        - When become **0**
            - reserve new sub-channel with probability $(1-P)$
                - $P$ : augments
                - maintain current reservation with probability $P$
        - if **current reservation of new-subchannel < latency deadline of new packet**
        - set the RRI in the SCI equal to 0 when change into new sub-channel
    - 3 steps of the sensing-based SPS scheme
        
        <aside>
        ☝ new sub-channel의 후보가 될 CSRs List를 구성
        
        1. 같은 sub-frame의 이웃한 sub-channel들로 구성
        2. 지금부터 자신의 Reselection Counter 동안 다른 vehicle에 사용될 예정인 CSR 제외
        3. RSRP threshold보다 큰 RSRP를 가지는 CSR 제외 *(적어도 전체 20%의 CSR 포함)*
        4. 남은 CSRs 중 lowest average RSSI를 가진 CSRs 추리기 *(전체 20%의 CSR 포함)*
        5. 4번까지 남은 CSR 중에서 random하게 new sub-channel 고름
        
        </aside>
        
        - Step 1. The ego vehicle identifies first the Candidate Single-Subframe Resources (CSRs) within the Selection Window. The Selection Window (Fig. 1) is the time
        period between *T* and the latency deadline of the incoming packet (equal or lower than 100 ms [16]). **A CSR is a group of adjacent sub-channels within the same
        sub-frame where the new SCI+TB to be transmitted fits.**
        - Step 2. The ego vehicle excludes the identified CSRs that it estimates will be used by other vehicles. To this aim, the ego vehicle senses the transmissions from other vehicles during the so-called Sensing Window. The Sensing Window is the time period that includes the last 1000 sub-frames before *T* (Fig. 1). A CSR
        is excluded if the two following conditions are met:
            
            1) the ego vehicle has received an SCI from another vehicle indicating that it will utilize this CSR in the current Selection Window or at the same time as the
            ego vehicle will need it to transmit any of its following *Reselection Counter* transmissions; 
            
            2) the ego vehicle excludes a CSR if its **RSRP(average Reference Signal Received
            Power)** measured over the TB associated to the corresponding SCI is higher than a given threshold. The RSRP threshold is a configurable parameter. The ego
            vehicle builds a list $L1$ with all the CSRs that have not been excluded. $L1$ must include **at least 20% of all CSRs in the Selection Window.** Otherwise, Step 2 is iteratively executed increasing the RSRP threshold by 3 dB at each iteration until the 20% target is met.
            
        - Step 3. The ego vehicle builds a list L2 with the CSRs included in L1 that have the **lowest average RSSI (Received Signal Strength Indicator) over all its RBs**. This RSSI value is averaged over all the previous *TCSR**TIPI*·*j* sub-frames where *TIPI* = 100 ms. **The total number of CSRs in L2 must be equal to 20% of all CSRs in the Selection Window.** The ego vehicle randomly selects a CSR from L2 to transmit its new packet, and it maintains the selection for its next *Reselection Counter* transmissions. **We refer to the selected CSR as selected sub-channel(s) in the rest of the paper.**

# Impact of Message Variability On The Operation Of The LTE-V2X mode 4 MAC

| IEEE 802.11p MAC | LTE-V2X MAC |
| --- | --- |
| nodes can access the channel at any time if they sense the channel is free | they have to reserve the channel and notify it using RRI in SCI associated to a TB

⇒ pre-defined time-frequency structure |
| not really affected by the size of messages and the time interval between messages | be affected by message varibility |

## Reselection in sensing-based SPS

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a7ca58e4-7e7a-4a2d-925f-51f33b8f02ec/Untitled.png)

- Reselection of vehicle A causes packet collisions
- neighboring vehicles don’t know the fact that A reselect new sub-channel(s) until next TB is transmitted
- If A and another neighboring vehicle select same sub-channel and their Selection Windows overlap, their transmissions can collide.

## Additional Reselection

⇒ Reselection occurs although Reselection Counter of vehicle doesn’t reach 0.

### 1.  various size

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/fa3a1014-048e-404e-9b19-e82a9e3c2080/Untitled.png)

- a new message has bigger size than the previous message so it doesn’t fit in the reserved sub-channel(s).

### 2. various time interval

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/129e8b7e-bdf9-4b89-b70f-638e9797afcf/Untitled.png)

- the time interval between the previous message and a new message is smaller than RRI
- And the latency deadline of the new mesaage is earlier than the next reservation time.
    
    ⇒ $(T_{G2}+100\text{ms}) < T_{R2}$
    
- the vehicle is forced to reselect new sub-channels to transmit TB before the latency deadline

## Unutilized Reservations

### 1. left without notice

⇒ When an additional reselection ouccrs

- If a vehicle decides to select new sub-channel when it transmits the last TB that make Reselection Counter zero, the vehicle sets RRI to zero
- A vehicle doesn’t set RRI to zero in its last transmission if the vehicle its new sub-channel because of additional reselection.
- It cannot inform neighboring vehicles that it will not utilize the previously reserved sub-channels.

### 2. the time interval between messages is larger than the RRI

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/74f06ee7-4a37-4cd9-804b-86028c0ceadf/Untitled.png)

- If a vehicle A wants to transmit a new message at $T_{R3}$ and this message is generated at $T_{G2}$.
- the time interval between messages is larger than RRI
    
    ⇒ $(T_{G2}-T_{G1}) > \text{RRI}$
    
- After $\text{RRI}$, the vehicle cannot reserve its sub-channel(s) at $T_{R3}$.
    
    → can’t announce its utilization
    
- Other vehicles believe then the sub-channels at $T_{R3}$ are free.
    
    → Other vehicles believe that the vehicle A reserve its sub-channel(s) until $T_{R2}$.
    

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/454b1dfc-a149-4c6c-b5a1-6cf560146531/Untitled.png)

1. If another vehicle generate a new message before $T_{R2}$, the sub-channel(s) reserved by the vehicle A is exclude as candidate sub-channels. (Fig. 5.b)
2. If another vehicle generate a new message after $T_{R2}$, the vechicle believes that vehicle A does not use the previous reserved sub-channel(s) ⇒ The risk of collision exists‼️

## Unused Sub-Channel(s)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8b78579e-b81a-4656-8f05-2f2de60e4b20/Untitled.png)

- When new TB is smaller than the reserved sub-channels.
- Then, some of reserved sub-channels will be left unused and other vehicles cannot utilize them.

# Message Generation Models

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9abde13e-8f70-48fc-b93a-3b78bb44845c/Untitled.png)

- fixed or variable size
- fixed or variable time intervals

# Simulation Environment

- network simulator OMNET++
- road traffic simulator SUMO

# Comparison of IEEE 802.11p and LTE-V2X