- 방법론: 프로젝트를 수행할 때 절차, 방법, 도구 , 산출물 등을 정의하고 있는것



### VirtualBox를 이용한 로컬 개발환경 구축

- 기존에 실행중인 가상머신을 중지 (Power Off)
- 가상머신 이미지를 배포할 시 문제점
  - 파일 용량이 너무 큼
  - 등.. 책 p.57 참고



### Vagrant로 Local 개발 환경의 Infrastructure as Code

- https://www.vagrantup.com/

- VirtualBox ver6.1을 사용하기 위해서는 Vagrant ver2.2.1 이상 사용

- mkdir C:\HashiCorp\WorkDir

- vagrant init

- Vagrantfile 생성

- Vagrantfile  수정

  ```
  # -*- mode: ruby -*-
  
  Vagrant.configure("2") do |config|
    config.vm.box = "generic/centos7"
    config.vm.hostname = "demo"
    config.vm.network = "private_network", ip: "192.168.33.10"
    config.vm.synced_folder ".", "/home/vagrant/sync", disabled: true
  end
  ```

- `vagarant up`







### Vigrant를 이요하여 웹서버가 설치된 가상 머신을 배포

- 팀 전체가 동일한 가상머신 환경을 공유

https://www.vagrantup.com/docs/provisioning