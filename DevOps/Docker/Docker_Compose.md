# Docker Compose

#### 설치

- https://docs.docker.com/compose/install/

- vagrant@xenial64:~$ `sudo curl -L "https://github.com/docker/compose/releases/download/1.27.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose`

  ```
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                   Dload  Upload   Total   Spent    Left  Speed
  100   651  100   651    0     0   6415      0 --:--:-- --:--:-- --:--:--  6445
  100 11.6M  100 11.6M    0     0  1929k      0  0:00:06  0:00:06 --:--:-- 2454k
  ```

- vagrant@xenial64:~$ `sudo chmod +x /usr/local/bin/docker-compose`

- vagrant@xenial64:~$ `docker-compose --version`

  ```
  docker-compose version 1.27.2, build 18f557f9
  ```

  

### docker-compose명령어로  컨테이너를 실행

1.  **작업 디렉터리 및 docker-compose.yml 파일을 생성**

   - vagrant@xenial64:~$` mkdir ~/compose && cd ~/compose`
   - vagrant@xenial64:~/compose$ `vim docker-compose.yml`

   ```yaml
   version: "3"                          # 문법 버전
   services:            
     echo:                               # 컨테이너 이름
       image: myanjini/echo:latest       # 컨테이너 생성에 사용할 도커 이미지 
       ports:
         - 9000:8080                     # 포트 포워딩 ⇒ 호스트:컨테이너
   ```

2. **컨테이너 실행**

   - vagrant@xenial64:~/compose$ `docker-compose up`

3. **다른 터미널에서 컨테이너 생성 여부를 확인**

   - vagrant@xenial64:~$ `docker container ls`

   ```
   CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    NAMES
   1166db9dfbb2        myanjini/echo       "go run /echo/main.go"   3 minutes ago       Up 3 minutes        0.0.0.0:9000->8080/tcp   compose_echo_1
   ```

4.  **컨테이너 중지 (중지와 함께 삭제도 수행)**

   - vagrant@xenial64:~/compose$ `docker-compose down`

   ```
   Stopping compose_echo_1 ... done
   Removing compose_echo_1 ... done
   Removing network compose_default
   ```





### 이미지를 만들고 컨테이너를 실행

1. **기존에 만들어 놓은 Dockerfile, main.go 파일을 작업 디렉터리로 복사**

   - vagrant@xenial64:~/compose$ `cp ../chap02/Dockerfile ./`
   - vagrant@xenial64:~/compose$ `cp ../chap02/main.go ./`
   - vagrant@xenial64:~/compose$ `ls`

   ```
   docker-compose.yml  Dockerfile  main.go
   ```

2. **Dockerfile을 이용해서 이미지를 빌드 후 실행도록 docker-compose.yml 파일을 수정**

   - vagrant@xenial64:~/compose$ `vim docker-compose.yml`

   ```yaml
   version: "3"
   services:
     echo:
       build: .
       ports:
         - 9000:8080
   ```


3. 빌드 후 실행

   - vagrant@xenial64:~/compose$ `docker-compose up -d --build`
     - `-d` 옵션을 추가했기 때문에 명령어 입력이 가능 (백그라운드 실행)

   ```
    Creating network "compose_default" with the default driver
      Building echo			# --build 옵션을 추가했기 때문에 이미지를 빌드(생성)
      Step 1/4 : FROM   golang:1.9
      1.9: Pulling from library/golang
      55cbf04beb70: Already exists
      1607093a898c: Already exists
      9a8ea045c926: Already exists
      d4eee24d4dac: Already exists
      9c35c9787a2f: Already exists
      8b376bbb244f: Already exists
      0d4eafcc732a: Already exists
      186b06a99029: Already exists
      Digest: sha256:8b5968585131604a92af02f5690713efadf029cc8dad53f79280b87a80eb1354
      Status: Downloaded newer image for golang:1.9
       ---> ef89ef5c42a9
      Step 2/4 : RUN    mkdir   /echo
       ---> Running in e08081daeb1d
      Removing intermediate container e08081daeb1d
       ---> d1039a14bb02
      Step 3/4 : COPY   main.go   /echo
       ---> 0ee3d1b9bb46
      Step 4/4 : CMD   [ "go", "run", "/echo/main.go" ]
       ---> Running in 564681f6cbaa
      Removing intermediate container 564681f6cbaa
       ---> de49d58f71ea
      
      Successfully built de49d58f71ea
      Successfully tagged compose_echo:latest
      Creating compose_echo_1 ... done
   ```

   - vagrant@xenial64:~/compose$ `docker image ls`

   ```
   REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
   compose_echo        latest              de49d58f71ea        4 minutes ago       750MB
   golang              1.9                 ef89ef5c42a9        2 years ago         750MB
   ```

   - vagrant@xenial64:~/compose$ `docker container ls`

   ```
   CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    NAMES
   9bc514c2ce6c        compose_echo        "go run /echo/main.go"   5 minutes ago       Up 5 minutes        0.0.0.0:9000->8080/tcp   compose_echo_1
   ```



### Jenkins  컨테이너를 실행

- vagrant@xenial64:~/compose$ `vim docker-compose.yml`

  ```yaml
  version: "3"
  services:
    master:
      container_name: master
      image: jenkinsci/jenkins
      ports:
        - 8080:8080
      volumes:
        - ./jenkins_home:/var/jenkins_home
  ```

- vagrant@xenial64:~/compose$ `docker-compose up`









