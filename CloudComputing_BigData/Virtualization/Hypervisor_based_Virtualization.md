# Server Virtualization

- **Hypervisor-based Virtualization**
- Container-based Virtualization



## Hypervisor-based Virtualization

#### Virtual Machine(VM)

- **가상화를 통하여 구현되는 복제된 컴퓨팅 환경**
- 실제 가상화하기 전과 같은 수준의 실행 기능을 제공하는 것을 목적
- 가상머신들 간의 물리적인 하드웨어를 공유
- 여러 개의 가상머신이 하나의 물리 서버에 동시에 존재할 수 있음
  - 각 **가상머신마다 서로 다른 구동 환경을 갖출 수 있어서 다양한 어플리케이션을 실행하는 것이 가능**
- **Consolidation(통합)**이 가능해진다
- 가상머신의 관점에서는 접근하는 하드웨어 디바이스가 가상이라는 것을 알지 못함
  - Paravirtualization(반가상화)의 경우에는 가상머신에 탑재되어 있는 Guest OS가 하드웨어가 가상화되었다는 것을 인지
    - 성능 측면의 이유
- **분할된 시스템 간 상호 간섭이 없음**
  - **Isolation을 보장**
  - Grid Computing의 단점 보완



#### Hypervisor

- H/W를 가상화하기 위해서는 이들을 관장할 뿐만 아니라 각각의 가상머신들을 관리할 **VMM(Virtual Machine Monitor)**와 같은 중간관리자

- VM과 H/W간의 I/O 명령을 처리

  - Type1 Hypervisor에서 더욱 두드러지는 특징
  - Type2 Hypervisor는 Host OS위에 Hypervisor가 올라가기 떄문애 일반적인 어플리케이션과 비슷하게 작동

- Hypervisor에 요구되는 세 가지 요소

  - **정확성(Fidelity)**: VM을 위해 만든 환경은 원래의 물리 머신과 본질적으로 동일해야 함
  - **독립성(Isolation)과 안정성(Safety)**: Hypervisor는 시스템 자원에 대한 완전한 제어권을 가짐
  - **성능(Performance): VM과 물리적 환경 간의 성능 차이가 없어야 함**
    - 사실상 불가능, 성능 차이가 있을 수 밖에 없음
      - **Hypervisor Layer가 추가되고 여러 개의 가상머신이 컴퓨팅 자원을 공유하다 보면 Overhead가 생기기 때문에**
    - Container 기반 가상화를 사용해야 성능 차이가 얼마 없음

- 위치 및 역할 차이에 따라 Type1과 Type2로 구분

  ​	<img src="..\..\img\image-20201007222805740.png" alt="image-20201007222805740" style="zoom:80%;" />

  - **Type1 Hypervisor**
    - Bare-metal 기반으로 **하드웨어 위에서 Hypervisor가 바로 구동**
    - 가상머신에 설치된 **Guest OS가 H/W 위에서 2번째 수준으로 구동**
    - **Type2 보다는 향상된 성능을 제공하지만, 여러 H/W 드라이버를 세팅해줘야 하며 설치가 어려움**
    - 서버에서 많이 사용
  - **Type2 Hypervisor**
    - 하드웨어 위에 Host OS가 있고, 그 위에서 다른 응용 프로그램과 유사한 형태로 동작
    - 가상머신의 **Guest OS는 H/W 위에서 3번째 수준으로 구동**
    - 기존 컴퓨터 환경에서 활용하는 것이기에 설치가 용이하고 구성이 편리한 장점이 있음
    - Type1 보다는 성능이 떨어질 수 있음



#### Full Virtualization(전가상화) vs. Paravirtualization(반가상화)

​	<img src="C:\Users\chan\AppData\Roaming\Typora\typora-user-images\image-20201007223215584.png" alt="image-20201007223215584" style="zoom:80%;" />

- 가상화 하는 방식에 따라서 전가상화와 반가상화로 구분
  - I/O 접근을 어디까지 가상화 할 것인가?
