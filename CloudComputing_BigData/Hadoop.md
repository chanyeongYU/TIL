# Hadoop Platform for Big Data

- Hadoop is the **de facto standard** Big Data **store** and **processing** platform

- de facto standard: 사실상 표준

- 공식 사이트

  - http://haddop.apache.org

- 참고 논문
  - MapReduce: Simplified Data Processing on Large Clusters -> **About Data Processing**
  
  - The Google File System -> **About Data Store**
  
    

### Justifying the "Big Ideas"

- Datacenter 를 하나의 컴퓨터처럼 만들기 위한 4가지의 Big Idea를 기반으로 **Hadoop** 탄생
- Hadoop의 철학



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

​	<img src="..\img\image-20200912222720097.png" alt="image-20200912222720097" style="zoom:80%;" />\



