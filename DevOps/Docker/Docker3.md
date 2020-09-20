### Dockerfile으로 이미지 빌드 시 주의사항

- 이미지 빌드가 완료되면 Dockerfile의 명령어 줄 수 만큼 레이어가 존재
  - 실제 컨테이너에서 사용하지 못하는 파일이나 디렉토리가 이미지 레이어에 존재하면 공간만 낭비
  - Dockerfile을 작성할 때 `&&`으로 각 `RUN`명령어를 하나로 묶어서 실행



##### 필요없는 레이어가 있을 경우

- vagrant@xenial64:~/dockerfile_test$ `vim Dockerfile`

  ```dockerfile
  FROM ubuntu
  
  RUN  mkdir /test
  
  RUN  fallocate  -l  100m  /test/dumy
  
  RUN  rm  /test/dumy
  ```

- vagrant@xenial64:~/dockerfile_test$ `docker image build -t falloc_100m .`

- vagrant@xenial64:~/dockerfile_test$ `docker image ls`

  ```
  REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
  falloc_100m         latest              cb419b52df77        23 seconds ago      179MB
  ubuntu              latest              4e2eef94cd6b        3 weeks ago         73.9MB
  ```



##### 명령어를 한 줄로 묶었을 경우

- vagrant@xenial64:~/dockerfile_test$ `vim Dockerfile`

  ```dockerfile
  FROM ubuntu
  
  RUN  mkdir /test  &&  fallocate  -l  100m  /test/dumy  &&  rm  /test/dumy
  ```

- vagrant@xenial64:~/dockerfile_test$ `docker build -t recommand .`

  ```
  REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
  recommand           latest              6a667c9fadb5        4 seconds ago       73.9MB
  ubuntu              latest              4e2eef94cd6b        3 weeks ago         73.9MB
  ```

  

### Docker 컨테이너 생명주기

​	<img src="..\..\img\image-20200915100457505.png" alt="image-20200915100457505"/>



### Docker Container run 명령의 인자를 이용해서 CMD 명령을 오버라이드

##### library/alpine:latest이미지의 Dockerfile확인

- https://hub.docker.com/_/alpine

- https://github.com/alpinelinux/docker-alpine/blob/90788e211ec6d5df183d79d6cb02e068b258d198/x86_64/Dockerfile

  ```
  FROM scratch
  ADD alpine-minirootfs-3.12.0-x86_64.tar.gz /
  CMD ["/bin/sh"]
  ```



##### alpinr 이미지를 이용해서 컨테이너 생성

- vagrant@xenial64:~$ `docker run -it alpine`

  ```
  Unable to find image 'alpine:latest' locally
  latest: Pulling from library/alpine
  df20fa9351a1: Pull complete
  Digest: sha256:185518070891758909c9f839cf4ca393ee977ac378609f700f60a771a2dfe321
  Status: Downloaded newer image for alpine:latest
  / #			<= 컨테이너가 생성되면 컨테이너 내부에 shell을 실행
  ```



##### CMD명령을 오버라이드

- vagrant@xenial64:~$ `docker run -it alpine uname -a`

  ```
  uname -a의 결과만 반환
  Linux e3d0c031162f 4.4.0-189-generic #219-Ubuntu SMP Tue Aug 11 12:26:50 UTC 2020 x86_64 Linux
  ```

