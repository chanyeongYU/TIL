# Jenkins

### Jenkins Install

##### JDK Install

- Java기반으로 만들어졌기 때문에

- [root@demo ~]# `yum -y install java-1.8.0-openjdk java-1.8.0-openjdk-devel`



##### Jenkins Install

- 공식 설치 문서

  - https://pkg.jenkins.io/redhat-stable/

    또는

  - [root@demo ~]# `yum -y install http://mirrors.jenkins-ci.org/redhat-stable/jenkins-2.235.5-1.1.noarch.rpm`



### Jenkins 구동

- [root@demo ~]# `systemctl start jenkins.service`

- 호스트PC 웹 브라우저에서 `192.168.33.10:8080` 접속

  - 접속 안될 경우 방화벽 등록

    [root@demo ~]# `systemctl status firewalld`

    - 방화벽이 켜져있는지 확인
    - 켜져 있어여 설정 가능

    [root@demo ~]# `sudo firewall-cmd --zone=public --permanent --add-port=8080/tcp`

    ```
    success
    ```

    [root@demo ~]# `firewall-cmd --reload`

    ```
    success
    ```

   <img src="..\img\image-20200911145341234.png" alt="image-20200911145341234" style="zoom:67%;" />

### Jenkins 접속

- 접속에 필요한 Password확인\

  [root@demo ~]# `cat /var/lib/jenkins/secrets/initialAdminPassword`

- 웹브라우저의 패스워드 입력필드에 위의 값을 입력



##### Jenkins 초기 설정

1. Install suggested plugins 클릭

2. 설치를 진행

3. 관리자 계정 설정
4. 재시작







#### 정해진 시각에 실행되는 프로젝트 생성



## Jenkins에서 Ansible 실행

#### Sample Code

- [vagrant@demo ~]$ `cd /tmp`
- [vagrant@demo tmp]$ `git clone https://github.com/devops-book/ansible-playbook-sample.git`



####  **jenkins 사용자가 패스워드 없이 sudo 명령을 실행할 수 있도록 권한을 추가**

- [vagrant@demo tmp]$ `sudo vi /etc/sudoers.d/jenkins`

  ```
  jenkins ALL=(ALL) NOPASSWD:ALL
  ```



#### exec-ansible 프로젝트 생성

- 





## Jenkins에서 Serverspec 실행

[vagrant@demo ansible-playbook-sample]$ `ansible-playbook -i development site.yml`





## Pipeline으로 프로젝트 연결

- exec-ansible에서 후행 프로젝트로 serverspec을 설정해둔 것은 해제

- Pipeline 생성 후 Script작성

  - stage: step(단계)

  ```j
  node {
  	stage 'ansible'
  	build 'exec-ansible'
  	stage 'serverspec'
  	build 'exec-serverspec'
  }
  ```

- 효율적으로 작업들을 묘사할 수 있음





- 테스트에 사용된 테스트 케이스 파일을 확인

  - [vagrant@demo ~]$ `cat /tmp/serverspec_sample/spec/localhost/web_spec.rb`

    ```
    describe package('nginx') do
      it { should be_installed }
    end
    
    describe service('nginx') do
      it { should be_enabled }
      it { should be_running }
    end
    
    describe port(80) do
      it { should be_listening }
    end
    
    describe file('/usr/share/nginx/html/index.html') do
      it { should be_file }
      it { should exist }
      its(:content) { should match /^Hello, development ansible!!$/ }
    ```

    



