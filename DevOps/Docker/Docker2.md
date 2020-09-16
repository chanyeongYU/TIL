## Docker

#### 우분투 가상머신 설치 및 도커 설치

- 작업 Directory 생성

  - C:\> `mkdir c:\docker`
  - C:\> `cd c:\docker`

- VagrantFile 생성

  - c:\docker> `vagrant init`
  - c:\docker\Vagrantfile

      ```
      # -*- mode: ruby -*-
      # vi: set ft=ruby :

      Vagrant.configure("2") do |config|
        config.vm.box = "ubuntu/xenial64"
        config.vm.hostname = "xenial64"
        config.vm.synced_folder ".", "/vagrant_data", disabled: true
      end
      ```

- 가상머신 생성
  - c:\docker> `vagrant up`
    - 로그 중에서 중요한 부분들

          ...
          default: 22 (guest) => 2222 (host) (adapter 1)	# 포트포워딩
          ...
          default: SSH address: 127.0.0.1:2222
          default: SSH username: vagrant
          default: SSH auth method: private key	# Private Key 위치는 vagrant ssh-config
      
  
- 가상머신에 SSH로 접속

  - c:\docker> `vagrant ssh`

- 패키지 업데이트

  - vagrant@xenial64:~$ `sudo apt update`
  - vagrant@xenial64:~$ `sudo apt upgrade`

- 도커 설치

  - vagrant@xenial64:~$ `sudo apt install -y docker.io`
  - vagrant@xenial64:~$ `sudo usermod -a -G docker $USER`
  - vagrant@xenial64:~$ `sudo service docker restart`
  - vagrant@xenial64:~$ `sudo chmod 666 /var/run/docker.sock`
  - vagrant@xenial64:~$ `docker --version`

      ```
      Docker version 18.09.7, build 2d0083d
      ```

  

#### 도커 이미지 생성

- 작업 디렉토리 생성 및 이동

  - vagrant@xenial64:~$ `mkdir chap01 && cd chap01`

- 쉘스크립트 파일 생성 및 실행 권한 부여

  - vagrant@xenial64:~/chap01$ `vim helloworld`

    ```
    #!/bin/sh
    
    ```