- vagrant@xenial64:~$ `docker run -it alpine ls -al`

  ```
  total 64
  drwxr-xr-x    1 root     root          4096 Sep 15 01:14 .
  drwxr-xr-x    1 root     root          4096 Sep 15 01:14 ..
  -rwxr-xr-x    1 root     root             0 Sep 15 01:14 .dockerenv
  drwxr-xr-x    2 root     root          4096 May 29 14:20 bin
  drwxr-xr-x    5 root     root           360 Sep 15 01:14 dev
  drwxr-xr-x    1 root     root          4096 Sep 15 01:14 etc
  drwxr-xr-x    2 root     root          4096 May 29 14:20 home
  drwxr-xr-x    7 root     root          4096 May 29 14:20 lib
  drwxr-xr-x    5 root     root          4096 May 29 14:20 media
  drwxr-xr-x    2 root     root          4096 May 29 14:20 mnt
  drwxr-xr-x    2 root     root          4096 May 29 14:20 opt
  dr-xr-xr-x  123 root     root             0 Sep 15 01:14 proc
  drwx------    2 root     root          4096 May 29 14:20 root
  drwxr-xr-x    2 root     root          4096 May 29 14:20 run
  drwxr-xr-x    2 root     root          4096 May 29 14:20 sbin
  drwxr-xr-x    2 root     root          4096 May 29 14:20 srv
  dr-xr-xr-x   13 root     root             0 Sep 15 01:14 sys
  drwxrwxrwt    2 root     root          4096 May 29 14:20 tmp
  drwxr-xr-x    7 root     root          4096 May 29 14:20 usr
  drwxr-xr-x   12 root     root          4096 May 29 14:20 var
  ```

  

### 출력 형식 지정(Formatting )

- https://docs.docker.com/engine/reference/commandline/ps/

- vagrant@xenial64:~/chap02$ `docker container ls -a --format "table {{.ID}}: {{.Command}}\t{{.Names}}"`

  ```
  CONTAINER ID: COMMAND                  NAMES
  e0617160297f: "go run /echo/main.go"   cranky_mccarthy
  096e47f0844f: "go run /echo/main.go"   competent_cray
  be8ad5817ff0: "ls -al"                 tender_sutherland
  e3d0c031162f: "uname -a"               laughing_pascal
  46c40a29a225: "/bin/sh"                zen_kirch
  ```



### 컨테이너 내부의 표준 출력을 호스트로 연결

- vagrant@xenial64:~/chap02$ `docker container run -d -p 8080:8080 -p 5000:5000 jenkins`

- vagrant@xenial64:~/chap02$ `docker container ls`

  ```
  CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                                                       NAMES
  ea7de1475999        jenkins             "/bin/tini -- /usr/l…"   6 seconds ago       Up 6 seconds        0.0.0.0:5000->5000/tcp, 0.0.0.0:8080->8080/tcp, 50000/tcp   serene_easley
  ```

- vagrant@xenial64:~/chap02$ `docker container logs -f ea7de1475999`

  ```
  Running from: /usr/share/jenkins/jenkins.war
  webroot: EnvVars.masterEnvVars.get("JENKINS_HOME")
  Sep 15, 2020 2:28:47 AM Main deleteWinstoneTempContents
  WARNING: Failed to delete the temporary Winstone file /tmp/winstone/jenkins.war
  Sep 15, 2020 2:28:47 AM org.eclipse.jetty.util.log.JavaUtilLog info
  INFO: Logging initialized @617ms
  Sep 15, 2020 2:28:47 AM winstone.Logger logInternal
  INFO: Beginning extraction from war file
  Sep 15, 2020 2:28:48 AM org.eclipse.jetty.util.log.JavaUtilLog warn
  WARNING: Empty contextPath
  Sep 15, 2020 2:28:48 AM org.eclipse.jetty.util.log.JavaUtilLog info
  INFO: jetty-9.2.z-SNAPSHOT
  Sep 15, 2020 2:28:49 AM org.eclipse.jetty.util.log.JavaUtilLog info
  INFO: NO JSP Support for /, did not find org.eclipse.jetty.jsp.JettyJspServlet
  Jenkins home directory: /var/jenkins_home found at: EnvVars.masterEnvVars.get("JENKINS_HOME")
  ...
  ```

  

### 실행중인 컨테이너 내부로 명령을 전달

- `docker container exec [CONTAINER NAME] [명령어]`

  vagrant@xenial64:~/chap02$ `docker container run -t -d --name echo --rm exam/echo`

  ```
  1cb8a73c694271783766d710f32a722664916928d3e06ee699ed975ab9e88fd0
  ```

- vagrant@xenial64:~/chap02$ `docker container exec echo pwd`

  ```
  /go
  ```

