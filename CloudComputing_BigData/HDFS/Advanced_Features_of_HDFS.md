# ADVANCED FEATURES OF HDFS

### HDFS Federation

- Namenode는 모든 파일과 Block에 대한 Metadata를 메모리에 저장
  - 클러스터의 Size가 커짐에 따라 확장의 제한이 있음
- **Namenode를 추가 함으로써 클러스터 확장을 가능하게 함**
  - 각 Namenode는 파일 시스템 Namespace의 부분을 관리
  - Namenode간에는 통신을 하지 않음
  - Datanode가 여러 Namenode에 등록



### HDFS High Availability

- 데이터 손실을 보호하기 위해

  - 여러 파일 시스템에 Namenode의 Metadata를 복제
  - 보조 Namenode로 Checkpoint 생성
    - **하지만, 고가용성(High Availability)을 제공하지는 않음**
      - High Availability는 1년 중 다운된 시간이 30분 이하

- Namenode는 Single Point of Failure(SPOF)

  - Namenode가 다운되면 MapReduce jobs를 포함한 모든 클라이언트 사용 불가
  - 새로운 Namenode가 작동할 때 까지 모든 Hadoop 시스템은 작동 불가

- Namenode를 복구하는 것

  - 관리자가 메타데이터 복제본으로 새로운 Namenode를 시작하고 Datanode와 클라이언트가 새로운 Namenode를 사용하도록 설정
  - 새로운 Namenode를 사용하기 위해 필요한 것
    - Namespace 이미지를 메모리에 저장
    - Edit Log 다시 시작
    - 충분한 Block Report를 받아 안전모드 해제

  **:point_right: 큰 규모의 클러스터인 경우 Namenode를 복구하는 데 30분 이상이 걸림**

- Hadoop 2 added support for HDFS High Availability

  - **Active-Standby**로 두 개의 Namenode 구성

  - Active중인 Namenode가 고장나면 Standby된 Namenode가 빠른 시간안에 서비스를 계속 수행

     ​	<img src="..\..\img\image-20201015023923968.png" alt="image-20201015023923968" style="zoom:80%;" />

    - Active-Standby 두 Namenode가 Shared Storage 형태로 공유
    - Datanode는 Block Report를 두 Namenode에 전송

    :point_right: 전체적인 시스템 정보를 두 Namenode가 공유

  - 몇가지 아키텍쳐의 변화

    - Edit Log를 두 Namenode간 공유하기 위해 Shared Storage사용
    - Datanode는 Block Report를 두 Namenode에 전송
    - Active Namenode가 고장났을때 클라이언트가 오류 해결을 처리하도록 설정
    - Hadoop1에서의 보조 Namenode의 역할은 Standby Namenode로 흡수

    **:point_right: Namenode를 최신 상태로 복구하는 데 짧은 시간(몇 십초)이 걸림**



### Hadoop 3.0 Major Features

- 최소 Java Version 7 -> 8

- **Erasure Encoding**

  - HDFS에서 기본적으로 Data Block을 세 개로 복제
    - 200%의 오버헤드 발생
    - 네트워크 대역폭과 같은 리소스를 소비
  - Erasure Encoding 지원
    - 적은 공간 오버헤드로 데이터 저장 및 내결함성 제공
    - **3 x Replication의 스토리지 오버헤드는 어느정도 해결하지만 성능부분은 포기**

   <img src="..\..\img\image-20201015025712696.png" alt="image-20201015025712696" style="zoom:80%;" />

- 두 개 이상의 Namenode 지원