echo "Hello World!"
    ```
    
  - vagrant@xenial64:~/chap01$ `ll`
  
    - `ls -l`과 동일한 명령어
    
    ```
    drwxrwxr-x 2 vagrant vagrant 4096 Sep 14 04:20 ./
    drwxr-xr-x 5 vagrant vagrant 4096 Sep 14 04:20 ../
    -rwxr-xr-x 1 vagrant vagrant   31 Sep 14 04:20 helloworld*
    ```
  
- Dockerfile 생성

  - vagrant@xenial64:~/chap01$ `vim Dockerfile`

    ```dockerfile
    # 베이스 이미지 정의
    FROM ubuntu:16.04
    # 호스트 파일을 컨테이너 안으로 복사
    COPY helloworld /usr/local/bin
    # 도커 빌드 과정에서 컨테이너 안에서 실행할 명령어
    RUN chmod +x /usr/local/bin/helloworld
    # 도커 빌드를 통해 만들어질 이미지를 도커 컨테이너로 실행하기 전에 실행 할 명령어
    CMD [ "helloworld" ]
    ```

- Dockerfile을 사용해서 이미지 빌드

  - vagrant@xenial64:~/chap01$ `docker image build -t helloworld:latest .`

    - `docker image build`: Dockerfile 명세에 맞춰 이미지를 생성
    - `-t helloworld:latest`: 이미지 이름을 명시 (사용자명/이미지명:태그명)
    - `.`: 도커 파일 위치 (현재 디렉터리)

    ```
    Sending build context to Docker daemon  3.072kB
    Step 1/4 : FROM ubuntu:16.04
    
    # 이미지 이름에 사용자 명이 기술되어 있지 않으면 공식 이미지를 사용
    16.04: Pulling from library/ubuntu
    
    # 이미지가  여러 개의 파일로 분리되어 있는 것을 확인
    8e097b52bfb8: Pull complete
    a613a9b4553c: Pull complete
    acc000f01536: Pull complete
    73eef93b7466: Pull complete
    
    Digest: sha256:3dd44f7ca10f07f86add9d0dc611998a1641f501833692a2651c96defe8db940
    Status: Downloaded newer image for ubuntu:16.04
     ---> 4b22027ede29
    Step 2/4 : COPY helloworld /usr/local/bin
     ---> 236c04c7aac4
    Step 3/4 : RUN chmod +x /usr/local/bin/helloworld
     ---> Running in 52c0ada865bf
    Removing intermediate container 52c0ada865bf
     ---> 7a3aacb3ad09
    Step 4/4 : CMD [ "helloworld" ]
     ---> Running in 17cbb3c27e90
    Removing intermediate container 17cbb3c27e90
     ---> 540e40dd066b
    Successfully built 540e40dd066b
    Successfully tagged helloworld:latest
    ```

- 생성된 이미지를 조회

  - vagrant@xenial64:~/chap01$ `docker image ls`

    ```
    REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
    helloworld          latest              540e40dd066b        5 minutes ago       127MB
    ubuntu              16.04               4b22027ede29        3 weeks ago         127MB
    ```

- 컨테이너 실행 및 확인

  - vagrant@xenial64:~/chap01$ `docker container run helloworld:latest`

    ```
    Hello World!
    ```

  - vagrant@xenial64:~/chap01$ `docker container ps

    ```
    CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
    ```

  - vagrant@xenial64:~/chap01$ `docker container ps -a`

    ```
    CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                      PORTS               NAMES
    0ba518eb9000        helloworld:latest   "helloworld"        24 seconds ago      Exited (0) 23 seconds ago                       heuristic_archimedes
    ```



#### 도커 이미지로 도커 컨테이너 생성

- gihyodocker/echo:latest 이미지를 이용해서 컨테이너를 생성, 실행

  - vagrant@xenial64:~/chap01$ `docker image pull gihyodocker/echo:latest`

- vagrant@xenial64:~/chap01$ `docker image ls`

  - 로컬 레포지토리에 저장된 이미지를 조회

  ```
  REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
  helloworld          latest              540e40dd066b        About an hour ago   127MB
  ubuntu              16.04               4b22027ede29        3 weeks ago         127MB
  gihyodocker/echo    latest              3dbbae6eb30d        2 years ago         733MB
  ```

- Docker Container run 명령으로 컨테이너 실행

  - vagrant@xenial64:~/chap01$ `docker container run -t -p 9000:8080 gihyodocker/echo:latest`
    - `-t`옵션: 가상 콘솔을 보여줌, `-i`옵션과 같이 자주 쓰임
    - `-p`옵션: 포트 포워딩

  ```
  2020/09/14 05:40:42 start server
  (제어권을 반환하지 않고 멈춘 상태)
  ```

- CMD창 하나 더 열어서 SSH접속

- 두 번째 터미널에서 컨테이너 실행 확인

  - vagrant@xenial64:~$ `docker container ps`

  ```
  CONTAINER ID        IMAGE                     COMMAND                  CREATED             STATUS              PORTS                    NAMES
  976842a60fc0        gihyodocker/echo:latest   "go run /echo/main.go"   4 minutes ago       Up 4 minutes        0.0.0.0:9000->8080/tcp   optimistic_jepsen
  ```

- 두 번째 터미널에서 eurl명령어로 컨테이너로 요청 전송

  - vagrant@xenial64:~$ `curl http://localhost:9000`

  ```
  Hello Docker!!
  ```

  - 첫번째 터미널에서 로그가 생성되는것 확인

  ```
  2020/09/14 05:41:01 start server
  2020/09/14 05:48:20 received request
  2020/09/14 05:48:24 received request
  ```