- vagrant@xenial64:~/chap02$ `docker container exec echo ip a`

  ```
  1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1
      link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
      inet 127.0.0.1/8 scope host lo
         valid_lft forever preferred_lft forever
  16: eth0@if17: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default
      link/ether 02:42:ac:11:00:03 brd ff:ff:ff:ff:ff:ff link-netnsid 0
      inet 172.17.0.3/16 brd 172.17.255.255 scope global eth0
         valid_lft forever preferred_lft forever
  ```

- vagrant@xenial64:~/chap02$ `docker container exec -it echo /bin/sh`

  - 쉘 실행
  - `-it` 옵션과 같이 쓰임



### 호스트의파일 또는 디렉토리를 컨테이너 내부로 복사

- vagrant@xenial64:~/chap02$ `date > host_now`

- vagrant@xenial64:~/chap02$ `ls`

  ```
  Dockerfile  host_now  main.go
  ```

- vagrant@xenial64:~/chap02$ `docker container cp ./host_now echo:/tmp/`

- vagrant@xenial64:~/chap02$ `docker container exec echo cat /tmp/host_now`

  ```
  Tue Sep 15 02:39:13 UTC 2020
  ```



### 컨테이너 내부의 파일을 호스트로 복사

- `docker container cp [컨테이너이름]:[컨테이너내부경로] [호스트경로]`

- vagrant@xenial64:~/chap02$ `docker container cp echo:/tmp/host_now ./host_now_feom_container`

- vagrant@xenial64:~/chap02$ `cat host_now_feom_container`

  ```
  Tue Sep 15 02:39:13 UTC 2020
  ```



### 사용하지 않는 컨테이너, 이미지 삭제

- vagrant@xenial64:~/chap02$ `docker container prune -f`
- vagrant@xenial64:~/chap02$ `docker image prune -f`



### 컨테이너 단위 시스템 리소스 사용 현황 확인

- vagrant@xenial64:~/chap02$ `docker container stats`

  ```
  CONTAINER ID        NAME                CPU %               MEM USAGE / LIMIT     MEM %               NET I/O             BLOCK I/O           PIDS
  1cb8a73c6942        echo                0.00%               5.016MiB / 991.9MiB   0.51%               912B / 0B           43.3MB / 8.19kB     12
  ea7de1475999        serene_easley       0.10%               226.6MiB / 991.9MiB   22.84%              4.41MB / 25.4kB     45.6MB / 5.37MB     32
  ```

  

## 실습) 특정 웹페이지를 포함하고 있는 웹서버 이미지를 생성

#### 방법1

- 우분투 이미지를 이용해서 컨테이너를 실행
- 컨테이너 내부를 변경 후 이미지를 생성



1. **작업 디렉토리 생성**

   - vagrant@xenial64:~$ `mkdir webserver && cd webserver`

2. **웹페이지 "Hello.html"생성**

   - vagrant@xenial64:~/webserver$ `echo "Hello Docker" > hello.html`

   - vagrant@xenial64:~/webserver$ `cat hello.html`

     ```
     Hello Docker
     ```

3. **우분투 이미지를 이용해서 컨테이너를 실행**

   - vagrant@xenial64:~/webserver$ `docker container run -dit -p 8080:80 --name myweb ubuntu:14.04 /bin/bash`

     ```
     61f2b695a1208c3a2c6cbdb9ab9ea90639a4454779db40ee32416ac29d5ff44f
     ```

   - vagrant@xenial64:~/webserver$ `docker container ps`

     ```
     CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                  NAMES
     61f2b695a120        ubuntu:14.04        "/bin/bash"         3 seconds ago       Up 1 second         0.0.0.0:8080->80/tcp   myweb
     ```

4. **컨테이너 내부의 shell로 접속**

   - vagrant@xenial64:~/webserver$ `docker container exec -it myweb /bin/bash`

     또는

   - vagrant@xenial64:~/webserver$ `docker container attach myweb`
     - attach 명령어로 접속 후 빠져 나왔을 때는 컨테이너 종료 
     - 다시 사용할 때는 `docker container restart myweb` 사용하여 다시 시작

