# 자율주행자동차 인지기술 : V2X

[<자율주행 자동차공학 - 정승환 저, 2023년 01월 10일 발행>](https://book.interpark.com/product/BookDisplay.do?_method=Detail&sc.shopNo=0000400000&dispNo=&sc.prdNo=355880312&sc.saNo=002001017&bkid1=category&bkid2=ct028017&bkid3=newbook&bkid4=001) 책을 기반으로 작성된 글입니다.


## V2X

Vehicle to Everything의 약자로 자율주행자동차와 정보를 교환하는 기술을 통칭한다. 자율주행자동차가 어떤 대상과 정보를 교환하느냐에 따라 그 종류가 나뉜다.

최근에는 V2X 기술과 GPS 및 UWB(Ultra WideBand) 기술이 접목되어 V2X 통신 음영 지역과 사각 지역에서도 자율주행 서비스될 수 있는 관련 기술이 연구 개발 중이다.

### 종류

- V2V : Vehicle To Vehicle
- V2P : Vehicle To Pedestrian
- V2I :  Vehicle To Infrastructure
- V2H : Vehicle To Home
- V2N : Vehicle To Network
- V2C : Vehicle To Cloud

### V2V

- 차량간 통신
- 단거리 전용 통신(DSRC)를 이용해 각각의 정보 교환
- 정의된 차량의 ID를 기준으로 위치 정보와 속도 및 조향각도 등의 주행 정보를 교환
- 센서만을 이용하여 인식할 수 없는 주행 돌발 상황에 대한 예측 제어를 가능하게 한다.

### V2P

- 차량과 보행자간의 통신
- 보행자의 위치, 헤딩 정보와 속도 등의 정보를 교환
- 교차로 시작 지점 등에서 장애물로 인해 센서로 보행자를 인식할 수 없는 경우 필요하다

### V2I

- 차량과 RSU(Road Side Unit)와의 정보 교환 기술
- 도로 인프라로는 신호등, 교차로 등이 있다.
    - 신호등의 ID, 위치, 색상 변경 시간 정보 등의 정보를 이용해 자율주행 제어 전략에 활용한다.
    - 교차로 폭 등의 정보도 활용 가능
- 카메라 센서만으로 인식하기엔 변경사항이 이미 적용된 후에 그 사항을 알 수 있어 미리 대처가 불가능하다.

### V2H

- 전기자동차 기반, 전기자동차의 남은 전기 에너지를 가정전원망에 공급할 수 있음
    - 긴급시 활용
    - 전력 판매
- V2B (Vehicle to Building)도 유사

### V2N

- 차량과 모바일 기기와의 일정한 정보 교환
- 보행자 보호, 내비게이션 연동 및 스마트 기기를 이용해 차량의 상태와 정보를 모니터링 할 수 있다.

### V2C

- 자율주행 S/W 업데이트, 개인 정보, 의료 서비스, 금융 서비스, 및 인포테인먼트(infortainment) 데이터를 클라우드 서버를 통해 자율주행자동차 내부 통신망과 연결해 정보 공유 가능
- 뉴스, 음악 및 영화 등의 영상 데이터 전송으로 역할 확장 기대

## V2X 통신 방식

| 구분 | WAVE (DSRC) | Celluar |
| --- | --- | --- |
| 채택 | 유럽 |  미국, 중국 |
| 기반 | WIFI  | 이동 통신망 |
| 표준화 | IEEE (2012년 완료) | 3GPP (2014년 시작) |
| 주파수 | 5.9GHz | 5.9GHz |
| 데이터 전송 속도 | 27Mbps | 100Mbps ~ 20Gbps (5G ~ LTE) |
| 신뢰성 | 95~99 (%) | 95~99 (%) |
| 지연 시간 | 0.1s 미만 | 0.01 ~ 0.1 (s) 미만 (5G ~ LTE) |
| 이동 속도 | 200km/h | 160 ~ 500 (km/h) (5G ~ LTE) |
| 영역 | 1km 미만 | 수 km |

### WAVE

- WAVE 통신의 경우 2012년에 기술이 완성되었기 때문에 기술의 안전성은 확보
- BUT 통신 영역이 범위가 좁고 확장성에 한계가 있다.

### Celluar

- 기존에 구축되어 있는 5G, LTE통신망을 활용한다. (기지국 활용)
- 기존 모바일 통신 인프라를 활용하기 때문에 통신 영역도 넓고 지연 시간도 WAVE에 비해 짧다.
- BUT 초기 통신 인프라망 구성에 많은 비용이 든다.

## SAE J2735 메시지 세트


DSRC(단거리 전용 통신)에서 사용하는 표준 메시지 세트

### BSM

**Basic Saftey Message**

- 차량 ID
- 위도 정보
- 경도 정보
- 주행 속도
- 헤딩 방향
- 조향 각도

등의 **주행 데이터**를 담고 있음

- 제동 제어 상태
- 차량 경고등 점등 상태

등의 **모듈 상태 정보**

- 차량 전폭
- 전장의 제원 데이터

위와 같은 정보를 주변 차량이 수신할 수 있도록 **“주기적”**으로 전송

### CSR

**Common Safety Reqeust**

BSM을 교환하는 차량들을 중심으로 현재 실행되고 있는 안전과 관려된 정보를 담고 있음

수신된 안전정보는 다시 BSM에 추가하여 다음 주기에 전송함

### EVA

**Emergency Vehicle Alert**

긴급 사항 발생시 근방 차량에 경고 메시지를 전송하는 용도

경고에 대한 상세 정보를 담고 있다.

- ATIS (Advanced Traveler Information System) 메시지 기반
- 위도, 경도, 도로 폭 및 제한 속도 등의 정보 포함

### ICA

**Intersection Collision Avoidance**

교차로에 진입하는 차량들에게 충돌 위험에 대한 경고를 전송하는 목적

메시지 전송 매체는 차량 또는 교차로 설치된 도로 인프라 시설 모두 가능

### MAP

**Map Data**

지역 ID, 지도 위도, 지도 경도, 차로 폭, 차로 방향 및 차로 유형, 도로 연결 ID 등의 정보 전달

도로의 특정 위치에서 발생한 이벤트 정보를 SPAT(Signal Phase And Timing) 메시지와 연동해 전달 및 활용할 수 있다.

### PVM

**Probe Vehicle Message**

도로를 중심으로 차량의 이동 경로 정보, 차량 이벤트 데이터를 RSU로 전송

- 현재 위도
- 현재 경도
- 현재 속도
- GPS 정확도와 제동
- 조향 및 가속의 차량 거동 데이터
- 방향 지시등 및 각종 경고등 점등 상태

등의 차량의 현재 상태에 대한 정보 포함

<span class="red-highlight"> ⁉️ BSM과 다른 점은? </span>

### RSA

**Road Side Alert**

차량이 현재 주행하는 도로 상에서 앞으로 발생할 위험 요소 정보를 제공한다.

- 보행자 출현
- 낙하물 출현

과 같은 객체 방해물 속성 정보

- 역주행 차량
- 저속 주행 차량

과 같은 특이 이동 물체 방향과 속도 정보

### SPAT

**Signal Phase And Timing**

신호등이 설치된 교차로에서 현재의 신호등 상태 정보 제공

- 교차로 위치 ID
- 신호등 ID
- 신호 제어기 상태
- 신호 색깔 상태와 현재 신호 유지 시간

## V2X 통신 원리


### WAVE 기반의 IEEE 802.11p

WAVE 기반의 5.9 (GHz) 주파수의 10 (MHz) 대역폭의 IEEE 802.11p 무선 통신 기반의 V2X 통신


#### WAVE 통신 프로토콜

[##_Image|kage@F20N7/btr4LHiVDNz/7dDj6Lqn1OKntV9nnz8oWk/img.png|alignCenter|width="100%"|_##] <figcaption>http://www.fescaro.com/ko/archives/571/</figcaption>

[##_Image|kage@c2AIsd/btr42zQZTpd/SqbPCypt62AuoVvhkUfhsK/img.png|alignCenter|width="100%"|_##]
<figcaption>
https://www.semanticscholar.org/paper/Delay-analysis-and-study-of-IEEE-802.11p-based-DSRC-Yao-Rao/de31c8ced09cfa83abbcabab1fad217f0cdc0e22
</figcaption>
  
- IEEE 1609.1 핵심 시스템 (Core System) 규격 
    - 최상위 계층
    - 정보 데이터 교환 담당
    - 사진에서 saftey/non-saftey applications의 resource manager 역할을 한다.
- IEEE 1609.2 통신 보완 규격
- IEEE 1609.3 네트워크 서비스 규격
- IEEE 1609.4 멀티 채널 운영 규격

### Celluar 통신 방식의 PC5

- V2V 통신의 경우 OBE(On board Equipment) 단말기를 이용, 별도의 주파수를 사용해 1(km) 미만의 PC5 무료 통신망을 구성한다.
- 1(km) 이상의 통신 범위를 요구하는 경우 기지국들간의 무선 접속 Uu 유로 통신망을 이용해 중앙 제어 방식으로 운영한다.

#### PC5 프로토콜

[##_Image|kage@BCmUw/btr41JfhgeX/i8N0u6cDogDnzODN1j1151/img.png|alignCenter|width="100%"|_##]
<figcaption>https://www.autoelectronics.co.kr/article/articleView.asp?idx=3449</figcaption>

- 제어 평면 프로토콜
- 사용자 평면 프로토콜

두 개의 프로토콜로 구성

- 5개 계층으로 구성
    - PHY (Physical)
        - 10~20 MHz 대역폭의 5.9 GHz 주파수 대역
        - 물리 계층 사이드링크(Sidelink) 신호를 이용해 데이터 송신
    - MAC (Media Access Control)
        - 논리 채널과 전송 채널 사이의 매핑과 스케줄링
        - HARQ (Hybrid Automatic Repeat and Qequest) 를 통한 오류 수정
    - RLC (Radio Link Control)
        - 상위 계층으로의 데이터 전송
        - 오류 수정
        - 분할 및 재조립 과정
    - PCDP (Packet Data Convergence Protocol)
        - 제어 평면 데이터와 사용자 평면 데이터의 암호화와 해제 기능
        - 제어 평면 데이터의 오류 검증
    - RRC (Radio Resource Control)
        - 통신 접속 계층과 비접촉 계층의 정보를 전송
    - SDAP (Service Data Adaptation Protocol)
        - 필요에 따른 데이터 패킷 교환 담당

## V2X 장점 및 단점

V2X 통신망을 활용할 경우 차량의 센서에 의존하는 것보다 더 넓은 인지 영역을 보장할 수 있어 안정적인 자율주행 예측 제어가 가능해진다.

⇒ 객체의 정보를 ‘미리’ 받을 수 있음

다양한 객체들과 LOS(Line Of Sight)와 NLOS(Not Line Of Sight) 조건과 상관없이 일정한 인지 성능을 유지할 수 있다.

하지만 주고받는 데이터에 대한 비용, 인프라 설치 및 운영 비용, 그리고 표준화 관련 법적 근거 마련 문제를 해결해야 한다.