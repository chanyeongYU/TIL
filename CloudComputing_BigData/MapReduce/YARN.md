# YARN

**Yet Another Resource Negotiator**

- Chapter 4: YARN (Hadoop The Definitive Guide 4th Edition)
- 12장: 하둡 2 소개 (시작하세요! 하둡 프로그래밍)
- Apache Hadoop YARN by Arun C. Murthy et al. (AddisonWesley, 2014)



## EVOLUTION OF HADOOP

#### Hadoop 1.0의 발전과 문제점

- **발전**
  - 하둡은 빅데이터 기술의 표준으로 자리잡음
    - 기존 RDBMS의 한계를 극복
  - 하둡을 중심으로 한 에코시스템(Ecosystem) 활성화



- **문제점**
  - 리소스(자원) 관리 문제
    - **하나의 클러스터에서 다양한 하둡 에코시스템이 적절히 시스템 자원을 할당받고, 할당된 자원이 모니터링되고 해제되는 체계가 미흡**
  - 하둡의 안정성 문제
    - SPOF(Single Point of Failure)인 네임노드의 이중화 문제
    - 데이터노드 블록들이 하나의 네임스페이스만 사용



##### Hadoop 2.0의 등장

- :fire:**YARN**
  - 가장 큰 변화
- 네임노드 고가용성
- HDFS Federation
- HDFS Snapshot
- NFSv3 파일 시스템 지원
- 성능 개선



## YARN

- 하둡 2의 가장 큰 변화
- 하둡 1의 맵리듀스 프레임워크는 반드시 MapReduce API로 개발된 어플리케이션만 실행 가능
- YARN은 맵리듀스 외에 다른 종류의 어플리케이션도 실행할 수 있는 구조
- 하둡 클러스터의 운영체제 역할을 수행



#### YARN의 등장 배경

- 맵리듀스의 단일 고장점(Single Point of Failure, SPOF)
  - **자원 할당과 작업 스케줄링이 일원화되어 있음**
- JobTracker의 메모리 이슈
  - JobTracker는 메모리 상에 전체 작업의 실행 정보를 유지
  - 메모리 부족은 곧 맵리듀스 작업 실행의 걸림돌이 됨
- 맵리듀스의 리소스 관리 방식
  - 맵리듀스는 Slot이라는 개념으로 클러스터에서 실행할 수 있는 태스크들의 개수를 관리
    - 설정한 Map Slot과 Reduce Slot이 모두 사용되고 있을 때는 문제가 없으나, 실행 중인 작업이 Map Slot만 사용하거나 Reduce Slot만 사용하고 있다면 다른 Slot들은 잉여 자원으로 낭비 
- 클러스터 확장성
  - 최대 단일 클러스터는 4,000대, 최대 동시 실행 태스크는 40,000개 가 한계
  - 맵리듀스는 Map과 Reduce라는 정해진 구조 외에 다른 알고리즘을 지원하는데 한계가 있음
- 버전 통일성
  - • 맵리듀스 작업 실행을 요청하는 클라이언트와 맵리듀스 클러스터 버전이 반드시 동일



#### Hadoop 2.0 아키텍처 설계 방향

- JobTracker의 주요 기능 추상화
  - 클러스터 자원 관리와 어플리케이션 라이프 사이클 관리라는 두 가지 핵심 기능을 분리
- 다양한 데이터 처리 어플리케이션의 수용
  - 기존 하둡 1.0은 반드시 MapReduce API로 구현된 프로그램만 실행 가능
  - But, YARN 체제에서는 맵리듀스도 실행 가능한 어플리케이션의 하나
- 확장성
  - YARN은 수용 가능한 단일 클러스터 규모를 10,000 노드까지 확대
  - 실행 가능한 데이터 처리 작업 개수 증가
- 클러스터 활용 개선
  - 자원 관리를 위한 별도의 컴포넌트 개발
    - **ResourceManager**
      - **CPU, Memory 등 실제 가용한 단위로 자원을 관리**
      - **YARN에서 실행되는 어플리케이션들에게 자원을 배분**
- 워크로드 확장
  - 하둡 1은 맵리듀스, 하이브, 피그 등 맵리듀스 기반의 어플리케이션만 실행 가능
  - YARN은 맵리듀스 이외에도 Interactive 질의, 실시간 처리, 그래프 알고리즘 등 다양한 형태의 워크로드 확장이 가능
- 맵리듀스 호환성
  - YARN은 기존의 맵리듀스 프로그램 코드를 수정하지 않고도 실행 가능하게 지원



 <img src="..\..\img\image-20201203110531321.png" alt="image-20201203110531321" style="zoom:80%;" />





## YARN ARCHITECTURE & EXECUTION MODEL

#### Basic Concepts of YARN

- YARN은 클러스터 자원 관리 시스템
- 두 개의 long-running daemon을 통해 핵심 서비스 제공
  - **Resource manager**: Master
  - **Node managers**: Slave
  - **Container**: 제한된 리소스(메모리, CPU 등)로 애플리케이션별 프로세스 실행



- 두 개의 layer
  - **platform layer**: 자원 관리(자원 할당), first-level scheduling
    - 자원할당의 단위는 컨테이너
    - Resource Manager, Node Manager
  - **framework layer**: 어플리케리션 실행 조율
    - ApplicationMaster
    - **MapReduce ApplicationMaster가 알아서 Data Locality 적용**



 <img src="..\..\img\image-20201203112141858.png" alt="image-20201203112141858" style="zoom:80%;" />

 <img src="..\..\img\image-20201203112456649.png" alt="image-20201203112456649" style="zoom:80%;" />





## YARN SCHEDULING MECHANISMS

- 소스가 제한되고 사용량이 많은 클러스터에서 애플리케이션은 종종 대기해야 함
- 정의된 정책에 따라 리소스를 애플리케이션에 할당하는 것이 YARN 스케줄러의 역할



#### Scheduler Options

- **FIFO**
  - 선착순(first in, first out)
  - 간단함, 별도의 설정이 필요 없음
  - 공유 클러스터에는 맞지않음
    - 공유 클러스터에는 Capacity나 Fair
- **Capacity**
  - YARN의 default scheduler
  - 공유 클러스터
  - 별도의 전용 대기열로 작은 작업이 제출되는 즉시 시작될 수 있음
- **Fair**
  - CDH와 같은 일부 Hadoop은 Fair Scheduler가 default scheduler
  - 실행 중인 모든 작업 간 동적으로 리소스 밸런싱
  - 자원을 공평하게 분배
  - 실행 중인 모든 애플리케이션이 동일한 리소스를 공유하도록 할당
  - **오버헤드가 발생할 수 있음**
    - **두 번째 작업이 시작될 때, 첫 번째 작업에서 사용한 컨테이너의 리소스가 비워질 때까지 대기**
  - 종료된 컨테이너를 다시 실행하기때문에 전체 클러스터 효율성이 감소