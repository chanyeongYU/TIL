## 역정규화 개요



### 1. 역정규화 개요

- **조회 성능향상(Read Performance)**
- 운영(MAINTENANCE)의 단순화
- 의도적으로 중복(Redundancy) 수행
- **데이터무결성이 깨질 수 있는 위험을 무릅쓰고 데이터를 중복하여 반정규화를 적용하는 이유는 데이터를 조회할 때 디스크 IO량이 많아서 성능이 저하된다든지 경로가 너무 멀어 조인으로 인한 성능저하가 예상이 된다든지 컬럼을 계산하여 읽을 때 성능이 저하되는 경우가 예상되는 경우 반정규화를 수행**



#### 반정규화의 종류

- 테이블 중복
- 컬럼 중복
- 관계 중복



### 2. 역정규화 기준 

- 각각의 엔터티와 속성, 관계에 대해 **데이터의 정합성, 데이터의 무결성을 가장 우선으로 할지 아니면 데이터베이스 구성의 단순화와 조회성능을 우선으로 할지 고려**

 <img src="C:\Users\chan\AppData\Roaming\Typora\typora-user-images\image-20201208223630747.png" alt="image-20201208223630747"/>





### 3. 역정규화 절차

1. **역정규화 대상 조회**

   - 범위처리빈도수 조사
     - 항상 일정한 범위만을 조회하는 경우
   - 대량의 범위 처리 조사
     - 대량의 데이터 범위를 자주 처리하는 경우에 처리범위를 일정하게 줄이지 않으면 성능을 보장할 수 없을 경우
   - 통계성 프로세스 조사
     - 통계성 프로세스에 의해 통계 정보를 필요로 할 때 별도의 통계테이블(역정규화 테이블)을 생성
   - 테이블 조인 개수 
     - 테이블에 지나치게 많은 조인이 걸려 데이터를 조회하는 작업이 기술적으로 어려울 경우 역정규화를 검토

2. **다른 방법 유도 검토**

   - 뷰(VIEW) 테이블
     - 지나치게 많은 조인이 걸려 데이터를 조회하는 작업이 기술적으로 어려울 경우 뷰(VIEW)를 사용
   - 클러스터링 적용
     - 대량의 데이터를 특정 클러스터링 팩트에 의해 저장방식을 다 르게 하는 방법(조회중심의 테이블에만 적용가능)
   - 인덱스의 조정
     - 인덱스를 통해 성능을 충분히 확보할 수 있다면 인덱스를 조정 하여 역정규화를 회피
   - 응용어플리케이션
     - 응용 애플리케이션에서 로직을 구사하는 방법을 변경함으로 써 성능을 향상

   

3. **역정규화 적용**

   - 테이블 역정규화
   - 속성의 역정규화
   - 관계의 역정규화



## 역정규화 수행 



### 1. 테이블 역정규화 수행

​	 <img src="C:\Users\chan\AppData\Roaming\Typora\typora-user-images\image-20201208224608448.png" alt="image-20201208224608448" style="zoom:80%;" />

#### 역정규테이블

- **1차 정규화의 역**
  - 속성의 원자성을 위배하는 방향으로 역정규화
    - 속성의 원자성을 위배한다는 것은 특정 컬럼이 반복되는 값이 될 때
- **2차 정규화의 역**
  - 부분함수 종속성을 가지는 방향으로 역정규화
  - 정규화하기 전과 같음
- **3차 정규화의 역** 
  - 이행함수 종속성을 가지는 방향으로 역정규화
    - 이행함수 종속성: 일반 속성 중 특성 속성이  결정자



#### 테이블 병합

- **1:1 관계 테이블병합** 
  - 1:1관계를 통합하여 성능향상
- **1:M 관계 테이블병합** 
  - 1:M 관계 통합하여 성능향상
- **슈퍼/서브타입 테이블병합**
  - 슈퍼/서브 관계 통합하여 성능향상



#### 테이블 분할

- **수직분할**
  - 컬럼단위의 테이블을 디스크 I/O를 분산처리 하기 위해 테이블 1:1로 분리하여 성능향상
  - **트랜잭션의 처리되는 유형을 파악이 선행**되어야 함
  - 컬럼수가 적은 테이블 에서 데이터 처리하게 되면 디스크 I/O 양이 감소하여 성능이 향상 됨
- **수평분할**
  - 로우 단위로 집중발생되는 트랜잭션을 분석하여 디스크 I/O및 데이터접근의 효율성을 높여 성능향상 하기 위해 로우단위로 테이블을 쪼갬(관계가 없음)



#### 테이블 추가

- 중복테이블 추가
  - 서버B의 ‘연계’ 테이블과 조인이 빈번한 경우
  - 원격 서버 간의 조인이 빈번한 경우
  - 로컬조인 조회 성능 향상
- 집계(통계)테이블 추가 
  - 미리미리 집계
  - BATCH 작업
- 이력테이블 추가
- 부분테이블 추가



### 2. 컬럼 역정규화 수행 

​	  <img src="C:\Users\chan\AppData\Roaming\Typora\typora-user-images\image-20201209030650613.png" alt="image-20201209030650613" style="zoom:80%;" />



### 3. 관계 역정규화 수행 

- 중복관계 추가
  - 데이터를 처리하기 위한 여러 경로를 거쳐 조인이 가능하지만 이때 발생할 수 있는 성능저하를 예방하기 위해 추가적인 관계를 맺는 방법이 관계의 반정규화 임




