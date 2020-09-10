# Ansible

- Python으로 만들어진 인프라 구성 관리 도구
- VirtualBox와 Vagrant를 이용하여 Local개발 환경을 구축해도 환경의 효율적 관리, 이용을 고려하면 아직까지 부족함
  - 구축 절차를 알기 어려움
  - 설정을 추가할 수 없음
  - 구축 절차를 다른 환경에서 유용하기 힘듬
- 환경 설정 및 구축 절차를 통일된 형식으로 기술
- 매개 변수 등 환경의 차이를 관리
- 실행 전에 변경되는 부분을 파악



## 인프라 구성 관리 도구

- 선언적
- 추상화
- 수렴화
- 멱등성
- 간소화



### 구성

- Ansible 본체
- Inventory
- Module: 실행할 명령어



### Install

- nginx가 설치되어 있는지 확인
  - `vagrant ssh`
  - `systemctl status nginx`
  - `sudo systemctl stop nginx.service`
- 설치
  - `sudo yum install -y epel-release`
  - `sudo yum install -y ansible`
  - `ansible --version`



###  ansible 명령으로 nginx를 기동

- 서버 인벤토리에 Localhost  추가
  - `sudo sh -c "echo \"localhost\" >> /etc/ansible/hosts"`

- nginx를 중지 상태에서 실행 상태로 변경
  -  ``ansible localhost -b -c local -m service -a "name=nginx state=started"`
    - **localhost**: 인벤토리 파일에 기재된 서버 중 이번 명령어를 수행 할 대상
    - **-b**: 원격 실행되는 대상 서버의 사용자 (-b = root)
    - **-c local**: 대상 서버가 자기 자신이므로 SSH를 사용하지 않고 local로 연결
    - **-m service**: service 모듈을 이용
    - **-a "name=nginx state=started"**: 모듈의 추가 인자
- nginx가 상태 확인
  - `systemctl status nginx`

- nginx가 실행 상태일 때 ansible 명령(nginx 서비스를 시작)를 실행
  -  ``ansible localhost -b -c local -m service -a "name=nginx state=started"`
  - 실행 상태였기 때문에 상태 변화가 일어나지 않음



### Git 설치

- `sudo yum install -y git `



### **ansible-playbook-sample**

- `git clone https://github.com/devops-book/ansible-playbook-sample.git`



- `ansible-playbook -i development site.yml`

  - -i development: 인벤토리 파일(development) 지정

  - `curl localhost`

    - output

      ```
      hello, development ansible   <=  nginx 홈 디렉토리  index.html의 내용
      ```

- `ansible-playbook -i production site.yml`

  - -i production : 인벤토리 파일(production ) 지정

  - `curl localhost`

    - output

      ```
      hello, production ansible
      ```

- 실행 대상 정의를 확인
  - `cat development`
  - `cat production `
- 실행 내용 정의를 확인
  - `ls ./roles`
    - common jenkins nginx serverspec serverspec_sample
      - 롤 별로 실행될 내용을 담고 있는 디렉터리
  - `ls ./roles/common/tasks/`
    - main.yml
      - common 롤에서 수행해야 할 내용을 정의
  - `ls ./roles/nginx/tasks/`
    - main.yml
      - nginx 롤에서 수행해야 할 내용을 정의

- 템플릿 확인

  - `ls ./roles/nginx/templates/`
    - index.html.j2
  - `cat ./roles/nginx/templates/index.html.j2`
    - hello, {{ env }} ansible

- 템플릿에 사용하는 변수 값을 확인

  - `ls ./group_vars/`
    - development-webservers.yml		production-webservers.yml
  - `cat ./group_vars/development-webservers.yml`
    - env: "development"
  - `cat ./group_vars/production-webservers.yml`
    - env: "production"

- 템플릿 내용을 변경

  - `vi ./roles/nginx/templates/index.html.j2`

    ```
    HELLO, {{ env }} ansible
    ```

- dry-run 모드로 실행

  - `ansible-playbook -i development site.yml --check --diff`
    - **--check**: dry-run 모드로 실행
    - **--diff**: 변경 차이를 표시
  - `curl localhost`

  ```
  hello, production ansible
  ```

  ​			- run-dry 모드로 실행했기 때문에 이전과 동일한 상태로 실행

- 변경 사항을 호스트에 반영

  - `ansible-playbook -i development site.yml --diff`

  - `curl localhost`

  ```
  HELLO, development ansible	⇐ 변경된 내용이 반영된 것을 확인
  ```