# MapReduce Algorithm Design

- Some slides and examples are provided by Prof. Jimmy Lin (University of Waterloo)
- Chapter 5: Hadoop I/O (Hadoop The Definitive Guide 4 th Edition)
- Chapter 8: MapReduce Types and Formats (Hadoop The Definitive Guide 4th Edition)



#### Basic Cluster Components(ver1)

- On the master
  - Namenode: master node for HDFS
  - Jobtracker: master node for job submission
- On each of the slave machines
  - Datanode: serves HDFS data blocks
  - Tasktracker: contains multiple task slots
- YARN과는 다름



#### Overall Flow of MapReduce Execution

 <img src="..\..\img\image-20201202232201532.png" alt="image-20201202232201532" style="zoom:80%;" />

#### 

#### Probably the most complex aspect of MapReduce execution(Shuffle and Sort)

 <img src="..\..\img\image-20201203004835982.png" alt="image-20201203004835982" style="zoom:80%;" />

- **Map side**
  - 매핑 작업의 실행 결과 원형 버퍼에 저장
  - 버퍼가 임계값에 도달하면 해당 노드의 로컬디스크에 저장
  - 저장된 데이터를 병합해서 파티션 파일 생성
  - 병합할 때 Combiner 실행



- **Reduce side**
  - 매핑 작업의 출력을 리듀서 머신에 저장
  - 여러개의 중간 결과물을 병합하여 하나의 파일 생성
  - 최종 병합된 파일이 리듀서 통과



#### MapReduce API

- `Mapper<Kin, Vin, Kout, Vout>`
  - `void setup(Mapper.Context context)`
    - 작업이 시작할  때 한번 호출
    - 매퍼 하나가 담당하는 Input Split이 작업 하나
  - `void map(Kin key, Vin value, Mapper.Context context)`
    - **Input K/V 한 쌍당 한번 호출**:fire:
  - `void cleanup(Mapper.Context context)`
    - 작업이 끝날 때 한번 호출



- `Reducer<Kin, Vin, Kout, Vout> / Combiner<Kin, Vin, Kout, Vout>`
  - `void setup(Reducer.Context context)`
    - 작업이 시작할  때 한번 호출
  - `void reduce(Kin key, Iterable values, Reducer.Context context)`
    - **각 Intermediate Key 별로 한번씩 호출:fire:**
  - `void cleanup(Reducer.Context context)`
    - 작업이 끝날 때 한번 호출



- `Partitioner<K, V>`
  - 필수로 구현해야하는 것은 아님
  - `int getPartition(K key, V value, int numPartitions)`
    - K/V가 들어왔을 때  몇번 파티션인지 알려줌



- **Job**
  - input과 output 경로 설정
  - input과 output 형식 지정
  - 매퍼, 리듀서, (Compiner), (Partitioner) 클래스 정의
  - 중간 결과물(K/V)과 최종 결과물(K/V) 클래스 정의
  - 리듀서 갯수 정의
    - **:star:매퍼는 갯수를 정하지 않는 이유?**
      - **사용자가 정의하는 것이 아니라 전체 입력데이터의 크기를 Input Split의 크기로 나눈 갯수만큼 Mapper가 실행되기 때문에**
      - Input Split의 크기는 사용자 정의 가능
      - 노드당 10~100개가 적절



#### Data Types in Hadoop

- :fire:**Writable**
  - **serialization/deserialization** 프로토콜 정의
    - **serialization**: 구조화된 객체를 byte stream으로 바꾸는 것
    - **deserialization**은 반대로
- :fire:**WritableComprable**
  - 정렬, 비교를 할 수 있도록 해줌
  - 모든 Key는 이 데이터 타입을 사용해야함
    - Key는 정렬의 기준이기 때문에
- IntWritable, LongWritable, Text ...
  - 다양한 데이터 타입에 대한 구체적인 클래스



#### Serialization

- **serialization**: 구조화된 객체를 byte stream으로 바꾸는 것
  - **네트워크로 전송**을 하거나 **하드디스크에 저장**하기 위해
- **deserialization**은 반대로
  - 네트워크로부터 받은 byte stream을 구조화된 객체로 구성하기 위해
- 둘다 CPU가 하는 일



- **:star:RPC serialization 포맷의 특징**

  - **Compact**
    - 네트워크 대역폭을 적게 사용하도록 간결해야 함
  - **Fast**
    - serialization/deserialization과정에 있어 성능 상의 오버헤드가 적도록 속도가 빨라야함
  - **Extensible**
    - 확장 가능하도록 구성해야 새로운 요구사항이 반영될 때 프로토콜이 바뀌면 명확해야함
  - **Interoperable**
    - 서로 다른 개발언어에서도 지원하도록 여러가지 클라이언트 인터페이스 지원

  

  - **스토리지 입장**에서도 네가지 특징이 중요
    - **Compact**
      - 효율적으로 스토리지 공간을 활용
    - **Fast**
      - 대용량의 데이터를 읽거나 쓸때 오버헤드가 적도록 속도가 빨라야함
    - **Extensible**
      - 오래된 포맷의 데이터를 읽을 수 있도록 확장 가능해야 함
    - **Interoperable**
      - 저장된 데이터를 서로 다른 개발언어에서도 읽거나 쓸 수 있어야 함



