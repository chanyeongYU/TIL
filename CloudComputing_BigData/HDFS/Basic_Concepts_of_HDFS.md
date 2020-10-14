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
- HDFS Blocks
  - 데이터를 한번 읽고 쓸 때의 단위
  - 일반적인 파일시스템보다는 큰 사이즈인 64MB
    - 큰 사이즈의 데이터를다루기 때문에
    - 

