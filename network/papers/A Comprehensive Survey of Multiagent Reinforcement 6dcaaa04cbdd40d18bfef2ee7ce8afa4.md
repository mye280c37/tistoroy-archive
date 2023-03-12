2008년에 쓰인 글이지만 MARL의 큰 틀을 잡고 자 정리해보았다.

[A Comprehensive Survey of Multiagent Reinforcement Learning](https://ieeexplore.ieee.org/document/4445757)

## Benefits and Challenges in MARL

### Benefits

- agent끼리 경험을 공유하면 비슷한 task를 수행하는 경우 서로에게 도움이 된다.

### Challenges

- 차원의 저주
- agent끼리의 상관관계 때문에 서로 학습에 방해가 될 수 있다.
- agent들이 계속 action을 취하고 이에 따라 environment가 변하기 때문에 nonstationary 상태가 된다.
	- environment 뿐만 아니라 other agents도 고려해야 함


## MARL GOAL

<ol>
<li> Stability 
<div class="inbox">
    ➡️ dynamic environments 상황에서도 stationary policy가 convergence 상태에 이르는 것
</div>
</li>
<li> Adaptation
<div class="inbox">
    ➡️  다른 agents의 영향에도 자신의 Performance를 유지하거나 향상시키는 것
</div>
</li>
</ol>

### Stability Property

<ol>
<li> equilibrium learning
<div class="inbox">
<ol>
    <li> converge to a coordinated equilibrium </li>
    <li> Nash equilibria </li>
    <li> stagewise convergence와  Nash equilibria 사이의 상관관계 정확하지 않다는 비판 [14] </li>
</ol>
</div>
</li>
<li>        
convergence
</li>
<li>
opponent independent
</li>

<div class="aside">
<div class="title">
📌 Nash equilibrium
</div>
<div class="contents">
 내시 균형. 게임이론에서 경쟁자의 대응에 따라 최선의 선택을 하면 서로가 자신의 선택을 바꾸지 않는 균형상태.
  
 상대방이 현재 전략을 유지한다는 전제 하에 나 자신도 현재 전략을 바꿀 유인이 없는 상태
  
 어떤 agent a가 다른 policy를 들고와도 return을 최대화할 수 없는 평형상태
  
 출처 : <a href="https://ko.wikipedia.org/wiki/%EB%82%B4%EC%8B%9C_%EA%B7%A0%ED%98%95">https://ko.wikipedia.org/wiki/내시_균형</a>
 </div>
</div>
  
### The  need for Stability

- more amenable to analaysis and meaningful performance guarantees
- reduces the nonstationarity in the learning problem of other agents

### Adaptation Property

<ol>
<li> Rationality
    <div class="inbox">
    <ul>
    <li> converge to best response when the other agents remain stationary. </li>
    <li> [13], [56] 참고 </li>
    </ul>
    </div>
</li>
<li> no-regret
<div class="inbox">
<ul>
    <li> at least as good as the return of any stationary strategy </li>
    <li> agent가 다른 agent에 의해 “being exploited” 되는 것을 막는 게 목적 </li>
    <li> [57] 참고 </li>
</ul>
</div>
</li>
<li> Targeted optimality/compatibility/safety
<div class="inbox">
<ul>
    <li>expressed in the form of average reward bounds</li>
</ul>
</div>
</li>
<li> openent-aware</li>
</ol>

### The need for Adaptation

- 여러 agent들이 환경에 있음으로써 발생하는 “the dynamics”가 예측 불가능하다

## Taxonomy of MARL Algorithms

- Stability에만 집중하는 알고리즘
    - 다른 agent들의 행동을 인식 X
    - independent
- Adaptation도 고려하는 알고리즘
    - need to be aware to some extent of their behavior

### Task 종류

- static (stateless)
- dynamic

### Agent들간의 관계

[##_Image|kage@bc9ZDx/btr3dT4ppxz/4DYzscDDzvhFJp7p4lLztk/img.png|alignCenter|width="100%"|_##]

- fully cooperative
- fully competitive
- mixed

### the organization of the algorithms

[##_Image|kage@bYVbPa/btr3cbLxr7D/hWLySWHAPgM5kIF8PfffF1/img.png|alignCenter|width="100%"|_##]

- temproal-differene RL
- game theory
- direct policy search

### other taxonomy axes (다른 분류 기준)

- Homogeneity vs. Heterogeneous
    - Homogeneity : 모든 agent가 하나의 algorithm 공유
    - Heterogeneous : 각 agent가 서로 다른 algorithm 운영
- model-based vs. model-free
- 다른 agent의 상황을 얼마나 고려하는가
    - none
    - actions
    - actions and rewards

### breakdown of MARL algorithms

[##_Image|kage@vz479/btr3aLfD3Mt/0geiv4790sPAxm4Oys44RK/img.png|alignCenter|width="100%"|_##]

## MARL Algorithms

<div class="aside">
<div class="title">
📌 stationary vs. nonstationary
</div>
<div class="contents">
stationary policy : independent of time
nonstationary policy : dependent of time
</div>
</div>

### Fully Cooperative Tasks

agents들이 공유하는 공통의 return을 최대화하는 것이 main goal이다.

- 모든 agent들이 같은 reward 값을 공유한다.

이 공유되는 값을 중앙에서 관리하느냐 아니냐에 따라 분류할 수 있다.

<ol>
<li> controlled by centralized controller </li>
<li> indepedent agents 가 병렬적으로 common optimal Q-function을 사용 </li>
</ol>

2번의 경우 추가적인 mechanisim이 없다면 각 agent의 action selection이 optimal joint action set을 망칠 수 있다. (break ties randomly)

### Explicit Coordination Mechanisms

coordinated or negotiated mechanisims를 도입해 [coordination problems](https://www.notion.so/A-Comprehensive-Survey-of-Multiagent-Reinforcement-Learning-6dcaaa04cbdd40d18bfef2ee7ce8afa4)을  해결하고자 함

#### How?

모든 agent들이 동시에 “break ties”를 실행

#### 어떻게 동시에 실행하는가?

- social conventions, roles, and communications를 기반으로
- 가장 간단한 방법으로는 agent에 action selection을 하는 순서를 지정해주고, agent마다 action을 고르는 데에도 순서를 부여하는 것
    - agent 1 → 2 순서가 부여됐다고 가정하자
    - agent 1이 먼저 자신의 action order를 기반으로 action을 고르낟.
    - agent 2는 agent 1의 action을 기반으로 자신의 action을 고른다.

### Fully Competitive Tasks

agent들이 fully competitive 관계인 경우는 만약 agent 1, 2가 있을 경우 agent 1은 A라는 task를 이루는 것을 목표로하고, agent 2는 A를 이루지 않는 것을 목표로 할 경우 두 agent가 fully competitive 관계이다.

#### minimax principle

opponent-independent algorithms를 사용한다.

[##_Image|kage@b8cYmf/btr3ccp81YD/2TwGlQ09U50gvbijwV6ydK/img.png|alignCenter|width="100%"|_##]

opponent가 어떤 행동을 하든 관계없이 얻을 수 있는 최소한의 return 값 중 최댓값을 취하는 minimax return을 얻게 된다.

### Mixed Tasks

self-interested agents 나 cooperating agents이나 immidiate interests가 충돌하는 경우 적합

(environment의 resource를 경쟁하는 상황)

Mixed Tasks를 위한 알고리즘은 주로 <span class="text-emphasis">static tasks</span> (i.e., repeated, gerneral-sum games)을 위해 디자인된 것이 많다.

- repeated game에서 agent의 dynamic behavior로 인해 nonstationary 상태가 되기 때문에 <span class="text-emphasis">adaptation</span>이 중요!

#### 1. Single-Agent RL

- single-agent RL 알고리즘을 바로 MARL에 적용
- multi agents 상황으로 인해 발생하는 nonstationary 문제를 해결해야 함

#### 2. Agent-Independent Methods

- agent의 알고리즘이 다른 agents로부터 독립적

[##_Image|kage@CvzCH/btr3al2Nyyf/Mz1yWiweYIreMWT186Xaw0/img.png|alignCenter|width="100%"|_##]

- $\text{solve}_i$ 는 agent $i$의 equilibrium 값을 반환한다
- $\text{eval}_i$ 는 agent $i$의 equilibrium 기대값을 반환한다.
  
[##_Image|kage@uxMIw/btr3a6cSthw/PZlTuSCUHv6J3src6G0fpk/img.png|alignCenter|width="100%"|_##]

위는 Nash equilibrium으로 convergence를 이룰 경우 $\text{solve}_i$ 와 $\text{eval}_i$ 예시이다.

- small class of problems에만 적용 가능하다는 단점이 있다. (다른 case의 경우 추가적인 mechanism이 필요하다

<h5>[ Example ]</h5>

- correlated equilibrium Q-learning (CE-Q)
    - correlated equilibria
- asymmetric Q-learning
    - Stackelberg (leader-follower) equilibria

#### 3. Agent-Tracking Methods

- convergence to stationary strategies 가 필요하지 않음
- 각 agent들은 서로의 action을 모두 알 수 있다고 가정한다.

<h5> Static Tasks</h5>

- fictitious plays
- MetaStrategy
- Hyper-Q

<h5>Dynamic Tasks</h5>
    
- Nonstationary Convergin Policies (NSCP)
- computes a best response

[##_Image|kage@btDDpp/btr3dbEhBF3/Cqvz8URr9b8ZP5oRP1dqYK/img.png|alignCenter|width="100%"|_##]

#### 4. Agent-Aware Methods

- convergence, adaptation 모두 노림

#### 5. Remarks and Open Issues

- dynamic tasks로의 확장
- exact task model을 기반으로 함 (현실성 ⬇️)
- 차원의 저주 → 다른 agent도 observation 해 policy에 반영하기 때문
- 확실하지 않은 observation


## Multi-Agents in Network System

네트워크 시스템에서 노드들에게 가장 중요한 것은 자신의 패킷을 실패하지 않고 보내는 것이고, 자신이 패킷을 보낼 기회를 잡는 것이다.

⇒ 즉, agent 자신이 throughput을 올리는 것을 목표로 한다고 볼 수 있다.
  
<br/>

하지만 시스템 전체에서는 resource가 낭비되지 않는 것, 그리고 edge nodes에게도 전송의 기회를 주는 것이 목표라고 할 수 있다. 

<br/>
  
agent와 시스템 전체의 목표가 같으면서도 단순히 throughput만 높아선 안되기 때문에 서로 상충하는 부분이 있다고 볼 수 있다.

<br/>
  
또한, agent들은 자신의 목표를 달성하기 위해 resource를 공유한다. 즉, 경쟁 관계에 있는 것이다. 하지만 시스템의 목표를 이루기 위해서는 edge nodes에 어느 정도 양보가 이루어져야 하기 때문에 협력은 불가피하다. 기존 프로토콜은 우위 노드들에 패킷 전송 기회가 쏠릴 수 밖에 구조이기 때문이다. (특히 doubling contention window에서)
  
<br/>

따라서 stability와 adaptation을 모두 고려해야 하고, stability의 경우 어느 정도 균형 상태 (edge nodes에 최소한의 throughput을 보장) 하면서도 우위 노드들은 자신의 throughput을 최대화할 수 있는 convergene 상태를 찾아야 한다. (전체 시스템 throughput이 너무 내려가지 않도록) adaptiation의 경우 edge nodes들이 최소한의 throughput 밑으로 내려가지 않는 “no-regret” 상태를 유지해야한다.

## Further Studies

 🍊 추가 문서들 

- MARL 관련 gitbook : [https://kilmya1.gitbook.io/deep-multi-agent-reinforcement-learning/2.-background/2.-background/2.2-multi-agent-settings](https://kilmya1.gitbook.io/deep-multi-agent-reinforcement-learning/2.-background/2.-background/2.2-multi-agent-settings)
- **결합확률분포(Joint Distributions) :** [https://everyday-image-processing.tistory.com/23](https://everyday-image-processing.tistory.com/23)