5. **Apache 웹서버 설치**

   - root@d388cc4ca93d:/# `apt-get update`

   - root@d388cc4ca93d:/# `apt-get install apache2 -y`

   - root@d388cc4ca93d:/# `service apache2 status`

     ```
      * apache2 is not running
     ```

   - root@61f2b695a120:/# `service apache2 start`
     ```
     * Starting web server apache2                                                                                                                                        AH00558: apache2: Could not reliably determine the server's fully qualified domain name, using 172.17.0.2. Set the 'ServerName' directive globally to suppress this message
     *
     ```

   - root@61f2b695a120:/# `service apache2 status`

     ```
      * apache2 is running
     ```

   - root@61f2b695a120:/# `ls /var/www/html/`

     ```
     index.html
     ```

   - root@d388cc4ca93d:/# `exit`

6. **아파치 웹서버의 웹루트에 hello.html 복사**

   - vagrant@xenial64:~/webserver$ `ls`

     ```
     hello.html
     ```

   - vagrant@xenial64:~/webserver$ `docker container cp ./hello.html  myweb:/var/www/html/`

   - vagrant@xenial64:~/webserver$ `docker container exec myweb cat /var/www/html/hello.html`

     ```
     Hello Docker
     ```

7. **컨테이너를 웹 서비스를 요청**

   - vagrant@xenial64:~/webserver$ `curl http://localhost:8080/hello.html'

     ```
     Hello Docker
     ```

8. **이미지 생성**

   - vagrant@xenial64:~/webserver$ `docker commit myweb uchan0/myweb`

     ```
     sha256:27b38892faa06b7a00d47eb3bdc79307cc60fe4aea97246460e61bd045e477ac
     ```

   - vagrant@xenial64:~/webserver$ `docker image ls`

     ```
     REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
     uchan0/myweb        latest              27b38892faa0        48 seconds ago      221MB
     ```

     

#### 방법2

- Dockerfile을 작성



1. **Dockerfile 정의**

   - vagrant@xenial64:~/webserver$ `vim Dockerfile`

     ```dockerfile
     FROM ubuntu:14.04
     
     RUN apt-get update
     RUN apt-get install -y apache2
     
     ADD hello.html /var/www/html
     
     EXPOSE 80
     
     CMD apachectl -DFOREGROUND
     ```

     

2. **Dockerfile을 빌드해서 이미지 생성**

   - vagrant@xenial64:~/webserver$ `docker image build -t uchan0/myweb:dockerfile .`

   - vagrant@xenial64:~/webserver$ `docker image ls`

     ```
     1. - REPOSITORY          TAG                 IMAGE ID            CREATED              SIZE
          uchan0/myweb        dockerfile          e423400dec9e        About a minute ago   221MB
          uchan0/myweb        latest              27b38892faa0        11 minutes ago       221MB
     ```



3. **생성한 이미지로 컨테이너를 실행**

   - vagrant@xenial64:~/webserver$ `docker container run -d -p 9090:80 --name mywebdockerfile uchan0/myweb:dockerfile`

     ```
     0fe917fdc995d21ba43b1d14ac6a55f132c19246d4a961e7b6372a4cc5126715
     ```

   - vagrant@xenial64:~/webserver$ `curl http://localhost:9090/hello.html`

     ```
     Hello Docker
     ```



#### 참고

- vagrant@xenial64:~/webserver$ `docker container run -d -P --name mywebrandport uchan0/myweb:dockerfile`

  - **`-P` 옵션을 사용하면 호스트의 랜덤하게 할당된 포트와 컨테이너에서 EXPOSE된 포트를 자동으로 맵핑**

  - 랜덤으로 할당된 포트 확인 하는법

    - vagrant@xenial64:~/webserver$ `docker image ls`

      또는

    - vagrant@xenial64:~/webserver$ `docker port mywebrandport`



### 만들어진 이미지를 이용해서 웹서버를 구축

1. **도커 허브에서 적당한 이미지를 검색**

   - https://hub.docker.com/_/nginx

   

