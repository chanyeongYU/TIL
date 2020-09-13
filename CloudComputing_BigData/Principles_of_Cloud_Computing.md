# BASIC CONCEPTS & FEATURES OF CLOUD COMPUTING



## Cloud Computing in a Nutshell

- 전기기기를 사용할 때 전력이 어떻게 생성되고 공급되는 방식을 몰라도됨
  - 전기의 추상화, 가상화
- 위와 같이 일상 생활의 개념을 Information Technologies에 적용시키면
  - 유용한 컴퓨터 기능들을 어떻게 동작하는지 몰라도 사용할 수 있음
  - 자체가 **Virtualized**
  - Processing, Storage, Data, SW Resources
- **Cluster, Grid, Cloud Computing는 같은 목적을 가지고 있었음**
  - 대용량의 Computing Power를 완전한 가상화된 형태로 제공하는 목표
  - Cluster: 컴퓨터들을 묶어 놓은 것(하나의 로컬에 묶여있는 것)
  - Grid: 넓은 지역에 분산되어 있는 클러스터들을 묶어 놓은 것
  - 컴퓨팅을 Utility 형태로 제공
- **Utility Computing**
  - 필요한 만큼 제공을 받고 사용한 만큼 비용을 제공(**Pay-As-You-Go**)
  - 전기, 수도, 통신 같은 공공 Utility Service와 비슷
- 클라우드 컴퓨팅은 온디맨드 서비스의 포괄적인 용어가 됨
  - 초창기에 상업적인 용도로 제공(Such as Amazone, Google, MS ...)
  - 전세계 어디서든 클라우드에서 제공되는 서비스를 사용 가능
  - Computing, Storage, SW를 서비스로서(as a Service) 제공



#### Main Characteristics od Cloud Computing

- **Pay per User**
  - 사용한 만큼 돈을 지불
  - 사용한만큼 측정(metering)
- **Elastic Capacity**
  - 탄성이 있는
  - 필요할 때마다 늘렸다가 줄였다가
- **Illusion of Infinite Resources**
  - 무한한 자원을 제공하는 것처럼 느껴짐
- **Self-Service Interface**
  - 사용자 스스로 서비스
- **Abstracted or Virtualized Resources**
  - 자원들의 추상화
    - 실제로 어떻게 동작하는지 알 필요 없음
    - 실제로 클라우드컴퓨팅을 구현할 때 가상화 기술 사용



- Cloud is not only for raw computing and storage
  - 단순히 서비스와 스토리지만 제공하는것이 아니다
  
  - 클라우드 컴퓨팅의 최종적인 목표는 **사용자들의 모든 IT infrastructure가** 
  
    **클라우드에서 가능하도록** 하는것
  
- 유틸리티 형태로 컴퓨팅을 제공하는 것은 컴퓨터 공학자의 오랜 목표

  - 클라우드 컴퓨팅이 등장 함으로써 실현 가능해짐
  - 지난 수년간 여러 기술들이 발전하고, 그 기술들이 모여서 클라우드 컴퓨팅이 구현



## Roots of Cloud Computing (클라우드 컴퓨팅의 뿌리)

​	<img src="C:\Users\chan\AppData\Roaming\Typora\typora-user-images\image-20200913025406714.png" alt="image-20200913025406714" style="zoom:80%;" />

- 클라우드 컴퓨팅은 혼자 갑자기 나타난 것이 아니라 다양한 분야의 기술이 발전되고 

  접목되면서 나타남

  

#### From Mainframes(On -Premise) to Clouds

- From in-house generated computing power into **utility-supplied** computing resources 

  delivered over the Internet as Web services

- **Benefits**
  - Benefits to Consumers
    - IT관련 비용 절감
      - 투자 비용, 인건비 감소
      - 클라우드 제공자로부터 더 저렴한 서비스를 고를 수 있음
    - On-Demand
      - 원할 때 서비스를 받을 수 있음
      -  예측하기 어려운 Computing Needs에 맞출 수 있음

  - Benefits to IT services Providers
    - 운영비 개선(achieve better operational costs)
      - 하나의 서버를 여러개로 가상화하여 운영하면 운영비 절감
    - 효율성과 ROI(Return On Investment, 투자수익률)가 높아지고 TCO(Total Cost of Ownershop, 총소유비용)를 낮춤

- Key enablers of computing delivered as a utility(유틸리티로 제공되는 컴퓨팅의 핵심요소)

- 조직의 컴퓨팅 자원이 Underutilized되고 있음
- 빠른 광 네트워크의 발전과 컴퓨팅파워 공유를 가능하게 하는 기술 출현
- **클라우드 사업자들이 사용자들의 컴퓨터에서 사용할 수 있을 속도와 신뢰성을 제공**



