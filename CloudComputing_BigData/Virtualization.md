# VIRTUALIZATION TECHNOLOGIES

- 참고자료
  - “클라우드 가상화 기술의 변화 - 컨테이너 기반의 클라우드 가상화와 DevOps”, 안성원, 소프트웨어정책연구소, 2018.12.



# Basic Concepts and Types of Virtualization

### Hypervisor-Based Virtualization

  	<img src="..\img\image-20200910003636198.png" alt="image-20200910003636198" style="zoom:67%;" />

  - 가상머신을 관리하는 별도의 SW
  - **하나의 물리적인 자원에서 구동할 수 있는 운영체제 종류가 다양해짐**
  - Type1
    - **Server에서 주로 사용**
      - Data Center에 들어가는 Server는 하이퍼바이저가 관리 해야하는 하드웨어가 다양하지 않기 때문에
    - HW와 Hypervisor 사이의 OS가 없음
    - **Bare-metal Hypervisor**
      -  HW위에 Hypervisor가 바로 올라감
  - Type2
    - 보통 사용자들이 일반적으로 많이 사용
    - 개인 PC 대상



### Container-Based Virtualization

<img src="..\img\image-20200910004406042.png" alt="image-20200910004406042" style="zoom:67%;" />

  - Guest OS가 사라짐
  - **Host OS의 Kernel 기능을 공유**
  - 하이퍼바이저 기반 가상화보다 가벼움
  - Guest는 Host OS를 따라감
  - 이미지가 작고, 부팅시간이 적음
  - 많은 수의 컨테이너를 운용할 수 있음



## Basic Concepts of Virtualization

#### Virtualization은 물리적인 컴퓨터 자원을 추상화

- **클라우드 컴퓨팅 구현을 위한 핵심기술**

- 가상화의 대상이 되는 컴퓨팅 자원은 CPU, Memory, Storage, Network 등을 포함

  - 서버나 장치들을 가상화함으로써 높은 수준의 자원 사용율과 분산 처리 능력을 제공

- 하나의 장치를 여러 개처럼 동작(**Partitioning**)시키거나, 여러 개의 장치를 묶어 마치 하나의 장치인 것처럼 (**Aggregation**)사용자에게 제공

  ​	<img src="img\3.png" alt="image-20200921011451392" style="zoom: 80%;" />

  - **Partitioning**
    - 일반적인 가상화
  - **Aggregation**
    - 대용량의 자원이 필요한 경우



#### **데이터의 효율적인 자원관리**

- 일반적으로 서버에서 제공되는 서비스는 항상 많은 양의 컴퓨팅 자원을 소모하진 않음
- 어떤 서버는 컴퓨팅 자원이 모자란 반면, 다른 서버는 남아도는 상황
- 가상화를 통해 한 대의 서버 컴퓨터에서 동시에 여러 개의 운영 체제를 가동시키고, 컴퓨팅 자원이 모자란 서버의 요청 태스크를 분산처리 가능
  - 서비스 요청이 몰린 다운 직전의 서버의 부담을 해소하고, 다른 서버의 남아도는 자원을 활용하는 효과를 동시에 얻을 수 있음



**클라우드 컴퓨팅**은 기존의 하드웨어와 이들을 연결하는 네트워크로 구성된 환경을 **가상화를 통해** 통합된 계산, 저장 및 처리가 가능한 환경으로 제공



#### History

- 가상화의 개념은 1960년대 IBM Mainframe에서 시도
- 첫 상용 솔루션은 2001년 발표된 VMWare