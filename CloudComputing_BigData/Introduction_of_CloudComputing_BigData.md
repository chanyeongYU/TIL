# Introduction to Big Data & Cloud Computing

- 명지대 컴퓨터공학과 김직수 교수님 *클라우드 컴퓨팅*  강의 내용



## Big Data

- Big Data is a term for data sets that are so large or comlpex that traditionsal data processing application SW are inadequate to deal with them
- **21세기 원유**
  - **잘 정제하면 혁신적이다**
- 데이터를 얻는 능력, 이해하는 능력, 처리하는 능력, **가치를 뽑아내는 능력**, 시각화하는 능력, 전달하는 능력
- **데이터를 잘 정제해서  가치를 뽑아내기**
- B - KB - MB - GB - TB - PB - EB - ZB - YB
- 기존의 DB는 사라지는 것이 아님, 각각의 target이 다름
- 크게 Big Data Platform기술, 데이터 분석 두가지로 나뉨

### Concepts of Big Data

- **데이터의 규모에 초점을 맞춘 정의**
  - 기존 DB 관리 도구의 데이터 수집, 저장, 관리, 분석하는 역량을 넘어서는 데이터 
- **업무 수행 방식에 초점을 맞춘 정의**
  - 다양한 종류의 대규모 데이터로부터 **저렴한 비용으로 가치를 추출**하고, 데이터의 빠른 수집, 발굴, 분석을 지원하도록 고안된 차세대 기술 및 아키텍쳐



### Why Big Data?

- Science, Engineering, Commerce
- 모든 분야에 걸쳐 빅데이터의 중요성이 점점 늘어남
-  **Science**
- 대형 강입자 충돌기 (Large Hadron Collider)에서 연간 생성되는 데이터 1PB
  - **상당한 규모의 데이터가 생성되고 적절하게 다루는 기술이 필요**
- **Engineering**
  - **데이터 분석 -> 장애 발생 징후 패턴 분석 -> 고장 예측 가능**
- **Commerce**
  - **Customer Data -> Insights -> Competitive Advantages**
- **Convergence**
  - Healthcare + Big Data Technology



### Big Data의 3대 요소 (3Vs)

- Volume(크기), Velocity(속도), Variety(다양성)

- Volume이 중요 요소

  ​	<img src="C:\Users\chan\AppData\Roaming\Typora\typora-user-images\image-20200909202758215.png" alt="image-20200909202758215" style="zoom: 67%;" />

  

​	<img src="C:\Users\chan\AppData\Roaming\Typora\typora-user-images\image-20200909203032651.png" alt="image-20200909203032651" style="zoom:50%;" />

- **Volume - 크기**
  - 일반적으로 수십 TB 또는 수십 PB 이상
  - 확장 가능(Scalable)한 방식으로 데이터를 저장하고 분석하는 **분산 컴퓨팅 기법으로 접근 필요**
    - 기존 데이터 분석을 위해 사용하는 데이터 웨어하우스(DW)같은 솔루션이 소화하기 어려울 정도로 데이터의 양이 급격히 증가
    - Scalable - Scale Up, Scale Out
      - Scale Up: 한 컴퓨터의 자원을 증가시키는 것
      - Scale Out: 여러 컴퓨터를 묶어서 사용 (분산 컴퓨팅)

- **Velocity - 속도**
  - 실시간 처리 (Real-Time Processing)
    - 데이터의 생산, 저장, 유통, 수집, 분석의 실시간 처리가 중요
  - 장기적인 접근 (Batch Processing)
    - 수집된 대량의 데이터를 다양한 분석 기법과 표현 기술로 분석(데이터 마이닝, 기계 학습, 자연어 처리)
  - Analysys of Streaming Data

- **Variable  - 다양성**
  - 정형(Structured)
    - **고정된 핑드에 저장**되는 데이터
  - 반정형(Semi-Structured)
    - 고정된 필드로 저장되어 있지는 않지만 XML, JSON등 과  같이 **메타데이터나 스키마 등을 포함**하는 데이터
  - 비정형(Unstructured)
    - 고정된 필드에 저장되어 있지 않은 데이터
    - ex) 동영상, 사진, 메신저 대화 내용 등..
  - **데이터의 다양성에 따라서 다양한 형태의 데이터 저장소 기술을 사용 (Hadoop, MongoDB 등 ..)**



### Importance of Big Data

- **빅데이터 분석은 그 데이터를 사용하는 조직이 효율적으로 사용하고 새로운 기회를 발견하도록 도움을 줌**

 <img src="C:\Users\chan\AppData\Roaming\Typora\typora-user-images\image-20200909205439502.png" alt="image-20200909205439502" style="zoom:67%;" />

- **비용 절감**
  - **하둡**과 같은 빅데이터 기술, 클라우드 기반 분석 기술이 결합시 상당한 비용 절감을 얻을 수 있음
