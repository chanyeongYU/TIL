## SCALING OUT



### Data Flow of MapReduce

- MapReduce Job은 사용자가 수행하기 원하는 작업단위

  - InputData, MapReduce 프로그램 및 설정 정보로 구성됨

- Map Task와 Reduce Task로 나누어 실행

  - **Hadoop1.0에서는 Jobtracker와 여러개의 Tasktracker에 의해 실행**
  - Hadoop2.0에서는 YARN에 의해 실행

- :fire:**고정된 크기**의 Input Split으로 나눔

- Map Task하나당 각각의 Split 담당

- :fire: **Data locality optimization**

   <img src="C:\Users\chan\AppData\Roaming\Typora\typora-user-images\image-20201202030129609.png" alt="image-20201202030129609" style="zoom:80%;" />

  

- **:fire: 최적의 분할 크기가 블록 크기와 동일해야 하는 이유**

  - Input Split Size: M/R Job이 데이터를 처리하는 단위, 하나의 Mapper를 얼만큼의 데이터별로 띄울것인가? (Processing)
  - HDFS Block Size: 빅테이터를 청크로 어떻게 저장, 관리할 것인가? (Storering)
  - 서로 독립적인 개념
  - 단일 노드에 저장할 수 있는 최대 입력 크기
  - **하나의 Input Split이 두개의 블럭을 사용한다면** 
    - **두 개의 블럭은 서로 다른 데이터처럼 됨(독립적으로 저장, 복제)**
    - **노드가 하나의 Input Split이 필요로하는 두개의 HDFS블럭을 가지고 있을 경우가 별로 없음**
    - **둘중 하나는 네트워크를 통해 전송되어야함**
  - **Input Split이 작아지면 관리해야할 Map Task가 많아지면서 오버헤드가 발생**

- 매핑 작업에서 출력을 HDFS가 아닌 로컬 디스크에 기록

  - 매핑작업 출력은 중간 결과물이기때문에 HDFS에 저장하는 것은 낭비
  - 매핑작업이 실행중에 죽으면 하둡은 자동적으로 다른 노드에서 재실행
    - 로컬태스크의 작업이 날라갔기 때문에 중간 결과물을 다시 만들어야함

- 리듀스 작업은 데이터인접성의 이접이 없음

  - 모든 매핑작업의 출력 결과가 몰리기때문에 네트워크를 통한 전송이 필수적이기 때문에

- 리듀스와 매퍼의 차이점

  - 매퍼의 갯수는 자동적으로 input split에 따라 만들어지지만 리듀서는 사용자 정의가능 (default = 1)



#### Multiple Reducers

 <img src="C:\Users\chan\AppData\Roaming\Typora\typora-user-images\image-20201202220657682.png" alt="image-20201202220657682" style="zoom:80%;" />

- 매핑 작업의 출력(중간 결과물)이 분할
  - 리듀서의 갯수만큼 중간결과물이 남겨짐
- 특정한 키에 해당하는 데이터는 하나의 파티션(리듀서)로 전달
  - 첫번째 파티션은 첫번째 리듀스에게 구번째는 두번째에게
  - :fire:**같은 Intermediate Key를 공유하는 모든 값들은 특정한 파티션에 몰아서 저장**
    - **정확한 aggregation을 하도록 보장**



#### zero reduce tasks

 <img src="C:\Users\chan\AppData\Roaming\Typora\typora-user-images\image-20201202220648117.png" alt="image-20201202220648117" style="zoom:80%;" />

- 매핑 작업만 수행
- 매핑작업의 결과물이 HDFS에서 쓰일 때
- 데이터 전처리 같은 경우



#### Combiner Functions

- 매핑작업의 결과물이 소수의 리듀서에게 전송되는 양을 줄이기 위해
  - 매핑작업의 결과물에 리듀스와 같은 작업을 적용함
- The combiner function is an optimization
  - 생략 가능
  - Map → (**Combiner**) → Reducer
- 매핑작업이 이뤄지는 노드에서 같이 실행
- 항상 사용할 수 있는것이 아님
  - **사용하기 위해서는 리듀스 작업에 대해 교환법칙과 결합 법칙이 성립해야함**
    - 예를 들어 평균 계산 같은 경우는 사용 불가



#### 참고사항

##### Hadoop Streaming

- 자바외의 언어를 사용하여 맵리듀스 작업을 할  수 있도록 해주는 API 제공
-  Unix standard stream을 사용 할 수 있어야 가능