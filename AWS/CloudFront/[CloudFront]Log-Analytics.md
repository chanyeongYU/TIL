# CloudFront Log 분석

- 목표: CDN(CloudFront) 서비스로 부터 AWS S3의 Object(이미지 등)를 요청 받을 때, 로그를 분석하여 저장한다.

- 환경
  - AWS S3: 오브젝트 저장
  - AWS CloudFront: CDN Server
  - AWS Kinesis Data Stream: CloudFront로 부터 실시간 로그를 전송받고, Kinesis Data Analytics로 데이터 전송
  - AWS Kinesis Data Analytics: Kinesis Data Stream으로 부터 받은 로그를 분석