- 두번째 터미널에서 docker container stop 명령으로 컨테이너를 중지

  - vagrant@xenial64:~$ `docker container stop 97`
    - 컨테이너 이름 또는 ID (ID는 식별 가능한 범위까지만 입력)
  - 또는 첫 번째 터미널에서 `Ctrl + C`

- 중지된 컨테이너 확인

  - `vagrant@xenial64:~$ docker container ps -a`

  ```
  CONTAINER ID        IMAGE                     COMMAND                  CREATED             STATUS                          PORTS               NAMES
  976842a60fc0        gihyodocker/echo:latest   "go run /echo/main.go"   14 minutes ago      Exited (2) About a minute ago                       optimistic_jepsen
  0ba518eb9000        helloworld:latest         "helloworld"             About an hour ago   Exited (0) About an hour ago                        heuristic_archimedes
  ```



#### 간단한 어플리케이션과 도커 이미지 만들기

- vagrant@xenial64:~$ `mkdir chap02 && cd chap02` 

-  main.go 작성 

  - 8080 포트로 요청을 대기하고, 요청이 들어 왔을 때 Hello, Docker!! 라는 메시지를 반환
  - vagrant@xenial64:~/chap01$ `vim main.go`

  ```go
  package main
  
  import (
  	"fmt"
  	"log"
  	"net/http"
  )
  
  func main() {
  	http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
  		log.Println("received request")
  		fmt.Fprintf(w, "Hello Docker!!")
  	})
  	log.Println("start server")
  	server := &http.Server{ Addr: ":8080" }
  	if err := server.ListenAndServe(); err != nil {
  		log.Println(err)
  	}
  }
  ```

- Dockerfile 생성

  - vagrant@xenial64:~/chap02$ `vim Dockerfile`

  ```dockerfile
  FROM   golang:1.9
  
  RUN    mkdir   /echo
  
  COPY   main.go   /echo
  
  # "go run /echo/main.go" 콤마로 구분해서 작성
  CMD   [ "go", "run", "/echo/main.go" ]
  ```

- 이미지 생성

  - vagrant@xenial64:~/chap02$ `docker image build -t example/echo:latest .`

- 컨테이너 실행

  - vagrant@xenial64:~/chap02$ `docker container run example/echo:latest`
- vagrant@xenial64:~/chap02$ `docker image ls`
  
```
  REPOSITORY          TAG                 IMAGE ID            CREATED              SIZE
  #  새로 만든 이미지
  example/echo        latest              d7f6b8338df2        About a minute ago   750MB
  # 이전에 만들었던 이미지
  <none>              <none>              ff0d7cfd440e        9 minutes ago        750MB
  helloworld          latest              540e40dd066b        2 hours ago          127MB
  ubuntu              16.04               4b22027ede29        3 weeks ago          127MB
  golang              1.9                 ef89ef5c42a9        2 years ago          750MB
  gihyodocker/echo    latest              3dbbae6eb30d        2 years ago          733MB
  ```
  
- 컨테이너 실행

  - vagrant@xenial64:~/chap02$ `docker container run example/echo:latest`

  ```
  2020/09/14 06:54:32 start server
  (제어권을 반환하지 않고 멈춘 상태, 입력 불가능한 상태)
  (컨테이너에서 실행되는 main.go가 서비스 형태로 동작)
  ```

- `-d`옵션으로 컨테이너를 백그라운드에서 실행

  - vagrant@xenial64:~/chap02$ `docker container run -d example/echo:latest`

  ```
  7b80542a52f9d914438ac4b78c972cc68e2ca65d966c217029da185cdf91befd
  (입력 가능한 상태)
  ```

  - vagrant@xenial64:~/chap02$ `docker container ls`

  ```
  CONTAINER ID        IMAGE                 COMMAND                  CREATED             STATUS              PORTS               NAMES
  7b80542a52f9        example/echo:latest   "go run /echo/main.go"   3 minutes ago       Up 3 minutes                            suspicious_newton
  ```