- 빠르고 향상된 의사 결정
  - **하둡**과 In-Memory Analytics(스파크) 그리고 새로운 데이터 소스의 결합은 즉각적이고 향상된 의사결정을 할 수 있도록 도와줌
- **새로운 제품과 서비스** 
  - 고객의 요구 사항과 만족도를 분석하면 그들이 원하는 서비스를 만들 수 있다.





## Cloud Computing

### What is Cloud?

- **Local machine 대신 연결된 서버에서 프로그램이 실행**되고 네트워크로 연결된 컴퓨터들의 집합
  - 연결된 서버란? 클라우드 사업자가 제공하는 어디선가..
- Virtual Hosting Solution



### Concepts of Cloud Computing

- **Software as Service(Saas)**

  - 클라우드 환경에서 동작하는 응용프로그램을 서비스 형태로 제공
  - is what **YOU** use
  - Gmail, Office365 등 ..
  - SalesForce
    - 기업용 소프트웨어를 클라우드 서비스
  - 개인용 뿐 아니라 기업용 서비스도 제공
    - 기업용이 돈이 더 많이 됨

- **Platform as Service(Paas)**

  - 서비스를 개발할 수 있는 안정적인 환경(Platform)과 응용프로그램을 개발 할 수 있는 **API까지 제공**하는 형태
  - Take care od Maintenance, Upgrades..
  - is what **Developers** use
  - Azure
    - Windows Azure
    - SQL Azure
    - .NET

- **Infrastructure as Service(Iaas)**

  - 서버/스토리지/네트워크 등 HW자원을 필요에 따라서 사용할 수 있게 제공

  - is what **IT Departments** use

  - AWS

    - Elastic Compute Cloud(EC2)
    - Simple Storage Service(S3)

  - Utility Computing을 통해 가장 먼저 구현

  - **Utility Computing**

    **What?**

    - 컴퓨팅 자원을 계량화될 수 있는 서비스로 제공
      - **사용하는 만큼 지불**
    - 가상 머신을 **동적으로 프로비저닝** 할 수 있는 능력

    **Why?**

    - **Cost**
      - Capital(초기 투자 비용)을 줄이고, Operating Expenses(운용 비용)을 늘림
    - **Scalability**
      - 충분한 수용량
    - **Elasticity**
      - Scale Up & Down On-Demand
    - **스타트업들이 매력을 느낌**



- On-Demand Self-Service
  - **원할 때 서비스를 받을 수 있음**
  - **사용자 스스로**
- Resource Pooling
  - 서비스를 필요로하는 고객들에게 **적시에 제공**해줄 수 있어야함
- Broad Network Access
  - 특정 지역에서만 접근하는것이 아니라 **어디서든 접근 가능**
- Rapid Elasticity
  - 자원의 빠른 탄력성
    - **수요에 맞게 자원을 조절**할 수 있음
- **Measured Service**
  - 클라우드 컴퓨팅: 돈을 버는 사업
  - 사용자가 사용하는 만큼 비용 지불
    - **사용량을 측정**



### Enabling Technology: Virtualization

​	<img src="C:\Users\chan\AppData\Roaming\Typora\typora-user-images\image-20200910003326508.png" alt="image-20200910003326508" style="zoom:67%;" />

- Hypervisor 위에 새로운 가상 PC를 만들 수 있음



- #### Hypervisor-Based Virtualization

  ​	<img src="C:\Users\chan\AppData\Roaming\Typora\typora-user-images\image-20200910003636198.png" alt="image-20200910003636198" style="zoom:67%;" />

  - 가상머신을 관리하는 별도의 SW
  - **하나의 물리적인 자원에서 구동할 수 있는 운영체제 종류가 다양해짐**
  - Type1
    - **Server에서 주로 사용**
    - HW와 Hypervisor 사이의 OS가 없음
    - **Bare-metal Hypervisor**
      -  HW위에 Hypervisor가 바로 올라감
  - Type2
    - 보통 사용자들이 일반적으로 많이 사용



- #### Container-Based Virtualization

  ​	<img src="C:\Users\chan\AppData\Roaming\Typora\typora-user-images\image-20200910004406042.png" alt="image-20200910004406042" style="zoom:67%;" />

  - Guest OS가 사라짐

  - **Host OS의 Kernel 기능을 공유**

  - 하이퍼바이저 기반 가상화보다 가벼움

  - Guest는 Host OS를 따라감

    

- **두 기술이 함께 사용**



## Cloud Computing & Big Data 연관성

- **A Source of Problems**
  - 클라우드 기반 서비스는 빅데이터를 생성
  - 클라우드는 빅데이터를 생성하는 기업이 시작하는 것을 쉽게해줌

- **As well as a Solution**
  - On-Demand 형태로 분석 클러스터를 구동
  - Commoditization(흔하게) & Democratization(누구나)



