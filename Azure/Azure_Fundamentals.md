# Azure Fundamentals

- **Microsoft Azure Virtual Training Day: Fundamentals**
- **2020/10/13 ~ 2020/10/14**
- https://docs.microsoft.com/ko-kr/learn/azure/
- https://docs.microsoft.com/ko-kr/learn/certifications/exams/az-900
- https://docs.microsoft.com/ko-kr/learn/?ocid=AID3001216
- https://docs.microsoft.com/ko-kr/learn/paths/azure-fundamentals/



### 클라우드 주요 개념 및 용어

- 내결함성
- 고 가용성
  - 가동 중지 시간을 최소화
- 재해 복구
- **확장성**
  - Scale Up/Out
  - 늘어나는 것에 초점
- 민첩성
  - 확장을 해야할 때 빠르게 확장
  - 빠르게 배포
- **탄력성**
  - 필요없는 자원을 줄이는 것에 초점
- 글로벌 지원
- 응답 속도

- 예측 비용
  - **사용한만큼 비용 지불**
- 보안
  - 공용 공간이기 때문에 보안이 중요



### 비용

#### 규모 경제(Economies of Scale)

- 작은 규모로 운영하는 것에 비해 더 큰 규모로 운영할 때 더 저렴하고 효율적으로 작업을 수행할 수 있는 능력



##### Cap Ex vs Op Ex

- 자본 지출(Capital Expenditure)
  - 물리적 인프라에 대한 지출을 **선불로 지출**
  - 높은 초기 비용
  - 투자 가치는 시간이 지남에 따라 감소
- 운영 비용(Operatinal Expenditure)
  - 필요에 따라 서비스 또는 제품에 지출되고 즉시 청구
  - 선 결제 비용이 없고, 종량제 사용



#### 소비 기반 모델(Condumption-Based Model)

- 선 결제 비용 없음
- 비용이 많이 드는 인프라를 구매하고 관리할 필요 없음
- 필요 없는 자원에 대한 지출을 중지할 수 있음



### 클라우드 종류

#### 퍼블릭 클라우드

- 클라우드 서비스 또는 호스팅 공급자가 소유
- 여러 조직과 사용자에세 리소스와 서비스 제공
- 네트워크를 통해 연결
- Cap Ex 없음
- 민첩성
  - 응용 프로그램에 빠르게 액세스 가능
- 소비 기반 모델



#### 프라이빗 클라우드

- 클라우드 리소스를 사용하는 조직이 소유 및 운영
- 조직은 그들의 데이터센터에 클라우드 환경을 구축
- 제어력
  - 조직은 리소스를 완벽하게 제어 가능
- 보안



#### 하이브리드 클라우드

- 공용 및 사설 클라우드를 결합하여 응용 프로그램이 가장 적합한 위치에서 실행
- 유동성
  - 가장 유연한 시나리오



### 클라우드 서비스 종류

#### Iaas

- 가장 기본 클라우드 컴퓨팅 서비스 범주
- 클라우드 공급자로부터 가상머신, 스토리지, 네트워크 및 OS를 대여하여 종량제 IT인프라 구축
- 가장 유연한 클라우드 서비스



#### Paas

- 소프트웨어 응용프로그램을 개발, 테스트 및 배포하기 위한 환경을 제공
- 기본 인프라 관리에 신경쓰지 않고 응용프로그램을 신속하게 만들 수 있도록 도와줌



#### Saas

- 최종 사용자를 위해 중앙에서 호스팅되고 관리되는 소프트웨어
- 사용자는 인터넷을 통해 클라우드 기반 앱에 연결하고 사용



### Azure의 핵심 서비스

#### 핵심 Azure 아키텍쳐 구성 요소

- https://docs.microsoft.com/ko-kr/learn/modules/explore-azure-infrastructure/

- Region: 인접한 데이터 센터들의 모임
- Region Pairs
  - 각 Region은 다른  Region과 페어링 연결
  - 대략 300마일 정도 떨어져있음
- Geographies(지리)
  - 데이터 상주 및 규정 준수 경계를 보존하는 개별 시장
- 가용성 영역(AV Zone)
  - Azure 지역 내에서 물리적으로 분리된 데이터 센터
- 가용성 집합(AV Set)
  - UD(Update Domains)
    - 예약된 유지 관리, 성능 또는 보안 업데이트는 업데이트 도메인을 통해 순서가 정해짐
  - FD(Fault Domains)
    - 데이터 센터 내에서 여러 하드웨어에서 워크로드를 물리적으로 분리



#### Azure 논리적 구조



#### 데이터 형태(Azure Data Categories)

- 정형 데이터
- 반정형 데이터
- 비정형 데이터



#### Azure Storage Service

##### IasS

- Disks
  - 영구 디스크
- Files



##### PaaS

- Containers
  - 비정형 데이터: 텍스트 및 바이너리
- Tables
  - NoSQL 데이터 저장
  - 사용에 따른 동적 확장 지원
  - 빠른 K/V 조회 지원
- Queues
  - 메시지 저장 및 처리
  - 높은 확장성
  - 비동기적 메시지 처리



#### Azure Service 종류

Azure IoT Solution

Azure advisor

Azure FireWall

Azure Active Directory

​	**SSO**

Azure Multi-Factor Authentication

Azure Key Vault



#### 비용 최소화

1. **Perform**
   - 비용 분석 수행
     - **가격 계산기** 또는 **TCO 계산기**
2. **Monitor**
   - **Azure Advisor**를 통한 사용량 모니터
3. **Use**
   - 지출 한도 사용, 평가판 고객 및 일부 신용 기반 Azure 구독을 통해 사용
4. **Use**
   - **Azure 예약** 또는 **Azure Hybrid Benefits(HUB)**활용
5. **Choose**
   - 저렴한 **위치** 및 **지역** 선택
6. **Keep**
   - **Keep-up-to-Date** 최신 Azure 오퍼링 확인
7. **Apply**
   - **태그**를 적용하여 비용 소유자를 식별