#### 컨테이너 중지

- 실행 중인 컨터이너를 중지

  - `docker container stop [CONTAINER_ID]`
  - vagrant@xenial64:~/chap02$ `docker container stop 7b`

  

##### 같은 이미지로 생성된 컨테이너를 일괄적으로 중지

- vagrant@xenial64:~/chap02$ docker container run -d example/echo:latest

```
580c0f2f07fbc05119d8e0e3df6895419117e2bfea8f5615366b1dbf67913f14
```

- vagrant@xenial64:~/chap02$ docker container run -d example/echo:latest

```
91f9963f499c6ecdf90df51166b196eaaab662a1d6b12eb74520e99903c03a5e
```

- vagrant@xenial64:~/chap02$ docker container run -d example/echo:latest

```
dfe4d4ef8e4b6333c29236fae78eb14fe1efc58af595de5b951450fce8152d8e
```

- vagrant@xenial64:~/chap02$ `docker container ls`

```
CONTAINER ID        IMAGE                 COMMAND                  CREATED             STATUS              PORTS               NAMES
dfe4d4ef8e4b        example/echo:latest   "go run /echo/main.go"   51 seconds ago      Up 51 seconds                           sad_neumann
91f9963f499c        example/echo:latest   "go run /echo/main.go"   55 seconds ago      Up 54 seconds                           romantic_villani
580c0f2f07fb        example/echo:latest   "go run /echo/main.go"   3 minutes ago       Up 3 minutes                            eager_shannon
```

- vagrant@xenial64:~/chap02$ `docker container ls --filter "ancestor=example/echo"`
  - `example/echo`로만 만들어진 컨테이너 조회

```
CONTAINER ID        IMAGE                 COMMAND                  CREATED             STATUS              PORTS               NAMES
dfe4d4ef8e4b        example/echo:latest   "go run /echo/main.go"   2 minutes ago       Up 2 minutes                            sad_neumann
91f9963f499c        example/echo:latest   "go run /echo/main.go"   2 minutes ago       Up 2 minutes                            romantic_villani
580c0f2f07fb        example/echo:latest   "go run /echo/main.go"   5 minutes ago       Up 5 minutes                            eager_shannon
```

- 컨테이너 조회 결과에서 컨테이너 ID만 추출

  - vagrant@xenial64:~/chap02$ `docker container ls --filter "ancestor=example/echo" -q`

  ```
  dfe4d4ef8e4b
  91f9963f499c
  580c0f2f07fb
  ```

- 일괄 중지

  - vagrant@xenial64:~/chap02$ `docker container stop $(docker container ls --filter "ancestor=example/echo" -q)`

  ```
  dfe4d4ef8e4b
  91f9963f499c
  580c0f2f07fb
  ```

- vagrant@xenial64:~/chap02$ `docker container ls`

```
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```



#### `-p` 옵션을 사용해 포트 포워딩

- `-p [Host Port]:[Container Port]`

- vagrant@xenial64:~/chap02$ `docker container run -d -p 9090:8080 example/echo:latest`

  ```
  d20882abfd69786e70c0b7a3cb6ae886586d2030da66d741a824d482cf3bd031
  ```

- vagrant@xenial64:~/chap02$ `curl localhost:9090`

  ```
  Hello Docker!!
  ```



##### 호스트 포트를 생략하는 경우

- 자동으로 할당

- vagrant@xenial64:~/chap02$ `docker container run -d -p 8080 example/echo:latest`

  ```
  d4702fde9be8dd8c4cef8631fd6c45c3a909a4a3285c0ffa210aec8be8ff2298
  ```