### Why the Cloud & Big Data? Why Now?

- **Cloud becomes "Enterprise Ready"**

  - 초창기에는 중소기업을 대상으로 서비스

  - 2010년 이후로 클라우드 **Security(보안), Compliances(규정 준수)** 제공

    - **금융 정보와 의료 정보와 같이 민감한 정보도 서비스 가능해짐으로써 대기업의 진입**

  - Early Adopters Paved the Way

    - 많은 대기업들이 클라우드를 꺼려했지만 혁신적인 기업들이 클라우드로 시작함으로써 길을 닦음
      - Airbnb, NETFLIX 등 ...

  - Enterprises are Feeling the Pain

    - 대기업 스스로 빅데이터를 저장하고 관리하는 것이 부담이 됨

    - 클라우드는 SW와 Infrastructure 관리의 부담을 많이 줄여줌

    - 클라우드 제공자를 일반 기업들이 따라가기 힘듦

      - **클라우드를 사용한다면 기업의 서비스들이 항상 최신의 기술이 적용될 수 있도록 보장**

      - **효율적으로 기업이 빅데이터 문제를 해결할 수 있도록 도와줌**

        

**=> Cloud와 Big Data는 서로 함께 사용 (상호유기적으로 연결)**

#### **클라우드는 가상화에 기반한 서비스, 빅데이터는 많은 데이터 저장하고 분산 처리하는 기술**

#### **클라우드가 큰 개념이 됨**



​	<img src="C:\Users\chan\AppData\Roaming\Typora\typora-user-images\image-20200910012700362.png" alt="image-20200910012700362" style="zoom:67%;" />



## Big Data 다루기

### Divide & Conquer

- **병렬화**

  - Challenges

    - 어떤 방식으로 여러 worker에 작업을 분배할 것인가?
    - worker들보다 더 많은 수의 작업들이 있다면?
      - **로드 밸런싱**
    - worker들이 실행 결과들을 서로 공유해야 한다면
    - 중간 결과물들을 어떻게 하나로 합칠 것인가?
    - 모든 worker들의 작업이 완료된 것을 어떻게 알것인가?
    - worker들이 죽게된다면?
    - 데이터 센터에서 또는 센터들끼리 할때 더욱 어려워짐
    - 디버깅도 어려움

  - 현실적으로

    - Lots of one-off solutions
    - 프로그래머들에게 부담이됨

  - **Synchronization Mechanism**이 필요

  - 문제를 해결하기 위해 **Programming Model**과 **Design Patterns**이 생김

    - **Programming Model**

      - Shared Memory
        - Scale Up만 가능 Scale Out은 안된다는 문제점
      - Message Passing

    - **Design Patterns**

      - Master-Slaves

      - Producer-consumer Flows

      - Shared Work Queues

         <img src="C:\Users\chan\AppData\Roaming\Typora\typora-user-images\image-20200912210158659.png" alt="image-20200912210158659" style="zoom:67%;" />



### Making the Datacenter as a Computer

- Datacenter가 하나의 커다란 컴퓨터라면?
- **추상화 레벨을 어떻게 잡아갈 것인가?**
  - 폰-노이만 아키텍쳐(형대 컴퓨터)를 넘어서서
  -  Datacenter의 Instruction set는 무엇인가?
- **개발자들에세 System-Level위 디테일을 숨기기**
  - 병렬화의 현실적인 문제들(Lots of one-off solutions, Custom Code)로 인해 개발자들에게 부담을 주지 않기위해
- **무엇을 할지와 어떻게 할지는 명확히 구분**
  - 무엇을 할지 알려주면 어떻게 할지는 알아서 하도록



#### Big Ideas

- Scale **OUT**, not UP
  - Scale Up으로는 컴퓨터 하드웨어적인 한계가 존재하기 때문에
- Move processing **close to** the data
  - Cluster(컴퓨터들의 집합)는 Bandwidth의 한계를 가지고 있기 때문에
- Process data **sequentially**, avoid random access
  - seeks are expensive, Disk throughput is reasonable

- Seamless **Scalability**



#### Scale "OUT"

- 어떤 Computer도 빅데이터를 담기에 충분하지 않음
- 컴퓨터들간의 통신이 필요
  - intra-node(한 컴퓨터 내에서): ~100ns
  - inter-node(다른 컴퓨터들간): ~ 100us
  - 1000배 차이



#### Storage Hierarchy

- Scale Out하면서 용량은 증가하지만 Bandwidth가 떨러짐

​	<img src="C:\Users\chan\AppData\Roaming\Typora\typora-user-images\image-20200912212114514.png" alt="image-20200912212114514" style="zoom:67%;" />



- 해결하기 위한 아이디어로 "**Move processing close to the data"**



#### Seeks vs Scans

