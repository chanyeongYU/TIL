# Cloud Computing

- 명지대 컴퓨터공학과 김직수 교수님 *클라우드 컴퓨팅*  강의 내용



## Big Data

- Big Data is a term for data sets that are so large or comlpex that traditionsal data processing application SW are inadequate to deal with them
- **21세기 원유**
  - **잘 정제하면 혁신적이다**
- 데이터를 얻는 능력, 이해하는 능력, 처리하는 능력, **가치를 뽑아내는 능력**, 시각화하는 능력, 전달하는 능력
- **데이터를 잘 정제해서  가치를 뽑아내기**
- B - KB - MB - GB - TB - PB - EB - ZB - YB
- 기존의 DB는 사라지는 것이 아님, 각각의 target이 다름
- 크게 Big Data Platform기술, 데이터 분석 두가지로 나뉨

### Concepts of Big Data

- **데이터의 규모에 초점을 맞춘 정의**
  - 기존 DB 관리 도구의 데이터 수집, 저장, 관리, 분석하는 역량을 넘어서는 데이터 
- **업무 수행 방식에 초점을 맞춘 정의**
  - 다양한 종류의 대규모 데이터로부터 **저렴한 비용으로 가치를 추출**하고, 데이터의 빠른 수집, 발굴, 분석을 지원하도록 고안된 차세대 기술 및 아키텍쳐



### Why Big Data?

- Science, Engineering, Commerce
- 모든 분야에 걸쳐 빅데이터의 중요성이 점점 늘어남
-  **Science**
- 대형 강입자 충돌기 (Large Hadron Collider)에서 연간 생성되는 데이터 1PB
  - **상당한 규모의 데이터가 생성되고 적절하게 다루는 기술이 필요**
- **Engineering**
  - **데이터 분석 -> 장애 발생 징후 패턴 분석 -> 고장 예측 가능**
- **Commerce**
  - **Customer Data -> Insights -> Competitive Advantages**
- **Convergence**
  - Healthcare + Big Data Technology



### Big Data의 3대 요소 (3Vs)

- Volume(크기), Velocity(속도), Variety(다양성)

- Volume이 중요 요소

  ​	<img src="C:\Users\chan\AppData\Roaming\Typora\typora-user-images\image-20200909202758215.png" alt="image-20200909202758215" style="zoom: 67%;" />

  

​	<img src="C:\Users\chan\AppData\Roaming\Typora\typora-user-images\image-20200909203032651.png" alt="image-20200909203032651" style="zoom:50%;" />

- **Volume - 크기**
  - 일반적으로 수십 TB 또는 수십 PB 이상
  - 확장 가능(Scalable)한 방식으로 데이터를 저장하고 분석하는 **분산 컴퓨팅 기법으로 접근 필요**
    - 기존 데이터 분석을 위해 사용하는 데이터 웨어하우스(DW)같은 솔루션이 소화하기 어려울 정도로 데이터의 양이 급격히 증가
    - Scalable - Scale Up, Scale Out
      - Scale Up: 한 컴퓨터의 자원을 증가시키는 것
      - Scale Out: 여러 컴퓨터를 묶어서 사용 (분산 컴퓨팅)

- **Velocity - 속도**
  - 실시간 처리 (Real-Time Processing)
    - 데이터의 생산, 저장, 유통, 수집, 분석의 실시간 처리가 중요
  - 장기적인 접근 (Batch Processing)
    - 수집된 대량의 데이터를 다양한 분석 기법과 표현 기술로 분석(데이터 마이닝, 기계 학습, 자연어 처리)
  - Analysys of Streaming Data

- **Variable  - 다양성**
  - 정형(Structured)
    - **고정된 핑드에 저장**되는 데이터
  - 반정형(Semi-Structured)
    - 고정된 필드로 저장되어 있지는 않지만 XML, JSON등 과  같이 **메타데이터나 스키마 등을 포함**하는 데이터
  - 비정형(Unstructured)
    - 고정된 필드에 저장되어 있지 않은 데이터
    - ex) 동영상, 사진, 메신저 대화 내용 등..
  - **데이터의 다양성에 따라서 다양한 형태의 데이터 저장소 기술을 사용 (Hadoop, MongoDB 등 ..)**



### Importance of Big Data

- **빅데이터 분석은 그 데이터를 사용하는 조직이 효율적으로 사용하고 새로운 기회를 발견하도록 도움을 줌**

 <img src="C:\Users\chan\AppData\Roaming\Typora\typora-user-images\image-20200909205439502.png" alt="image-20200909205439502" style="zoom:67%;" />

- **비용 절감**
  - **하둡**과 같은 빅데이터 기술, 클라우드 기반 분석 기술이 결합시 상당한 비용 절감을 얻을 수 있음
- 빠르고 향상된 의사 결정
  - **하둡**과 In-Memory Analytics(스파크) 그리고 새로운 데이터 소스의 결합은 즉각적이고 향상된 의사결정을 할 수 있도록 도와줌
- **새로운 제품과 서비스** 
  - 고객의 요구 사항과 만족도를 분석하면 그들이 원하는 서비스를 만들 수 있다.