# BASIC CONCEPTS OF HDFS

#### Big Data & Hadoop

- Hadoop은 Big Data를 처리하고 **저장하는** 사실상의 표준 플랫폼



#### Roots of Hadoop

- 두 개의 논문
  - The Google File System
  - MapRedece:Simplified Data Processing on Large Cluster



#### HDFS Paper

- Yahoo에서 "The Hadoop Distributed File System" 논문 작성(2010)

- Abstract

  ```markdown
  The Hadoop Distributed File System (HDFS) is **designed to store very large data sets
  reliably, and to stream those data sets at high bandwidth to user applications.** In a large cluster, thousands of servers both host directly attached storage and execute user application tasks. **By distributing storage and computation across many servers,** the resource can grow with demand while remaining economical at every size. We describe the architecture of HDFS and report on experience using HDFS to manage **25 petabytes** of enterprise data at Yahoo!
  ```



#### Hadoop Project

- **분산 파일 시스템**을 제공(HDFS) - Store
- 대규모의 데이터셋을 변환하고 분석하는 **MapReduce** paradigm을 사용하는 프레임워크 제공 - Processing
  - 데이터와 Computation을 분할해서 여러개의 호스트에 저장
  - Application Computation을 **데이터와 가깝게** 병렬로 실행 가능
- Computation Capacity(계산 용량), Storage Capacity(저장 용량) and I/O Bandwidth(입출력 처리량)를 **범용 서버를 추가함으로써 확장(Scale-Out)**



#### Hadoop Ecosystem

​	<img src="..\..\img\image-20200912222720097.png" alt="image-20200912222720097" style="zoom:80%;" />



#### Why Distributed File Systems?

- 단일한 물리 머신의 저장용량은 한계가 있음
  - 대규모의 데이터를 분할(Partition)해서 여러 대의 머신에 저장
  - 분할된 데이터들을 관리 할 필요가 있음
  - **Distributed File Systems은** 네트워크로 연결되어 있는 수많은 머신들의 스토리지들을 관리하는 시스템
  - 네트워크로 연결되어 있기때문에 일반적인 로컬 스토리지보다 복잡함
    -  **Biggest Challenges: Node Failure가 일어나더라도 데이터 손실을 어떻게 없앨 것인가?**
- Hadoop은 **HDFS**라는 분산 파일 시스템을 가지고 있음
  - 다른 스토리지 시스템과 통합할 수 있는 파일 시스템 추상화를 가지고 있음



#### The Design of HDFS

- For storing **very large files** with **streaming data access** patterns, running on clusters of **commodity hardware**(범용 하드웨어의 클러스터에서 실행하고 스트리밍 데이터 액세스 패턴이 있는 대용량 파일 저장)
  - Streaming data access
    - Seek은 한번만, 순차적으로 데이터 접근(Sequential Scan)
    - 한번 데이터를 작성면 여러 분석 어플리케이션이 여러번 읽어서 새로운 데이터셋 생성
    - 데이터는 변경되는 것이 아님(Append는 가능)
    - **첫 번째 레코드를 읽을 때 대기 시간(Latency)보다 전체 데이터 집합을 읽는 시간(Throughput)이 더 중요**
    - Random Seek과 성능 차이가 큼
  - Commodity hardware
    - 여러 제조사의 하드웨어 사용 가능
    - **Node Failure가 발생할 확률이 증가함**
- HDFS가 잘 동작하지 않는 상황
  - Low-latency data access
    - Latency보다 Throughput에 최적화 되어 있음
  - Lots of small files
    - Namenode는 파일 시스템의 메타데이터를 메모리에 저장
      - Namenode는 HDFS 구조에서 마스터 역할을 수행하는 Daemon
      - Namenode의 메모리에 따라 파일 시스템의 파일 갯수에 한계가 존재함
      - 작은 파일들이 갯수만 많으면 쓸데없는 메모리만 낭비
  - Multiple writers, arbitrary file modifications
    - 하나의 파일을 여러 Writers가 작성하는것 안됨
      - Single Writer만 작성
    - 파일 수정 안됨
      - Append-Only Fashion
      - 데이터 분석 분야에서 데이터 수정은 필요 없음



#### HDFS Architecture

​	<img src="..\..\img\image-20201013024353205.png" alt="image-20201013024353205" style="zoom:80%;" />

- 메타데이터와 어플리케이션 데이터를 **분리 저장**
  - 메타데이터 전용 서버(**Namenode**)
  - 다른 서버들에 어플리케이션 데이터 저장(**Datanodes**)
  - 모든 서버들은 완전하게 연결
  - 서로 간 TCP 기반 통신

##### HDFS Blocks