#### Internet Technologies(SOA, Web Services, Web 2.0, and Mashups)

- **Web Service**
  - 표준으로 정의되어 있음
  - SW Intergration에 기여
  - 웹서비스를 통해서 통신할 수 있음
  - 하나의 어플리케이션이 정보를 다른 어플리케이션에서 사용할 수 있도록 해줌
  - 주로 HTTP를 사용해서 XML형태로 데이터를 주고받는 것
    - 서비스들을 묶어서 더 큰 서비스를 만들 수 있음

- **SOA(Service Oriented Architecture)**
  - 서비스에 기반한 아키텍쳐
  - 웹 서비스들을 묶어서 Loosely-Coupled, Standard-Based, Protocol-Independent한 분산 컴퓨팅
  - Service 형태로 패키징
- **Saas(Software as a Service)**
  - 같거나 서로 다른 Service Provider들의 서비스를 묶어서 클라우드 어플리케이션을 구축할 수 있음
  - 기존 시스템이 없을 경우 서비스는 재사용될 수 있고 결합 할 수있음 



#### Distributed Computing(Utility & Grid Computing)

- **Grid Computing**
  - Utility Computing의 개념을 어느정도 실현한 패러다임
  - 클라우드 컴퓨팅과는 약간 다름
    - 그리드 컴퓨팅은 연구용(research computing), 과학응용 쪽에 초점이 맞춰져있음
    - 클라우드 컴퓨팅은 상업용
  - 여러 장소에 분산되어 있는 자원들을 모아서 하나처럼 사용하기위해 서로 통신하기 위한 표준 프로토콜이 필요
    - **Web Service Protocol 사용해서** 분산된 리소스를 단일 가상 시스템으로서 관리, 액세스, 모니터링 등을 할 수 있도록 함

  - **문제점**
    - QoS보장이 어려움
      - Lack of Performancs Isolation
        - 한 사용자의 액티비티를 제어하기 힘듦
        - 다른 유저의 사용에도 영향을 끼침
        - 시간 제약이 있는 어플리케이션에 맞지않음
    - a Portability Barrier
      - 다양한 SW를 지원하는게 어려움
      - 특정 환경을 설치하려면 각각의 Admin에 따로 설치해야함
    - 과헉적인 가치는 컸지만, 상업적으로는 부족했음



### Hardware(Hardware Virtualization, 가상화) :star:

​	<img src="C:\Users\chan\AppData\Roaming\Typora\typora-user-images\image-20200913144411210.png" alt="image-20200913144411210" style="zoom:67%;" />

- 클라우드 서비스는 수많은 컴퓨터로 구성된 대규모의 데이터 센터에서 제공됨
  - 다양한 형태의 시스템을 지원해야함
  - **가상화 기술(Hardware Virtualization)이 가능하도록 도와줌**
- **Portability Barrier 문제점을 해결할 수 있음**

- **물리적인 하나의 플랫폼에서 다양한 형태의 운영체제와 SW stack을 실행할 수 있도록 지원**

- **VMM(Virtual Machine Monitor)** Called a **Hypervisor**
  - 물리적인 HW와 각각의 Guest OS와 통신, 액세스를 중재
  - Guest OS: Hypervisor위에 올라가는 각각의 가상머신을 위한 운영체제

- 여러 혁신적인 기술의 등장으로 가상화 시스템을 적용하게 도니느 계기가 됨

  - **Multi-core chips**
    - 싱글 코어에서는 가상화를 운영하기 부담
  - **Paravirtualization**
    - 반가상화
    - 가상화의 성능을 높이기위해 일부 자원을 가상화하지 않음
  - **Hardware-Assisted Virtualization**
    - 하드웨어 차원에서 가상화기술 지원
  - **Live Migration of VM**
    - 가상머신이 실행 도중에 호스트간 이동하는 것

- **장점**

  - 공유(sharing) 및 이용률(utilization)증가
  - 관리용이성 증가
  - 신뢰성 향상
  - 최근에는 연구진과 실제 이용자의 입장에서 크게 3가지 장점이 나옴
    - **Isolation(고립성)**
      - 보안 측면에서 향상
      - 신뢰성 향상
      - 성능 향상
      - 컨테이너 기반 가상화는 서로 영향을 끼칠 수 있음
    - **Consolidation(통합성)**
      - 시스템 활용률(System Utilization) 증가
    - **Migration**
      - 실행중인 가상머신을 이동시키는 것
        - 하드웨어 관리나, 로드 밸런싱, 시스템 복구 등의 이유로..
      - Live Migration은 가상환경을 종료시키지 않고 이동

  

