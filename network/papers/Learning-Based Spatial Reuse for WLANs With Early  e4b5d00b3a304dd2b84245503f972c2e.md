# Learning-Based Spatial Reuse for WLANs With Early Identification of Interfering Transmitters


[Learning-Based Spatial Reuse for WLANs With Early Identification of Interfering Transmitters](https://ieeexplore.ieee.org/abstract/document/8915793)

## Preliminaries: Early Identification of Interfering Transmitters

![Untitled](Learning-Based%20Spatial%20Reuse%20for%20WLANs%20With%20Early%20%20e4b5d00b3a304dd2b84245503f972c2e/Untitled.png)

### State

Using MDP

![Untitled](Learning-Based%20Spatial%20Reuse%20for%20WLANs%20With%20Early%20%20e4b5d00b3a304dd2b84245503f972c2e/Untitled%201.png)

- four tuple $(\Omega, \Alpha, q, R)$
- union and the Cartesian product
    - $\Omega_{\text{MAC}} := \{S_0, S_1, S_2, S_3\}$
    - $\Omega_{\text{BS}}$ 
        - the current backoff stage
        - the times of consecutive transmission failure at present
    - $\Omega_{\text{CH}} := \{0,1,2,\cdots, N\}$
        - index of transmitting interferer that identified by the agent
        - $\omega_{CH}[t] = 0$ : “the channel is ide”l or “the interferer is unable to be identified”
    - $\Omega_{\text{DR}} := \{1, \cdots, K\}$
        - the number of available MCS
        - the currently chosen data rate for transmission

### Action

<ol>
<li> Select Data Rate (in $S_0$) </li>
<li> Choose Whether or not go ignore detected transmission / adjust data rate (in $S_2$) </li>
<li> Continue carrier sensing (in $S_1$ , backoff counter is still not 0) </li>
</ol>>

### Metric and Reward

Given that the agent has successfully transmitted a packet after $J$ **times of consecutive packet transmission failures, the service time $D$

![Untitled](Learning-Based%20Spatial%20Reuse%20for%20WLANs%20With%20Early%20%20e4b5d00b3a304dd2b84245503f972c2e/Untitled%202.png)

- $C_j$ : the duration of the unsuccessful transmission in backoff stage $j$
- $T_J$ : the duration of the successful transmission
- $B_j$ : the backoff countdown duration in backoff stage $j$
- $Y$ : the number of times that agent has freezed its backoff counter
- $F_i$ : the duration that the agent freezes its backoff counter

⇒ 즉, 새로운 Packet이 생성되고 나서부터 성공하기까지 (ACK reicept까지) 걸리는 시간

#### reward

- when transmission failed (from $S_1$ to $S_0$)
    - $-B_j-C_j$
- when transmission succeeded (from $S_1$ to $S_3$)
    - $-B_J-T_J$
- when it has fronzen the backoff counter to wait until the detected transmission ends
(when $a=0$, from $S_2$ to $S_1$)
    - $-F_i$

## Learning-Based Spatial Reuse Operation

### Learning Algorithm


- RUQL (Repeated )
    - learning rate 조절
    - 덜 탐색되는 action에 higher learning rate를 부여
        
        ![Untitled](Learning-Based%20Spatial%20Reuse%20for%20WLANs%20With%20Early%20%20e4b5d00b3a304dd2b84245503f972c2e/Untitled%203.png)
        
    - $\alpha_n$ : the learning rate in the conventional QL algorithm
- $\epsilon$-greedy exploration policy

### Transmit Power Restriction

concurrent transmission에서 on-going transmissions를 보호하기 위해 transmit power를 낮춘다.

![Untitled](Learning-Based%20Spatial%20Reuse%20for%20WLANs%20With%20Early%20%20e4b5d00b3a304dd2b84245503f972c2e/Untitled%204.png)

- $P_{ref}$ : maximum possible transmit power of the agent
- $\Theta_{min} = -82dBm$
    - default CCA threshold of legacy devices
- $I$ : measured interference strength

⇒ inversely proportional to the detected interference strength  

## Numerical Evalution

- Throughput
- MAC Service Time Composition
- Performance Gains Due to Identifying Interferers
- Time-Varying Topology
    - change the location once a second
- Impact to Legacy Transmitters
    - evaluate the percentage of packets transmitted by the OBSS transmitters that are corrupted by the transmission of the agent.
- Multiple Agents

## Analysis of Gains Due to Identifying Interferers


- State Partition : Stationary MDP
- Analysis of Gains Due to Identifying Interferers

## Points

- agent가 현재 topology에 놓인 상황을 state로 표현
- Agent의 MAC service time을 줄이고자 하는 것이 목적
- agents가 10개인 Multi-Agents 환경에 대해서도 실험
    - 각 agent selfish
- The Partitioned MDP의 사용
    - But identifier는 구분하지 않고 단순화 함
    - learning algorithm과 simulation evaluation에서는 사용되지 않음

## Questions

🧐 왜 모든 reward 값을 음수로 설정했는가? 이는 모든 agent의 action이 agent의 goal을 방해한다는 것을 의미하기에 좀 이상한 것 같다.

🧐 왜 adjusting transmit power에 proportional을 썼을까? proportional fairness의 의미?

🧐 Fig 8.의 Throughput 차이가 큰 의미가 있는가? (4개의 transmitters, Mbit/s 10정도의 차이)