- 데이터를 한번 읽고 쓸 때의 단위(Unit)
  - 일반적인 파일시스템보다는 큰 사이즈인 64MB
  - 큰 사이즈의 데이터를다루기 때문에
  - 각각의 Unit은 독립적으로 저장되고 동시에 복제까지 진행
- Seek Time을 줄이기 위해 큰 Block Size
  - **데이터 사이즈가 작아지면 데이터를 Transfer하는 시간보다 Seek Time이 더 길어짐**
- 각각의 Block은 여러 데이터노드에 독립적으로 복제
  - **보통 3개씩 복제(Default)**
    - Failure Recovery
    - Better Performance
      - 데이터를 Co-Location 할수 있는 기회가 늘어남
    - 전체적인 스토리지 가용량에 좋지 않음



##### NameNode

- HDFS 아키텍쳐에서 Master 역할을 수행
- Namespace Tree와 Metadata 관리
  - Metadata: File과 Directory의 속성
    - Ex) ownership, permissions, quotas, and replication factor
  - Namespace Image와 Edit Log형태로 Namenode Daemon이 실행되는 노드의 로컬 디스크에 저장
    - Block의 위치는 영구적으로 저장하지 않음 -> 시스템이 시작할 때 DataNode에서 재구성
- Namespace에 대한 전체적인 Metadata를 **RAM으로 유지**



##### DataNode

- HDFS 아키텍쳐에서 Slave 역할을 수행
- 각각의 Block 복제본을 Datanode에 저장
  - Data
  - Block의 Metadata
    - Ex) Checksums for the Block Data, Generation Stamp
- 시작 프로세스
  - Namespace ID와 SW Version을 확인하기 위한 Handshake
  - 맞으면 NameNode에 등록하고 고유한 스토리지 ID를 지속적으로 저장
- Block Report
  - Block ID, Generation Stamp, Block의 길이를 포함
  - 첫 번째 보고서는 Block 등록과 동시에 전송
  - 그 이후로는 매 시간 전송
    - Namenode 자체가 전체적인 Metadata를 관리하기 때문에 Report를 자주 받을 필요가 없음



##### Maintaining the overall system integrity

- Datanode는 Namenode에게 주기적으로 **Heartbeats** 메시지 전송
  - Default Time은 3초
  - Datanode는 Namenode에게 자신이 살아있다는 것을 알리기 위해
    - 10분 정도 지나도 Heartbeats 메시지가 오지 않으면 해당 Datanode를 제외시키고 새로운 복제본 생성
  - **Heartbeats Information**
    - 전체적인 스토리지 용량, 사용중인 스토리지 공간, 진행중인 데이터 전송 수
    - Namenode가 공간 할당 및 로드 밸런싱 결정에 사용함
- **Piggybacking**
  - Namenode가 Heartbeat 메시지를 받은 후에 Datanode에게 추가적인 지시 전달
    - replicate blocks to other nodes
    - remove local block replicas
    - remove local block replicas
    - send an immediate block report
- **Namenode(Master)가 항상 살아있다고 가정**
  - Namenode가 죽었을 때의 대비책이 없음
  - Single Point of Failure(단일 고장점)



##### HDFS Client

- NameNode 및 DataNode와 통신하여 사용자를 대신하여 파일 시스템에 접근

- **Reading a File**

   <img src="C:\Users\chan\AppData\Roaming\Typora\typora-user-images\image-20201014211953330.png" alt="image-20201014211953330" style="zoom:80%;" />

  - **클라이언트가 데이터노드에 직접 연락하여 데이터를 검색하고, 이름노드에 의해 각 블록에 대한 최상(Client와 가까이 있는)의 데이터노드로 안내**
    - **HDFS가 다수의 동시 클라이언트로 확장 가능하도록 설계됨**
    - 동시 다발적인 요청을 처리하기위해 Namenode가 Main Memory에 Metadata저장

  1. NameNode와 통신해서 Read하려는 Block들의 복제본이 있는 DataNode 목록을 요청
  2. DataNode에 **직접(Directly)** 연락하여 원하는 Block의 전송을 요청



- **Writing a File**

  ​	<img src="C:\Users\chan\AppData\Roaming\Typora\typora-user-images\image-20201014212026781.png" alt="image-20201014212026781" style="zoom:80%;" />

  - DataNode 목록이 PipeLine을 형성
    - 동시 다발적인 요청을 처리하기위해 Namenode가 Main Memory에 Metadata저장

  1. NameNode와 통신해서 Write하려는 Block들의 복제본이 있는 DataNode 목록을 요청
  2. Node간 **PipeLine** 구성 및 데이터 전송
  3. 첫 번째 Block을 다 쓰면 다음 Block의 복제본이 있는 DataNode 목록을 요청