2.  **nginx 이미지를 다운로드**

   - vagrant@xenial64:~/webserver$ `docker pull nginx`

   - vagrant@xenial64:~/webserver$ `docker image ls`

     ```
     vagrant@xenial64:~/webserver$ docker image ls
     REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
     uchan0/myweb        dockerfile          e423400dec9e        40 minutes ago      221MB
     uchan0/myweb        latest              27b38892faa0        About an hour ago   221MB
     nginx               latest              7e4d58f0e5f3        4 days ago          133MB
     ```

   

3.  **nginx 서버를 구동 (컨테이너를 생성)**

   - vagrant@xenial64:~/webserver$ `docker container run --name webserver -d -p 80:80 nginx`

     ```
     b3af8d027c47c90d42d7edc4a4de7144360e4cf76f552195e378febf47f7072e
     ```

   - vagrant@xenial64:~/webserver$ `curl http://localhost`

     ```html
     <!DOCTYPE html>
     <html>
     <head>
     <title>Welcome to nginx!</title>
     <style>
         body {
             width: 35em;
             margin: 0 auto;
             font-family: Tahoma, Verdana, Arial, sans-serif;
         }
     </style>
     </head>
     <body>
     <h1>Welcome to nginx!</h1>
     <p>If you see this page, the nginx web server is successfully installed and
     working. Further configuration is required.</p>
     
     <p>For online documentation and support please refer to
     <a href="http://nginx.org/">nginx.org</a>.<br/>
     Commercial support is available at
     <a href="http://nginx.com/">nginx.com</a>.</p>
     
     <p><em>Thank you for using nginx.</em></p>
     </body>
     </html>
     ```



### Wordpress와 MySQL을 연동한 Wordpress기반 블로그 서비스

1. 작업 디렉토리 생성

   - vagrant@xenial64:~/webserver$ `mkdir ~/blog && cd ~/blog`
   - vagrant@xenial64:~/blog$

   

2.  **MySQL 서비스를 제공하는 컨테이너를 실행**

   - 도커 허브를 검색
     - https://hub.docker.com/_/mysql
   - vagrant@xenial64:~/blog$ `docker run -d --name wordpressdb -e MYSQL_ROOT_PASSWORD=password -e MYSQL_DATABASE=wordpress mysql:5.7 `
     - **`-e MYSQL_ROOT_PASSWORD=password -e MYSQL_DATABASE=wordpress`: 컨테이너 내부의 환경 변수를 설정**

   

3. **Wordpress 이미지를 이용한 웹서버 컨테이너를 실행**

   - 도커 허브를 검색

     - https://hub.docker.com/_/wordpress

   - vagrant@xenial64:~/blog$ `docker run -d -e WORDPRESS_DB_PASSWORD=password --name wordpress --link wordpressdb:mysql -p 80 wordpress`

   - vagrant@xenial64:~/blog$ `docker container ls`

     ```
     CONTAINER ID        IMAGE                     COMMAND                  CREATED              STATUS              PORTS                   NAMES
     1551d0b3454d        wordpress                 "docker-entrypoint.s…"   About a minute ago   Up 59 seconds       0.0.0.0:32768->80/tcp   wordpress
     ea943298654d        mysql:5.7                 "docker-entrypoint.s…"   7 minutes ago        Up 7 minutes        3306/tcp, 33060/tcp     wordpressdb
     b3af8d027c47        nginx                     "/docker-entrypoint.…"   11 minutes ago       Up 11 minutes       0.0.0.0:80->80/tcp      webserver
     0fe917fdc995        uchan0/myweb:dockerfile   "/bin/sh -c 'apachec…"   About an hour ago    Up About an hour    0.0.0.0:9090->80/tcp    mywebdockerfile
     61f2b695a120        ubuntu:14.04              "/bin/bash"              About an hour ago    Up About an hour    0.0.0.0:8080->80/tcp    myweb
     ```

     

### 컨테이너의 데이터를 영속적(Persistent)인 데이터로 활용하는 방법

#### 방법 1. 호스트 볼륨 공유

- `-v` 옵션을 이용해서 호스트 볼륨을 공유
- 호스트의 디렉터리를 컨테이너의 디렉터리에 마운트
- 이미지에 원재 존재하는 디렉터리에 호스트의 볼륨을 공유하면 컨테이너의 디렉터리 자체가 덮어쓰게 됨



