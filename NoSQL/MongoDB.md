### MongoDB 란?

- MongoDB는 Document DB

  - JSON 기반의 Document 기반 데이터 관리

  - MongoDB Document 예

    ```json
    {
        "_id": ObjectId("5099803df3f42312312391"),
        "username": "gildong",
        "name": { first: "길동", last: "홍" }
    }
    ```



#### MongoDB의 특징

- Document-Oriented Storage : 모든 데이터가 JSON 형태로 저장되며 schema가 없음
- Full Index Support : RDBMS에 뒤지지 않는 다양한 인덱싱을 제공
- Replication & High Availability : 데이터 복제를 통해 가용성을 향상시킬 수 있음
- Auto-Sharding : Primary key를 기반으로 여러 서버에 데이터를 나누는 scale-out이 가능
- Querying : key 기반의 get, put 뿐만이 아니라 다양한 종류의 쿼리들을 제공
- Fast In-Place Updates : 고성능의 atomic operation을 지원
- Map/Reduce : 맵리듀스를 지원
- GridFS : 별도 스토리지 엔진을 통해 파일을 저장할 수 있음.



#### 프로그래밍에서 다루는 데이터 포맷

- 정수(int), 소숫점(float), 문자(string)
- CSV, JSON등
  - JSON document = { "id":"01", "languange":"java", "edition": { "first": "1st", "second":"2nd", "third":"third" } }



#### MongoDB 데이터 구조

- Database - Collection(Table과 유사) - Document(Row 와 Column이라는 개념이 없다.)

- 참고) RDBMS

  - RDBMS: Database - Table - Row와 Column
    - RDBMS의 table이 아니라, collection 에 JSON 형태의 Document를 넣음
    - Document 하나가 하나의 로우(레코드)임

  <img src="..\img\img05.png" style="zoom:80%;" />



#### MongoDB Database

- Database는 Collection의 집합



#### MongoDB Collection

- Collection은 MongoDB Document의 집합
- RDBMS Table과 유사한 개념, 단 정규화된 데이터 구조, 즉 Schema가 정의되어 있지 않음