가정: 100Byte 레코드로 구성된 1TB크기 데이터 베이스

- Random Access 할 때
  - 최대 35Days
- 모든 레코드를 Rewrite할 때
  - 100Mb/s로 가정하면 대략 5.6시간
- **결론 -> Avoid Random Seeks**



### Justifying the "Big Ideas"

- Datacenter 를 하나의 컴퓨터처럼 만들기 위한 4가지의 Big Idea를 기반으로 **Hadoop** 탄생
- Hadoop의 철학





## Hadoop Platform for Big Data

- Hadoop is the **de facto standard** Big Data **store** and **processing** platform

- de facto standard: 사실상 표준

- 공식 사이트

  - http://haddop.apache.org

- 참고 논문

  - MapReduce: Simplified Data Processing on Large Clusters -> **About Data Processing**

  - The Google File System -> **About Data Store**

    

### Hadoop 이란?

- **대용량 데이터를 분산 처리**할 수 있는 자바 기반의 **오픈소스** 프레임워크

- 구글이 논문으로 발표한 MapReduce와 GFS를 DOug Cutting이 구현한 결과물

- 분산 파일 시스템인  **HDFS(Hadoop Distributed File System)**에 데이터를 저장하고, 

  분산 처리 시스템인 **MapReduce**를 이용하여 데이터를 처리



### Why Hadoop?

- 엄청나게 많고 다양한 종류의 데이터



### 하둡의 장점

- 오픈소스 프로젝트이므로 비용에대한 부담이 적음
- **Commodity HW** (x86CPU x Linux)
- **Scale Out** 아케텍쳐
- 데이터 유실이나 장애 복구 가능 (**데이터 복제**)
- 여러 대의 서버에 데이터를 분산 저장하고, 데이터가 저장된 각 서버에서 동시에 데이터를 처리 (**Data Locality**)



### Misunderstanding about Hadoop

- RDBMS를 대체하는 것이 아님
  - 하둡은 RDBMS와 상호 보완적인 특성을 가짐
    - ETL(Extraction, Transformation, Loading)과정의 효율적인 구현
  - Hadoop은 신속한 데이터처리, 즉 트랜잭션이 매우 중요한 데이터를 처리하는대 부적함
- 하둡은 NoSQL?
  - 하둡 플랫폼 구성 요소 중의 하나인 HBase를 통해 NoSQL 기능을 제공하기는 함
  - NoSQL
    - RDBMS가 분산 환경에 적합하지 않지만 NoSQL
    - 키-값으로 구성



### Hadoop's Problem

- **고가용성(HA: High Availability) 지원**
  - 고가용성은 99.999% 상태의 가용을 지원
    - 1년 중 30분 제외하고 서비스가 가능한 수치
  - Name Node의 중앙집중적인 메타데이터 관리
    - **Single point od failure & contention**
      - 하둡의 Master-Slave 패턴 사용함으로써 고질적인 문제점
      - 단일 고장점
      - 한곳에만 집중되는 것
- **File Namespace 제한**
  - Name Node가 관리하는 메타데이터는 메모리로 관리
    - **메모리 용량에따라 HDFS에 저장되는 파일과 디렉토리 갯수 제한**
- **데이터 수정 불가**
  - 한번 저장한 파일은 수정 불가
  - 파일의 이동이나 이름 변경은 가능
  - 저장된 파일의 내용을 수정할 수 없음
    - **파일 읽기나 배치 작업만이 Hadoop에 적합**
  - 기존 저장된 파일에 내용을 Append하는 기능은 제공
  - **WHY?**
    - DB와 하둡과 스파크 또는 Data Warehouse같은 데이터 분석 분야의 특성을 이해가 필요
    - 기존의 DB는 OLTP(Online Transaction Processing)
    - 데이터 분석은 OLAP(Online Analytical Processing)
      - **새로운 데이터를 추가하는것은 가능하지만 분석의 대상이 되는 데이터를 직접 수정할 필요가 없음**
      - **데이터를 분석해서 새로운 결과데이터를 생성하는 것이 맞음**
      - 전통적인 데이터 분석 시스템도 마찬가지
- **POSIX 명령어 미지원**
  - 기존 파일 시스템에서 사용하던 rm, mv 등과 같은 POSIX 형식의 파일 명령어 사용 불가
  - 하둡에서 제공하는 별도의 Shell Command/API필요
- **전문 업체 부족**
  - 전문성 부족
  - 자체적인 기술력과 노하우 확보 전략이 필요



### Major Hadoop Distributions

- Cloudera
- Hortonworks (클라우데라에 합병)
- MapR(HPE에 인수)



### Hadoop Ecosystem

​	<img src="C:\Users\chan\AppData\Roaming\Typora\typora-user-images\image-20200912222720097.png" alt="image-20200912222720097" style="zoom:80%;" />\



