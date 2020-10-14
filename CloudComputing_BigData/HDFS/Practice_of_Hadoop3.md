# Practice of Hadoop3

### HDFS 운영 실습

#### 파일 시스템 상태 확인

- `# ./bin/hadoop fsck [경로명]`



#### Balancer

- `# ./bin/hadoop balancer`
  - 블록들의 복제본이 부족하거나 과도하게 생성된 경우를 정리
  - 상용 클러스터에서는 데이터노드 중 한 대에서 백그라운드로 실행해 두는 것이 좋음
    - 지속적인 블록 정리
    - 성능에 끼치는 영향은 적음



#### HDFS Admin

- `# ./bin/hadoop dfsadmin -help`
  - 하둡에서 HDFS 관리자 기능을 지원하기 위해 제공



- `# ./bin/hadoop dfsadmin -report`
  - HDFS의 기본적인 정보와 상태를 출력



- `# ./bin/hadoop dfsadmin -safemode [enter|leave]`
  - 네임노드와 데이터노드들간의 블록 리포팅(Block Report)이 완료되기 전까지의 상태를 **안전 모드(safemode)**라고 함
  - 블록 리포팅을 할 때 사용자가 파일을 저장한다면 리포팅 결과 에 오류가 생길 수 있음
    - 안전 모드에서는 파일을 쓰거나 변경 하는 작업이 금지(읽기는 허용)
  - 사용자가 직접 안전 모드를 제어 가능



- `# ./bin/hadoop dfsadmin -saveNamespace`
  - 로컬 파일 시스템에 저장되어 있는 파일 시스템 이미지 파일과 에디트 로그를 현재 버전으로 갱신
  - **안전 모드에서만 실행 가능**
  - 이미지 파일과 에디트 로그의 수정 시간을 확인 가능
  - 네임노드의 로그파일을 통해서도 확인 가능



##### 파일 저장 개수 설정

- `# ./bin/hadoop dfsadmin -setQuota 2 quota_test`
  - HDFS의 디렉토리에 파일들이 과도하게 생성되는 것을 제한할 수 있는 쿼터 설정 명령어 제공
  - **쿼터 수는 지정된 디렉토리까지 포함한 수치**
  - :question: **지정된 디렉토리에 지정값 이상인 하나의 디렉토리를 생성하면?** 
- `# ./bin/hadoop dfsadmin -clrQuota quota_test`
  - 쿼터 적용 해제

##### 파일 저장 용량 설정

- `# ./bin/hadoop dfsadmin -setSpaceQuota [용량] [디렉토리명]`
  - 특정 폴더가 지나치게 많은 용량을 차지하지 않도록 디렉토리에 저장할 파일 크기 설정
  - 쿼터 용량은 기본이 Byte, 숫자 뒤에 m을 붙이면 MB, g를 붙이면 GB, t를 붙이면 TB를 의미
  - **용량을 넘기면 파일이 생성되기는 하지만 용량은 0**
- `#  ./bin/hadoop dfsadmin -clrSpaceQuota [디렉토리명]`
  - 용량 쿼터 해제

