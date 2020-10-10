# Server Virtualization

- Hypervisor-based Virtualization
- **Container-based Virtualization**



## **Container-based Virtualization**

#### Basic Concepts of Container

- Container는 모듈화되고 격리된 컴퓨팅 공간 또는 컴퓨팅 환경

- 기존 Hypervisor와 Guest OS를 필요로 했던 가상머신 방식과는 달리,  

  **프로세스를 격리하여 모듈화된 프로그램 패키지로써 수행**

  - 기존의 가상머신에 비해 가볍고 빠르게 동작할 수 있음

    ​	<img src="..\..\img\image-20201010232604870.png" alt="image-20201010232604870" style="zoom:80%;" />

- 운영체제 위에서 프로세스처럼 실행

  - 가상 하드웨어의 설정을 바꿀 수 있음

- 리눅스에 내장된 LXC(LinuX Container) 기술 이 소개되면서부터 Container라는 개념이 처음 등장

  - LXC는 단일 머신상에 여러개의 독립된 리눅스 커널 컨테이터를 실행하기 위한 OS 레벨의 가상화 기법
  
- 컨테이너 기술은 하이퍼바이저 기반 가상화와 비교해서 경량화로 인한 속도와 이식성 측면에서 각광

  - Guest운영체제를 제외하고 어플리케이션 실행에 필요 한 모든 파일을 패키징 
    - **OS-level Virtualization**
  - 시스템에 대한 요구사항이 적음
    - Image의 크기가 작고, 이로 인해 복제와 배포가 용이
    - **OS 부팅이 필요 없기 때문에 시간 또한 상대적으로 짧음**
  - 시스템의 성능 부하가 훨씬 적음
    - Container 구동 시 운영체제 위에서 하나의 어플리케이션이 동작하는 것과 동일한 수준의 컴퓨팅 자원을 필요
  - 유연한 자원 배분 기능 지원
    - **실행중인 Container에 자원 할당 조정 가능:star:**
  - 더 많은 응용프로그램을 하나의 물리적 서버에서 구동 가능
  
- **Container는 동일한 운영체제 커널을 공유**

  - 보안이나 안정성 측면 에서 문제가 될 수 있음



#### Docker and Cloud Computing

- Docker는 오픈소스 기반의 Container 관리 플랫폼

- 리눅스 컨테이너(LXC) 기술 기반

  ​	<img src="..\..\img\image-20201011000849027.png" alt="image-20201011000849027" style="zoom:80%;" />

- Container Image 생성 기능 제공 (Dockerfile)

  - 특정 Container에서 실행될 소프트웨어와 구동사양을 미리 정의 가능
  - **Docker Image는 수정이 불가한 형태로 배포**
    - Container의 상태가 바뀌거나 삭제되더라도 Image는 불변
    - **Union File System** (images are comprised of multiple layers)

- 같은 Image에서 다수의 Container들을 생성 가능

- **Docker Architecture**

  ​	<img src="..\..\img\image-20201011001949528.png" alt="image-20201011001949528" style="zoom:80%;" />

- **Containers as a Service (CaaS)**



#### Container-based Cloud Operating System

- 다수의 Container(서비스)의 실행 관리 및 조율

  - **Orchestration**
  - 기능

   <img src="..\..\img\image-20201011005631426.png" alt="image-20201011005631426" style="zoom:80%;" />

  - 플랫폼 비교

  <img src="..\..\img\image-20201011005940429.png" alt="image-20201011005940429" style="zoom:80%;" />
  
  - **Kubernetes**



#### DevOps

- SW 개발자와 운영자 간의 소통과 협업을 강조하는 개발 환경 
  - **Container 기반 클라우드의 초석**



####  Container 기술은 클라우드컴퓨팅과 동반 성장

- 한계점도 분명하여 이를 해결하고자 하는 기술개발도 심화
  - 가상머신의 장점인 **고수준의 프로세스 분리 기능 결여**
    - 어떤 컨테이너가 리눅스에 부담되는 행동을 하면 모든 컨테이너에 영향
  - Container는 동일한 운영체제 커널을 공유
    - 보안이나 안정성 측면 에서 문제가 될 수 있음
- **성능은 Native 시스템과 가깝지만 Isolation이 떨어짐**
- 서버 가상화 기술과 상호 보완하면서 공존할 것으로 전망