- vagrant@xenial64:~/chap02$ `docker container ls`

  ```
  CONTAINER ID        IMAGE                 COMMAND                  CREATED             STATUS              PORTS                     NAMES
  d4702fde9be8        example/echo:latest   "go run /echo/main.go"   35 seconds ago      Up 34 seconds       0.0.0.0:32768->8080/tcp   jovial_bardeen
  d20882abfd69        example/echo:latest   "go run /echo/main.go"   2 minutes ago       Up 2 minutes        0.0.0.0:9090->8080/tcp    pedantic_saha
  ```

  - 컨테이너ID d4702fde9be8의 포트 32768가 자동으로 할당됨

- vagrant@xenial64:~/chap02$ `curl localhost:32768`

  ```
  Hello Docker!!
  ```



#### 도커 이미지, 컨테이너 일괄 삭제

##### 컨테이너 삭제

- 모든 컨테이너(실행 중, 종료..) 아이디 추출

  - vagrant@xenial64:~/chap02$ `docker container ls -a -q`

  ```
  d4702fde9be8
  d20882abfd69
  dfe4d4ef8e4b
  91f9963f499c
  580c0f2f07fb
  7b80542a52f9
  36fcda32e5d8
  f46056320ec3
  46aecaab2403
  c13e9a3ba6e8
  76b162f19ac9
  5ee32290ebbc
  976842a60fc0
  0ba518eb9000
  ```

- 일괄 삭제

  - vagrant@xenial64:~/chap02$ `docker container rm -f $(docker container ls -a -q)`
    - `docker container rm -f`: 컨테이너를 강제로 삭제 (실행 중인 컨테이너는 중지 후 삭제)
    - `$(docker container ls -a -q)`: 모든 컨테이너(실행, 중지 상태 모두)의 ID를 반환

  ```
  d4702fde9be8
  d20882abfd69
  dfe4d4ef8e4b
  91f9963f499c
  580c0f2f07fb
  7b80542a52f9
  36fcda32e5d8
  f46056320ec3
  46aecaab2403
  c13e9a3ba6e8
  76b162f19ac9
  5ee32290ebbc
  976842a60fc0
  0ba518eb9000
  ```

- vagrant@xenial64:~/chap02$ `docker image ls -a`

  ```
  CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
  ```



##### 이미지 삭제

- vagrant@xenial64:~/chap02$ `docker image ls -a`

  ```
  REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
  example/echo        latest              d7f6b8338df2        About an hour ago   750MB
  <none>              <none>              25cb7cc9a87f        About an hour ago   750MB
  <none>              <none>              e0f006b2909c        About an hour ago   750MB
  <none>              <none>              f59f6ac9c36e        About an hour ago   750MB
  <none>              <none>              ff0d7cfd440e        About an hour ago   750MB
  helloworld          latest              540e40dd066b        3 hours ago         127MB
  <none>              <none>              7a3aacb3ad09        3 hours ago         127MB
  <none>              <none>              236c04c7aac4        3 hours ago         127MB
  ubuntu              16.04               4b22027ede29        3 weeks ago         127MB
  golang              1.9                 ef89ef5c42a9        2 years ago         750MB
  gihyodocker/echo    latest              3dbbae6eb30d        2 years ago         733MB
  ```

- vagrant@xenial64:~/chap02$ `docker image rm -f $(docker image ls -q)`

- vagrant@xenial64:~/chap02$ `docker image ls -a`

  ```
  REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
  ```



#### docker search 명령을 이용한 리포지토리 검색

- vagrant@xenial64:~/chap02$ `docker search mysql`

  ```
  NAME                              DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
  mysql                             MySQL is a widely used, open-source relation…   9961                [OK]    
  mariadb                           MariaDB is a community-developed fork of MyS…   3644                [OK]    
  mysql/mysql-server                Optimized MySQL Server Docker images. Create…   725                                     [OK]
  percona                           Percona Server is a fork of the MySQL relati…   509                 [OK]    
  centos/mysql-57-centos7           MySQL 5.7 SQL database server                   83                          
  mysql/mysql-cluster               Experimental MySQL Cluster Docker images. Cr…   75
  ...
  ```

