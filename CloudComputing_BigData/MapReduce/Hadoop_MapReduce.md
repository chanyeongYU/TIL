# Hadoop MapReduce

 <img src="..\..\img\image-20201201003103281.png" alt="image-20201201003103281" style="zoom:80%;" />

- DataNode Daemon과 TaskTracker가 각각의 SlaveNode마다 존재함
  -  **TaskTracker가 없다면,** JopTracker가 TaskTracker가 없는 노드에게는 Map이나 Reduce 작업을 할당해줄 수 없음
    - TaskTracker는 JopTracker에게 명령을 받아 작업을 실행해야하기 때문에
    - SlaveNode가 없는거나 마찬가지
  - **DataNode Daemon이 없다면,** NameNode의 관점에서 없는거나 마찬가지이기 때문에 데이터를 저장할 수 없음
    - TaskTracker가 해당 SlaveNode의 로컬 파일을 사용할 수 없음



#### Google MapReduce와 차이점

- Master → “**JobTracker**” 
- Worker → “**TaskTracker**”



#### JobTracker

- 클러스트 내 특정 노드들에게 MapReduce작업을 관리해줌

  - **데이터가를 가지고 있거나 동일한 랙에 있는 노드들에게 작업 할당**
  - a (Single) Point of Failure for Hadoop MapReduce service
    - JobTracker가 죽으면, 실행중인 모든 MapReduce Job은 모두 정지

- **MapReduce Job Processing의 과정**

   <img src="..\..\img\image-20201201005536618.png" alt="image-20201201005536618" style="zoom:80%;" />

  - **HDFS같은 Shared FileSystem에 Job Resource(jar파일)를 복사하는 이유**

    - MapReduce Job을 제출하면 JobTracker가 알아서 실행하기 때문에 어느 노드에서 실행되는지 알 수없음

      **:point_right: 어디에서나 접근 가능한 파일 시스템에 저장**

  - **JobTracker가 Retrieve InputSplit하는 이유**

    - 각각의 InputSplit이 어느 TaskTracker에 할당되어 있는지 찾아내기 위해 

  - TaskTracker Child JVM을 통해서 사용자가 정의한 Map 또는 Reduce함수를 적용

  

  1. Client Applications이 JobTracker에게 Job을 제출(Submit)
  2. JobTracker는 NameNode를 통해 데이터의 위치를 결정
  3. JobTracker는 **사용 가능한 Slot이 있고 데이터와 가까운** TaskTracker 노드를 찾음
     - Slot: 노드가 몇개의 작업을 돌릴 수 있는지 설정이 되어있음
  4. JobTracker는 선택한 TaskTracker 노드에 작업을 건네줌
  5. TaskTracker는 모니터링됨
     - **Heartbeat** 신호를 충분히 제출하지 않으면 실패한 것으로 간주되며 다른 TaskTracker에서 작업이 스케줄링
  6. TaskTracker가 작업에 실패하면 JobTracker에게 알려주고, JobTracker는 다른 노드에 작업을 건네주거나 해당 TaskTracker를 블랙리스트에 올림
  7. TaskTracker의 작업이 완료되면,  JobTracker의 상태를 갱신
  8. Client Applications은 JobTracker에게 정보를 얻을 수 있음



#### TaskTracker

- JobTracker로부터 Task(Map, Reduce and Shuffle)를 받아서 실행
- 각각의 TaskTracker는 Slot들의 집합으로 설정되어있음(Static)
  - 받아 들일 수 있는 Task의 갯수가 사전에 정의되어 있음
  - 클러스터를 구성하는 노드의 사양이 균등하다고 가정하기 때문에 각 노드들의 사양의 달라지면 자원 관리가 비효율적임
- 별도의 JVM 프로세스를 생성하여 실제 작업 수행
  - Task가 실패하더라도 TaskTracker에게 영향을 끼치지않게 하기 위해
  - 생성된 프로세스 모니터링하고 , 출력 및 종료 코드 캡처
  - **프로세스가 완료되면 TaskTracker가 JobTracker에 통지(Heartbeat)**
  - JobTracker에 사용 가능한 슬롯의 수를 통지



**Task를 실행할때 고려해야 할 점**

- 사용가능한 Slot
- Data Locality