0. **모든 컨테이너, 이미지, 볼륨을 삭제**
   - vagrant@xenial64:~/blog$ `docker container rm -f $(docker container ls -aq)`
   - vagrant@xenial64:~/blog$ `docker image rm -f $(docker image ls -aq)`
   - vagrant@xenial64:~/blog$ `docker volume rm -f $(docker volume ls -q)`

1.  **MySQL 이미지를 이용한 데이터베이스 컨테이너를 생성**

   - vagrant@xenial64:~/blog$ `docker run -d --name wordpressdb_hostvolume -e MYSQL_ROOT_PASSWORD=password -e MYSQL_DATABASE=wordpress -v /home/wordpress_db:/var/lib/mysql mysql:5.7`

     ```
     4de0f6b077349789396a82cb4d6620f688e16407068bc7f787b750b982c646a5
     ```

     - /home/wordpress_db: 도커가 자동으로 생성해줌
     - /var/lib/mysql: mysql 데이터베이스의 데이터를 저장하는 기본 디렉터리

   

2. **워드프레스 이미지를 이용해 웹 서버 컨테이너를 생성**

   - vagrant@xenial64:~$ `docker run -d -e WORDPRESS_DB_PASSWORD-password --name wordpress_hostvolume --link wordpressdb_hostvolume:mysql -p 80 wordpress`

     ```
     6a4c95026f622df4e711c4de47700eafa91bd553d993b3d9fb2d6cc99a499120
     ```

   

3. **호스트 볼륨 공유를 확인**

   - vagrant@xenial64:~/blog$ `ls /home/wordpress_db`

     ```
     auto.cnf    ca.pem           client-key.pem  ibdata1      ib_logfile1  private_key.pem  server-cert.pem
     ca-key.pem  client-cert.pem  ib_buffer_pool  ib_logfile0  mysql        public_key.pem   server-key.pem
     ```

   

4.  **컨테이너 내부의 디렉터리를 확인**

   - vagrant@xenial64:~$ `docker container exec wordpressdb_hostvolume ls /var/lib/mysql`

     ```
     auto.cnf
     ca-key.pem
     ca.pem
     client-cert.pem
     client-key.pem
     ib_buffer_pool
     ib_logfile0
     ib_logfile1
     ibdata1
     ibtmp1
     mysql
     performance_schema
     private_key.pem
     public_key.pem
     server-cert.pem
     server-key.pem
     sys
     wordpress
     ```

   - 3에서 확인한 호스트 볼륨과 컨테이너 내부에 디렉토리가 같음(공유되고 있음)

   

5. **wordpressdb_hostvolume 컨테이너를 삭제한 후 호스트 볼륨을 확인**

   - vagrant@xenial64:~$ `docker container rm -f wordpress_hostvolume`

   - vagrant@xenial64:~$ `ls /home/wordpress_db/`

     ```
     auto.cnf    ca.pem           client-key.pem  ibdata1      ib_logfile1  mysql               private_key.pem  server-cert.pem  sys
     ca-key.pem  client-cert.pem  ib_buffer_pool  ib_logfile0  ibtmp1       performance_schema  public_key.pem   server-key.pem   wordpress
     ```

     - 컨테이너는 삭제 되었지만 공유되어있던 볼륨은 그래도 남아있음
       - 데이터의 영속성을 부여

   

6. MySQL 이미지를 이용해서 컨테이너를 실행(기존 호스트 볼륨을 맵핑)



#### 방법2. 볼륨 컨테이너

- `-v` 옵션으로 볼륨을 사용하는 컨테이너를 다른 컨테이너와 공유하는 것컨테이너를 생성할 때 `--volumes-from` 옵션을 설정하면 `-v` 또는` --volume` 옵션을 적용한 컨테이너의 볼륨 디렉터리 공유가 가능



#### 방법3. 도커 볼륨

- 도커 자체가 제공하는 볼륨 기능을 활용
- `docker volume` 명령어를 사용

