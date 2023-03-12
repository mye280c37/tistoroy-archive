
[Comparison of IEEE 802.11p and LTE-V2X: An Evaluation With Periodic and Aperiodic Messages of Constant and Variable Size](https://ieeexplore.ieee.org/document/9133075)

## Introduction

### V2X technologies

| IEEE 802.11p | 3GPP <span class="blue-highlight">(The Third Generation Partnership Project)</span> |
| --- | --- |
| DSRC<span class="blue-highlight">(Dedicated Shot Range Communication)</span> | LTE-V2X <span class="grey-highlight">(based on ‘PC5’)</span> |
| ITS-G5<span class="blue-highlight">(specified in Europe by ETSI)</span> | Celluar V2X <span class="blue-highlight">(C-V2X)</span> <span class="grey-highlight">(based on ‘sidelink LTE radio interface’)</span> |

### Comparison of IEEE 802.11p and LTE-V2X

<ol>
<li> <b>link level</b> : LTE-V2X can improve the link budget over IEEE 802.11p by around 7dB and increase the communication range and reliability at the link level 
</li>
    
<li>
<b>system level</b>:
    <ol>
    <li>The model generating periodic message </li>
    <li><b><span class="yellow-highlight-background">The model generating aperiodic messages of variable size</span></b></li>
    </ol>
    ⇒ IEEE 802.11p outperforms LTE-V2X
</li>

</ol>

## V2X Technologies

### IEEE 802.11p

- simpler and more flexible MAC
- use an OFDM<span class="blue-highlight">(Orthogonal Frequency Division Multiplexing)</span class="blue-highlight">-bsed PHY<span>(physical)</span> layer with a channel bandwidth of 10MHz.
- **DCF<span class="blue-highlight">(Distributed Coordination Function)</span>** of IEEE 802.11
    - **CSMA/CA<span class="blue-highlight">(Carrier Sense Multiple Access with Collision Avoidance)</span>**
    - **CCA<span class="blue-highlight">(Clear Channel Assistant)</span> threshold**
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
- <span class="yellow-hightlight-background"><b>time-frequency resource structure</b></span>

[##_Image|kage@2gBaF/btr2r5sxA0d/txMKZEHnhno08f1IGKH5C0/img.png|CDM|1.3|{"originWidth":1262,"originHeight":770,"style":"alignCenter"}_##]

$$
12 \cdot \text{OFDM sub-carriers} = \text{RB} \\ \sum\text{RB} = \text{channel bandwidth}
$$

- $1\cdot\text{RB} = 180\text{kHz}$
- $1 \cdot \text{sub-carrier} = 15\text{kHz}$
- data → TB로 encapsuled
- control information → SCI 로 encapsuled
- parameters
    - the number of RBs per sub-channel
    - the number of sub-channel per sub-frame
- **sensing based SPS<span class="blue-highlight">(Semi-Persistent Scheduling)</span> scheme**
    - sensing based
        - identify and select sub-channels that are not occupied by other vehicles
        - **RRI <span class="blue-highlight">(Resource Reservation Interval)</span>**
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
        
        <div class="aside">
        <p class="title">
        👉 new sub-channel의 후보가 될 CSRs List를 구성
        </p>
        <div class="contents">
        <ol>
        <li> 같은 sub-frame의 이웃한 sub-channel들로 구성</li>
        <li>지금부터 자신의 Reselection Counter 동안 다른 vehicle에 사용될 예정인 CSR 제외</li>
        <li>RSRP threshold보다 큰 RSRP를 가지는 CSR 제외 *(적어도 전체 20%의 CSR 포함)*</li>
        <li>남은 CSRs 중 lowest average RSSI를 가진 CSRs 추리기 *(전체 20%의 CSR 포함)*</li>
        <li>4번까지 남은 CSR 중에서 random하게 new sub-channel 고름</li>
        </ol>
        </div>
        </div>
        

## Impact of Message Variability On The Operation Of The LTE-V2X mode 4 MAC

| IEEE 802.11p MAC | LTE-V2X MAC |
| --- | --- |
| nodes can access the channel at any time if they sense the channel is free | they have to reserve the channel and notify it using RRI in SCI associated to a TB <br/> <br/> <span class="blue-highlight-background">⇒ pre-defined time-frequency structure</span> |
| not really affected by the size of messages and the time interval between messages | be affected by message varibility |

### Reselection in sensing-based SPS

[##_Image|kage@5Jy5C/btr2znejXMD/Hv13UEIkvXcbqZNvYGBMN1/img.png|CDM|1.3|{"originWidth":970,"originHeight":762,"style":"alignCenter"}_##]

-   Reselection of vehicle A causes packet collisions
-   neighboring vehicles don’t know the fact that A reselect new sub-channel(s) until next TB is transmitted
-   If A and another neighboring vehicle select same sub-channel and their Selection Windows overlap, their transmissions can collide.

### Additional Reselection

⇒ Reselection occurs although Reselection Counter of vehicle doesn’t reach 0.

#### 1\. various size

[##_Image|kage@eawQXL/btr2sW9XWnf/OWZAKwK5fS6oCdXMf0ekY1/img.png|CDM|1.3|{"originWidth":934,"originHeight":618,"style":"alignCenter"}_##]

-   a new message has bigger size than the previous message so it doesn’t fit in the reserved sub-channel(s).

#### 2\. various time interval

[##_Image|kage@cInRUh/btr2sFm3Hy6/k2Ehq5myomgrWTYoQKAWtk/img.png|CDM|1.3|{"originWidth":892,"originHeight":608,"style":"alignCenter"}_##]

-   the time interval between the previous message and a new message is smaller than RRI
-   And the latency deadline of the new mesaage is earlier than the next reservation time.
	-   $(T\_{G2}+100\\text{ms}) < T\_{R2}$
-   the vehicle is forced to reselect new sub-channels to transmit TB before the latency deadline

### Unutilized Reservations

#### 1\. left without notice

⇒ When an additional reselection ouccrs

-   If a vehicle decides to select new sub-channel when it transmits the last TB that make Reselection Counter zero, the vehicle sets RRI to zero
-   A vehicle doesn’t set RRI to zero in its last transmission if the vehicle its new sub-channel because of additional reselection.
-   It cannot inform neighboring vehicles that it will not utilize the previously reserved sub-channels.

#### 2\. the time interval between messages is larger than the RRI

[##_Image|kage@dENrCE/btr2znL9nFb/lj7ffIXwWL3lGMcRRlKA50/img.png|CDM|1.3|{"originWidth":776,"originHeight":498,"style":"alignCenter"}_##]

-   If a vehicle A wants to transmit a new message at $T\_{R3}$ and this message is generated at $T\_{G2}$.
-   the time interval between messages is larger than RRI
	-   $(T\_{G2}-T\_{G1}) > \\text{RRI}$
-   After $\\text{RRI}$, the vehicle cannot reserve its sub-channel(s) at $T\_{R3}$.
	-  can’t announce its utilization
-   Other vehicles believe then the sub-channels at $T\_{R3}$ are free.
	-   <span class="yellow-highlight-background">Other vehicles believe that the vehicle A reserve its sub-channel(s) until $T_{R2}$.</span>

[##_Image|kage@brmtIX/btr2wtFWkVf/UaZKSlfFPEf7sxljxQJDk0/img.png|CDM|1.3|{"originWidth":852,"originHeight":1008,"style":"alignCenter"}_##]

<ol>
<li>If another vehicle generate a new message before $T\_{R2}$, the sub-channel(s) reserved by the vehicle A is exclude as candidate sub-channels. (Fig. 5.b)</li>
<li>If another vehicle generate a new message after $T\_{R2}$, the vechicle believes that vehicle A does not use the previous reserved sub-channel(s) <span class="red-highlight">⇒ The risk of collision exists ‼️</li>
  </ol>

### Unused Sub-Channel(s)

[##_Image|kage@k4sUV/btr2uheSZW9/JwLjdxZxa0R9cBp6pwIkB0/img.png|CDM|1.3|{"originWidth":890,"originHeight":584,"style":"alignCenter"}_##]

-   When new TB is smaller than the reserved sub-channels.
-   Then, some of reserved sub-channels will be left unused and other vehicles cannot utilize them.

## Message Generation Models

[##_Image|kage@cGuwH0/btr2tD3oo8p/rtKsgxmfETnpRsMzBfYV3K/img.png|CDM|1.3|{"originWidth":952,"originHeight":714,"style":"alignCenter"}_##]

-   fixed or variable size
-   fixed or variable time intervals

## Simulation Environment

-   network simulator OMNET++
-   road traffic simulator SUMO