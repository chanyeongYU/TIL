# BASIC CONCEPTS OF MAPREDUCE

### Hadoop Project

- **MapReduce** 패러다임을 사용하는 대용량 데이터의 분석, 변환을 위한 **프레임워크**와 **분산파일시스템**을 제공
  - 데이터만 분할할 뿐 아니라 computation까지 분산 저장, 처리
  - **병렬적으로 데이터와 최대한 가깝게 처리**
- **일반적인 서버를 추가하는 것만으로도 컴퓨팅 용량, 스토리지 용량 및 IO 대역폭을 확장**



### MapReduce Paper

```
Abstract

MapReduce is a programming model and an associated implementation for processing and generating large data sets.
Users specify a map function that processes a key/value pair to generate a set of intermediate key/value pairs, and a reduce function that merges all intermediate values associated with the same intermediate key.
Many real world tasks are expressible in this model, as shown in the paper.
...
```



### Introduction

- MapReduce 이전
  -  구글은 수백개의 특수용 컴퓨터를 개발
    - 대용량의 Raw Data의 처리를 위해
  - **대용량의 Raw Data의 처리는 개념상으로는 단순하지만, 적절한 시간내에 연산을 마치기 위해서는 Input Data가 매우 커야하고  Computation이 여러대의 컴퓨터에 분산되어야함**
    - 분산 데이터로 인한 오버헤드가 너무 큼
    - Failure 문제
- **MapReduce는 프로그래밍 모델인 동시에 대규모의 데이터셋을 처리하고 생성을 위한 구현 결과물**
  - 새로운 단순한 연산을 표현하기 위한 **추상화**
  - 병렬화, 내결함성, 데이터 배포 및 로드 밸런싱의 복잡한 세부사항을 **숨김**
    - 분산 처리에 대한 문제는 MapReduce에게 맡기고, 사용자가 해결하고자 하는 문제만 MapReduce 프로그래밍 모델에 맞게 개발
  - Lisp 및 여러개의 함수형 언어에서 **Map과 Reduce라는 개념을 가져옴**
    - 각각의 **Logical Record**에 대해 Map 기능을 적용하여 **Intermediate Key/Value** 쌍을 생성
      - Logical Record: RDB의 테이블 row같은 데이터가 아니라 텍스트 줄 하나같은 비정형데이터
    - 같은 키를 공유하는 모든 값들에 대해 Reduce 기능을 적용하여 결합 -> Shuffling
      - **같은 키를 공유하는 모든 값 한꺼번에 묶여서 하나의 Reducer에게 전달**



### Programming Model

- Input “Key/Value” Pairs를 받아서 Output “Key/Value” Pairs를 생성

  - **Map**: 사용자가 작성, Input Pairs를 박아서 Intermediate Key/Value Pairs 생성
    - Input Split
    - 하나의 HDFS 블럭에 대해 하나의 Mapper가 적용
  - **같은 Intermediate Key를 공유하는  값들을 묶어주고 Reduce 함수에게 전달**
  - **Reduce**: 사용자가 작성, Intermediate Key와 값들의 집합을 받아 Merge

- **WordCount 예제**

   <img src="..\..\img\image-20201112135108462.png" alt="image-20201112135108462" style="zoom:80%;" />

  - **Input 단계**에서 단어를 세기위한 데이터를 입력
  - **Splitting 단계**에서 입력데이터를 하둡 분산 파일 시스템에서 정한 64MB 블록 사이즈로 나눠서 저장후 블록 단위로 처리
    - 예제에서는 Input Data의 한줄이 처리되는 하나의 블록이라고 가정(**Input Split**)
    - **Input Split과 Block의 사이즈는 같을 필요는 없음**
  - **Mapping 단계**에서 Mapper는 자기가 담당하는 Input Split을 읽어서 사용자가 정의한 Map함수를 적용
    - 각 단어를 Key로 정하고(**Intermediate key**), Value는 Input Split에 포함된 각 단어의 수가 들어감
    - Map함수 적용 결과로 Intermediate Key-Value의 집합이 생성
      - 뒤 늦게 Map 함수가 적용되는 것이 있으면 전체적인 결과에 혼란이 생길 수 있기 때문에 **모든 Mapper들이 Intermediate Key/Value를 생산하기 전까지는 Reduce단계로 진행하지 않음**
  - **Shuffling 단계**에서 Intermediate Key를 공유하는 Value들은 하나로 묶은 후, 이 Value들은 리스트 형태로 넘겨줌
  - **Reducing 단계**에서 리스트 형태로 받아온 Value들을 Aggregation 연산을 수행
    - 같은 Key를 갖는 Value들은 항상 같은 Reducer가 담당
  - 같은 Key를 갖는 Value들이 넘어오면 모두 합해 **Final Result가 생성**

- MapReducer의 장점
  - Input Data가 매우 크더라도 별도의 코드 수정없이 연산 수행 가능
    - Mapping 단계에서 Input Split 별로 동시다발적이면서 독립적(서로 통신할 필요가 없음)으로 실행되기 때문에
- **Map 함수와 Reduce 함수를 어떻게 구현하고, Intermediate key/Value를 무엇으로 할 건지가 중요**



### Implementation 

- Execution Overview
  - Input Data는 N개의 Input Split으로 Partitioning
  - N개의 Map 클래스가 호출 (Input Data의 크기와 비례)
  - 다른 머신들에서 병력적으로 처리
  - Reduce는 Intermediate key 공간을 분할
    - 같은 Intermediate key를 공유하는 값들이 흩어지지 않게 하기 위해

   <img src="..\..\img\image-20201122223606249.png" alt="image-20201122223606249" style="zoom:80%;" />