- STARS가 많은 상위 5개만 조회

  - vagrant@xenial64:~/chap02$ `docker search --limit 5 mysql1`

  ```
  NAME                  DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
  mysql                 MySQL is a widely used, open-source relation…   9961                [OK]
  mysql/mysql-server    Optimized MySQL Server Docker images. Create…   725                                     [OK]
  mysql/mysql-cluster   Experimental MySQL Cluster Docker images. Cr…   75
  bitnami/mysql         Bitnami MySQL Docker Image                      44                                      [OK]
  circleci/mysql        MySQL is a widely used, open-source relation…   19
  ```

  

#### 도커 허브에서 리포지터리를 검색할 수 있도록 API를 제공

- https://hub.docker.com/v2/repositories/uchan0/centos/

- `curl https://hub.docker.com/v2/repositories/uchan0/centos/tags`

  - 단순 문자열 형태로 출력되어 가독성 떨어짐
  - `jq` 설치
    -  vagrant@xenial64:~/chap02$ `sudo apt install -y jq`

- vagrant@xenial64:~/chap02$ `curl https://hub.docker.com/v2/repositories/uchan0/centos/tags | jq`

  ```json
    % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                   Dload  Upload   Total   Spent    Left  Speed
    0     0    0     0    0     0      0      0 --:--:--   0     0    0     0    0     0      0      0 --:--:-- 100   484  100   484    0     0    470      0  0:00:01  0:00:01 --:--:--   470
  {
    "count": 1,
    "next": null,
    "previous": null,
    "results": [
      {
        "creator": 10836467,
        "id": 116913087,
        "image_id": null,
        "images": [
          {
            "architecture": "amd64",
            "features": "",
            "variant": null,
            "digest": "sha256:99127cd5fe859271d7cff3f58e9485ff5dd195fc82152326f402aae478f20134",
            "os": "linux",
            "os_features": "",
            "os_version": null,
            "size": 126772034
          }
        ],
        "last_updated": "2020-09-11T03:01:10.94061Z",
        "last_updater": 10836467,
        "last_updater_username": "uchan0",
        "name": "1.1",
        "repository": 9810242,
        "full_size": 126772034,
        "v2": true
      }
    ]
  }
  ```

- `results`부분만 추출하기

  - vagrant@xenial64:~/chap02$ `curl https://hub.docker.com/v2/repositories/uchan0/centos/tags | jq '.results[]'`

    ```
      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
      0     0    0     0    0     0      0      0 --:--:-- 100   484  100   484    0     0    489      0 --:--:-- 100   484  100   484    0     0    486      0 --:--:-- --:--:-- --:--:--   486
    "1.1"
    ```

    

  - vagrant@xenial64:~/chap02$ `curl https://hub.docker.com/v2/repositories/uchan0/centos/tags | jq '.results[].name'`

    ```
      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
      0     0    0     0    0     0      0      0 --:--:--   0     0    0     0    0     0      0      0 --:--:-- 100   484  100   484    0     0    564      0 --:--:-- --:--:-- --:--:--   564
    {
      "creator": 10836467,
      "id": 116913087,
      "image_id": null,
      "images": [
        {
          "architecture": "amd64",
          "features": "",
          "variant": null,
          "digest": "sha256:99127cd5fe859271d7cff3f58e9485ff5dd195fc82152326f402aae478f20134",
          "os": "linux",
          "os_features": "",
          "os_version": null,
          "size": 126772034
        }
      ],
      "last_updated": "2020-09-11T03:01:10.94061Z",
      "last_updater": 10836467,
      "last_updater_username": "uchan0",
      "name": "1.1",
      "repository": 9810242,
      "full_size": 126772034,
      "v2": true
    }
    ```

    