- **하둡은 자체적으로 serialization 형식인 Writable 타입 제공**
  - **:star:자바의 Serialization 객체를 안쓰고 별도의 Format을 만든 이유** 
    - 자바의 Serialization 객체는 무겁다고 느낌
    - Hadoop의 핵심인 데이터가 읽어지고 쓰여지는 것을 정확한 컨트롤을 하고싶음
    - Hadoop에 있어 효과적인 고성능 커뮤니케이션이 필수



#### Input and Output

- InputFormat
  - TextInputFormat
    - 맵리듀스의 기본 입력 형식
    - 입력 파일의 한줄을 별도의 레코드
    - No Parsing
    - 특별한 형식이 없는 데이터 또는 로그파일
    - Key: 해당 라인이 파일의 시작부터 얼마나 떨어져있는지(offset)
    - Value: 라읜의 내용 자체
  - KeyValueTextInputFormat
    - Key와 Value가 탭으로 구분되어 텍스트 형태로 정의되어 있을 때
  - SequenceFileInputFormat



- OutputFormat
  - TextOutputFormat
  - SequenceFileOutputFormat



#### Job

- Hadoop MapReduce program = Hadoop job
- 맵과 리듀스 태스크로 나눠짐
- task attempt: 실행중인  태스크
- **각각의 태스크 tasktracker의 슬롯 하나를 차지**
- 여러개의 job이 워크플로우 형성



- Job submission
  - 클라이언트가 job을 생성하고 설정
  - jobtracker에게 제출
  - 다음은 Hadoop이 알아서
    - input Split 계산
    - Job data를 jobtracker에게 전송
    - jobtracker는 Job data를 **공유 위치에 저장**후 대기
      - 구현된 클래스의 정보를 담고있는 jar파일이 모든 구성 노드에서 사용 가능하도록 하기 위해
    - Tasktrackers가 Task 폴링



#### Hadoop Job Execution

 <img src="..\..\img\image-20201203023818887.png" alt="image-20201203023818887" style="zoom:80%;" />



 <img src="..\..\img\image-20201203024205029.png" alt="image-20201203024205029"/>



#### Getting Data to Mappers and Reducers

- Configuration parameters
  - Passing parameters
- :star:**DistributedCache**
  - 모든 노드에서 공통으로 참조해야 할 파일이 있을 경우
  - 모든 노드에 Local Copy



#### Context Object

- Mapper가 Hadoop 시스템과 상호 작용하도록 허용
- **용도**
  - 진행 상황 보고
  - 응용프로그램 상태 메시지 설정
  - counter 업데이트
  - 살아 있음을 나타냄
  - job configuration에 저장된 값들을 가져오기 위해 



#### Hadoop Runtime System

- **이점**

  - 스케쥴링 
    - 워커들에게 맵과 리듀스 작업 할당
  - 데이터 배포
    - 데이터와 가까운 쪽으로 프로세스를 이동
  - 동기화 처리
    - 리듀스로 넘어가는 단계
    - gathers, sorts, and shuffles intermediate data
  - 에러 및 결함 처리
    - 워커의 에러 및 결함 탐지

  

- **딘점**

  - 개발자는 데이터 및 실행에 대해 제한적인 제어만 가능
    - map, reduce, combine, partitioner 4개의 함수로만 표현
  - 알기 힘든 것
    - 매퍼와 리듀서가 작동하는 곳
    - 매퍼와 리듀서가 시작할 때나 끝날 때
    - 특정 매퍼가 처리중인 입력
    - 특정 리듀서가 처리중인 Intermediate Key



#### Local Aggregation

- 이상적인 스케일링의 특징

  - 데이터 2배이면, 실행 시간이 2배이므로
  - 리소스 2배, 실행 시간 절반!

- 위에 특징을 구현하기 힘든 이유

  - 동기화에 통신이 필요
  - **통신이 성능을 낮춤**

  :point_right: ​통신을 줄이기 위해 **Local Aggregation을 통해 중간 데이터 감소**:fire:

  ​		- 개발자가 직접적으로 관여

  :point_right: Combiner 사용

  		- 하둡이 알아서 실행



 <img src="..\..\img\image-20201203035513993.png" alt="image-20201203035513993" style="zoom:80%;" />



- BaseLine vs Histogram vs Preserving State
  - BaseLine 
    - 일반적인 매퍼와 리듀서
  - Histogram 
    - 한 레코드에서 중복된 단어를 미리 합침
  - **Preserving State**
    - 한 Input Split에서 합치는 수준
    - 단점
      - 명시적인 메모리 관리가 요구됨
      -  순서에 의존적인 문제가 발생할 수 있음



- **In-mapper combining(Local Aggregation) vs. Combiner**
  - In-mapper combining
    - Local Aggregation은 디자인 패턴
    - 개발자가 정확히 제어가능
  - Combiner
    - Combiner가 언제 불릴지 불분명함
  - 일반적으로 In-mapper combining가 더 효율적임
  - Combiner는 중간 데이터의 양을 줄여주지만 K/V 쌍의 수를 줄여주지는 않음
  - In-mapper combining은  K/V 쌍의 수를 줄여줌
    - 대신 메모리에서 추가적인 데이터구조의 관리 필요