- **성능 측면(I/O에 대한 병목현상)의 이유로 반가상화 기술 출현**
  - 컴퓨팅 성능의 순서
    - Native > 반가상화 > 전가상화> 에뮬레이션



##### Full Virtualization

- 컴퓨팅 시스템의 하드웨어 리소스를 완전하게 가상화 하는 방식
- **Guest OS의 수정 없이 구동 가능**
- **CPU의 가상화 지원 기술(VT, Virtualization Technology)**과 같은 하드웨어 기능을 일부 지원받을 수 있음
- Guest OS는 기존의 OS를 통해서 하드웨어에 접근(Type-2)
  - Guest OS에서 발생한 하드웨어 접근 및 제어 요구는 CPU의 VT가 Hypervisor에게 HW 접근을 요청
  - 처리 단계가 늘어남에 따라 Hypervisor의 부담이 가중되고 따라서 성능은 반가상화 기법보다 낮음



##### Paravirtualization

- 하드웨어를 완전히 가상화하지 않는 방식

- 대표적인 제품은 **Xen**

- I/O 처리에 있어서 전가상화보다 직접적인 루틴을 사용

- **Guest OS에 대한 수정 필요**

  - **OS를 수정 하기 위해서는 오픈 소스 OS이어야 함(Linux)**

- 가상머신들은 스스로가 가상화 되었다는 것을 인지하여야 함(전가상화와 다름)

  - I/O접근과 같은 특별한 Call(**Hypercall**)을 위해
  - **Hypercall**
    - Hypervisor를 호출하는 명령어로 Guest OS가 직접 서비스에 접근할 수 있는 반가상화 인터페이스

- **Xen Project Architecture**

  ​	<img src="..\..\img\image-20201007225734982.png" alt="image-20201007225734982" style="zoom:80%;" />

  - Xen Project Hypervisor
    - 오픈소스인 Type1 Hypervisor
    - 하드웨어위에서 바로 구동
    - CPU, memory, interrupts등을 담당
    - **I/O함수에 대해 알 필요가 없음**
      - **I/O에 대해서는 Domain0가 담당**
  - Guest Domains/Virtual Machines
    - 가상화된 환경에서 각자의 OS및 어플리케이션을 구동
    - **하드웨어로부터 독립적(Isolation)**
      - **하드웨어나 I/O에 대한 직접적인 접근 권한이 없음**
    - called unprivileged domain (or DomU)
  - **Control Domain (or Domain 0)**
    - **Xen에서 항상 실행되는 특별한 가상머신**
      - **I/O에 대한 모든 접근을 다루는 특별한 권한을 가짐**
  
  
  
  <img src="..\..\img\image-20201008000216013.png" alt="image-20201008000216013" style="zoom:80%;" />
  



#### 가상화의 성능 문제

- 가상화는 하드웨어를 하이퍼바이저를 통해 직접 가상화하거니 호스트 OS상에서 SW적으로 구동하기때문에 성능의 하락이 발생
- 가상머신에서 하드웨어 접근 시 기존의 Native시스템에 비해 거치는 단계가 늘어나므로 명령 처리 루틴이 길어짐

- Native > 반가상화 > 전가상화> 에뮬레이션



#### 가상머신의 독립성(Isolation)

- 하이퍼바이저에 의해 구동되는 가상머신은 각 가상머신별로 **독립된 기싱의 자원을 할당**받음
  - 하드웨어 특정 영역에 대한 접근 권한을 보장 받는다는 의미
  - 메모이릐 경우 하드웨어 용량 이상을 할당 받기도 하며, 페이징을 통해 메모리 황용을 지원
- **가상머신들은 동일 하드웨어에서 구동되어도 논리적으로 분리되어있어 한 가상머신에 오류가 발생하거나작동이 멈추어도 다른 가상머신 및 시스템으로 확산되지 않음**
  - **하이퍼바이저의 가장 큰 장점**
  - 컨테이너 기반 가상화는 약함
- 하이퍼바이저는 실행중에는 하드웨어 설정(메모리 용량 등..)을 할 수 없음
  - 컨테이너는 가능

