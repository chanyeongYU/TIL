# AWS IoT Core

- 커넥티드 디바이스가 쉽고 안전하게 클라우드 애플리케이션 및 다른 디바이스와 상호 작용할 수 있게 해주는 관리형 클라우드 서비스
- 디바이스를 AWS 서비스 및 다른 디바이스에 연결
- 데이터 및 상호 작용에 보안을 적용
- 디바이스 데이터를 처리하여 이를 기반으로 운영하고, 디바이스가 오프라인일 때도 애플리케이션이 디바이스와 상호 작용
- AWS IoT Core 메인 페이지
  - https://aws.amazon.com/ko/iot-core/
- AWS IoT Core 기능 페이지
  - https://aws.amazon.com/ko/iot-core/features/
- AWS IoT Core 사용 설명서
  - https://docs.aws.amazon.com/iot/latest/developerguide/what-is-aws-iot.html



### 기능

- AWS IoT 디바이스 SDK
  - 하드웨어 디바이스 또는 모바일 애플리케이션을 AWS IoT Core에 쉽고 빠르게 연결하는 데 도움
  - 디바이스에서 MQTT, HTTP 또는 WebSockets 프로토콜을 사용하여 AWS IoT Core와 연결, 인증, 메시지를 교환
- Device Gateway
  - AWS에 연결하는 IoT 디바이스를 위한 진입점의 역할
  - IoT 디바이스가 안전하고 효율적으로 통신할 수 있도록 도와줌
  - 게시/구독 구조, 1:1, 1: N 가능
  - MQTT, WebSockets 및 HTTP 1.1 프로토콜을 지원
    - MQTT 또는 WebSockets를 통해 연결하는 디바이스의 경우, 디바이스 게이트웨이에서 장기간 양방향 연결을 유지하므로 이러한 디바이스가 언제든 짧은 지연 시간으로 메시지를 주고받을 수 있음
- Message Broker
  - 모든 IoT 디바이스와 애플리케이션에서 짧은 지연 시간으로 안전하게 메시지를 송수신하며 처리량이 높은 게시/구독 메시지 브로커
  - 일대일 명령 및 제어 메시징부터 일대다(백만 개 이상) 브로드캐스트 알림 시스템과 그 사이의 모든 것에 이르는 메시징 패턴을 지원
- 인증 및 권한 부여
  - 모든 연결 지점에서 상호 인증 및 암호화를 제공하므로 디바이스와 AWS IoT Core 간에 입증된 자격 증명 없이는 데이터가 교환되지 않음
  - MQTT를 통해 연결하면 인증서 기반 인증을 사용할 수 있음
- 디바이스 섀도
  - 디바이스가 온라인 상태인지 여부에 관계없이 애플리케이션이 디바이스와 통신할 수 있도록 디바이스 상태를 유지
- 규칙 엔진
  - 인프라를 관리할 필요 없이 글로벌 규모로 연결된 디바이스에서 생성된 데이터를 수집, 처리, 분석하고 이를 기반으로 조치 가능



### 작동 방식

 <img src="C:\Users\chan\AppData\Roaming\Typora\typora-user-images\image-20201124022122782.png" alt="image-20201124022122782" style="zoom:80%;" />![image-20201124022357035](C:\Users\chan\AppData\Roaming\Typora\typora-user-images\image-20201124022357035.png)



- EC2를 이용한 가상 디바이스 생성
  - https://docs.aws.amazon.com/ko_kr/iot/latest/developerguide/creating-a-virtual-thing.html



