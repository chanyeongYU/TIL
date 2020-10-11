## CLOUD PROGRAMMING MODEL: MAPREDUCE

#### Introduction

- 최근 컴퓨터 세계는 **전통적인 비중앙화된  분산 시스템 아케텍쳐(Grid Computing)에서  중앙 집중화된 클라우드 컴퓨팅 시스템 아키텍쳐**로 변화

- 클라우드 컴퓨팅에 대한 관심이 증가한 이유

  - 저렴한 시스템 하드웨어
  - 컴퓨팅 파워와 Storage 용량의 증가
  - 디지털 미디어 매체, 과학 등 에서 발생되는 매우 많은 양의 데이터

- **클라우드 컴퓨팅의 도전과제**

  - **어떻게 많은 데이터셋을 효율적으로 저장하고 처리하고 분석, 활용 할 것인가?**

- 기존의 데이터 집약적인 시스템(ex, Super Computer)은 클라우드 컴퓨팅에 효율적이지 않음

  - 기존의 시스템은 **Data to Computing Paradigm** 이기 때문에
    - 각각의 Task들이 실행 될 때 필요한 데이터를 실행되는 Computing Node로 전송
    - **네트워크로 데이터를 전송할 경우 인터넷 상에서 병목현상이 발생할 수 있음**

- 새로운 패러다임이 필요

  - **컴퓨팅과 데이터 자원이 함께 위치(co-loacted)되어야 함**
  - **통신 비용을 최소화**
  - **로컬 디스크를 사용하여 I/O속도의 증가**

  **:point_right: Computing to Data: 데이터의 크기가 크기때문에 데이터를 카피하는 것이 아니라 데이터가 저장되어 있는 곳에 Task를 보냄**

   <img src="..\img\image-20201011203308301.png" alt="image-20201011203308301" style="zoom:80%;" />



#### Cloud Computing Programming

- Google이 새로운 데이터 집약적 패러다임인 **Google MapReduce**를 구현
  - Google File System위에서 동작
  - 데이터를 chunk로 분할하고, 각각의 chunk를 복제
  - 데이터 처리가 데이터 Storage와 함께 작동
    - **Data Locality(데이터 인접성)를 효율적으로 활용**
  - **MapReduce는 단순함(Simplicity), 내고장성(Fault Tolerance), 확장성(Scalability) 등의 기능으로 데이터 집약형 클라우드 컴퓨팅 프로그래밍을 강력하게 구현**
- MapReduce는 다양한 분야에 활용
  - machine learning, graphic programming, multi-core programming 등..
  - 유명한 오픈소스 프로젝트 **Hadoop**



#### MapReduce Programming Model

- **MapReduce는 많은 대규모의 컴퓨팅 문제를 해결하기 위한 SW Framwork**
- **Map과 Reduce 함수를 통해 추상화 레벨 제공**
  - **Map**: 독립적, 동시다발적으로 실행되는 Task들을 구현
  - **Reduce**: 독립적으로 수행한 결과를 하나로 모음(aggregate)
- **주요 특징**
  - **Data-Aware**
    -  MapReduce-Master node가 Map Task를 스케쥴링 할 때 **데이터의 위치정보를 고려**
  - **Simplicity**
    - MapReduce Runtime 시스템은 병렬화와 동시 제어를 담당
    - **프로그래머는 병렬 분산 어플리케이션을 쉽게 설계 할 수 있음**
  - **Manageability**
    - 데이터와 연산이 GFS를 이용하여 할당
    - **Input/Output 데이터를 쉽게 관리할 수 있음**
  - **Scalability**
    - **시스템의 노드를 추가(Scale-Out)하면 작업 성능이 향상**
    - 약간의 잠재적 성능 손실이 있을 수 있음
      - 통신할 노드들이 많아지기 때문에
      - MapReduce에서는 Reduce에서만 통신을 많이 하기 때문에 손실이 크지 않음
  - **Fault Tolerance and Reliability**
    - GFS의 복제 기능을 활용 
      - 호스트 노드가 종료되거나 작업이 실패되면 다른 노드에서 다시 실행
      - 속도가 느려진 Task(**stragglers**)가 발생하고 작업의 병목 현상이 발생될 때 백업 Task 실행
    - **사용자가 별도로 설정해주는 것이 아니라 MapReduce가 알아서 실행**



