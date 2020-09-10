# Vagrant

해시코드에서 제공하는 가상 환경 구축 도구



## 로컬 개발 환경의 Infrastructure as Code 화

- 기존에 실행중인 가상머신을 모두 중지

- Vagrant 설치
  
  - https://www.vagrantup.com/
  
- 작업 디렉토리 생성
  
  - C:\> `mkdir C:\HashiCorp\WorkDir`
  
- Vagrantfile 생성
  
  - C:\HashiCorp\WorkDir> `vagrant init`
  
- Vagrantfile 편집

    - nginx 설치된 가상환경

    ```
    # -*- mode: ruby -*-
    # vi: set ft=ruby :
    
    Vagrant.configure("2") do |config|
    # config.vm.box = "centos/7"
      config.vm.box = "generic/centos7"
      config.vm.hostname = "demo"
      config.vm.network "private_network", ip: "192.168.33.10"
      config.vm.synced_folder ".", "/home/vagrant/sync", disabled: true
      config.vm.provision "shell", inline: $script
    end
    $script = <<SCRIPT
      yum install -y epel-release
      yum install -y nginx
      echo "Hello, Vagrant" > /usr/share/nginx/html/index.html
      systemctl start nginx
    SCRIPT
    ```

- Provisioning 실행
  - C:\HashiCorp\WorkDir> `vagrant up`
- HostPC에서 가상머신 SSH로 접속
  - C:\HashiCorp\WorkDir> `vagrant ssh`
- nginx 설치 여부 및 index.html 수정 여부 확인
  - [vagrant@demo ~]$ `cat /usr/share/nginx/html/index.html`
  - 방화벽 때문에 막힌다면 방화벽 해제
    - [vagrant@demo ~]$ `sudo systemctl stop firewalld`



### 가상 머신 정지 , 삭제

- 가상머신 정지
  - C:\HashiCorp\WorkDir> `vagrant halt`
- 가상머신 삭제
  - C:\HashiCorp\WorkDir> `vagrant destroy`



### 가상 머신 생성 및 실행

- C:\HashiCorp\WorkDir> `vagrant up`





## Vagrant로 인프라를 구성했을 때 장점

- 환경 구축 작업이 감소
- 환경 공유 용이
- 환경 파악 용이
- 팀 차원의 유지보수 가능

