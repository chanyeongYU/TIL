# Docker	

#### Docker Install

[vagrant@demo html]$ `sudo su`

[root@demo html]# `cd`

[root@demo ~]# `yum install -y docker`



#### Docker Service 기동

[root@demo ~]# `systemctl start docker.service`

[root@demo ~]# `docker version`

```
Client:
 Version:         1.13.1
 API version:     1.26
 Package version: docker-1.13.1-162.git64e9980.el7.centos.x86_64
 Go version:      go1.10.3
 Git commit:      64e9980/1.13.1
 Built:           Wed Jul  1 14:56:42 2020
 OS/Arch:         linux/amd64

Server:
 Version:         1.13.1
 API version:     1.26 (minimum version 1.12)
 Package version: docker-1.13.1-162.git64e9980.el7.centos.x86_64
 Go version:      go1.10.3
 Git commit:      64e9980/1.13.1
 Built:           Wed Jul  1 14:56:42 2020
 OS/Arch:         linux/amd64
 Experimental:    false
```



#### Docker Login

[root@demo ~]# `docker login`
Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
Username: ***DOCKER_HUB_ID***
Password: ***DOCKER_HUB_PASSWORD***
Login Succeeded



#### Docker Image 검색

[root@demo ~]# `docker search centos`

- Stars가 높은 순

```
INDEX       NAME                                         DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
docker.io   docker.io/centos                             The official build of CentOS.                   6187      [OK] 
docker.io   docker.io/ansible/centos7-ansible            Ansible on Centos7                              132                  [OK]
docker.io   docker.io/consol/centos-xfce-vnc             Centos container with "headless" VNC sessi...   119                  [OK]
docker.io   docker.io/jdeathe/centos-ssh                 OpenSSH / Supervisor / EPEL/IUS/SCL Repos ...   115                  [OK]
docker.io   docker.io/centos/systemd                     systemd enabled base container.                 86                   [OK]
docker.io   docker.io/centos/mysql-57-centos7            MySQL 5.7 SQL database server                   83             
docker.io   docker.io/imagine10255/centos6-lnmp-php56    centos6-lnmp-php56                              58                   [OK]
docker.io   docker.io/tutum/centos                       Simple CentOS docker image with SSH access      47             
docker.io   docker.io/centos/postgresql-96-centos7       PostgreSQL is an advanced Object-Relationa...   46             
docker.io   docker.io/kinogmt/centos-ssh                 CentOS with SSH                                 29                   [OK]
docker.io   docker.io/pivotaldata/centos-gpdb-dev        CentOS image for GPDB development. Tag nam...   12             
docker.io   docker.io/guyton/centos6                     From official centos6 container with full ...   10                   [OK]
docker.io   docker.io/centos/tools                       Docker image that has systems administrati...   6                    [OK]
docker.io   docker.io/drecom/centos-ruby                 centos ruby                                     6                    [OK]
docker.io   docker.io/pivotaldata/centos                 Base centos, freshened up a little with a ...   5              
docker.io   docker.io/darksheer/centos                   Base Centos Image -- Updated hourly             3                    [OK]
docker.io   docker.io/mamohr/centos-java                 Oracle Java 8 Docker image based on Centos 7    3                    [OK]
docker.io   docker.io/pivotaldata/centos-gcc-toolchain   CentOS with a toolchain, but unaffiliated ...   3              
docker.io   docker.io/pivotaldata/centos-mingw           Using the mingw toolchain to cross-compile...   3              
docker.io   docker.io/blacklabelops/centos               CentOS Base Image! Built and Updates Daily!     1                    [OK]
docker.io   docker.io/indigo/centos-maven                Vanilla CentOS 7 with Oracle Java Developm...   1                    [OK]
docker.io   docker.io/mcnaughton/centos-base             centos base image                               1                    [OK]
docker.io   docker.io/pivotaldata/centos6.8-dev          CentosOS 6.8 image for GPDB development         0              
docker.io   docker.io/pivotaldata/centos7-dev            CentosOS 7 image for GPDB development           0              
docker.io   docker.io/smartentry/centos                  centos with smartentry                          0                    [OK]
```



#### Docker Image 획득 및 확인

- 이미지 획득

[root@demo ~]# `docker pull docker.io/centos`

```
Using default tag: latest		<= 별도의 태그를 지정하지 않았기 때문에
Trying to pull repository docker.io/library/centos ...
latest: Pulling from docker.io/library/centos
3c72a8ed6814: Pull complete
Digest: sha256:76d24f3ba3317fa945743bb3746fbaf3a0b752f10b10376960de01da70685fbd
Status: Downloaded newer image for docker.io/centos:latest
```

- 이미지 확인

[root@demo ~]# `docker image ls`

```
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
docker.io/centos    latest              0d120b6ccaa8        4 weeks ago         215 MB
```



##### 태그를 지정해서 Docker 이미지 획득

[root@demo ~]# `docker pull docker.io/centos:centos7`

```
Trying to pull repository docker.io/library/centos ...
centos7: Pulling from docker.io/library/centos
75f829a71a1c: Pull complete
Digest: sha256:19a79828ca2e505eaee0ff38c2f3fd9901f4826737295157cc5212b7a372cd2b
Status: Downloaded newer image for docker.io/centos:centos7
```

[root@demo ~]# `docker image ls`

```
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
docker.io/centos    centos7             7e6257c9f8d8        4 weeks ago         203 MB
docker.io/centos    latest              0d120b6ccaa8        4 weeks ago         215 MB
```



#### 컨테이너 실행 및 확인

- 컨테이너 실핼

[root@demo ~]# `docker run -t -d --name centos7 docker.io/centos:centos7`

```
3af6e61f52f2e03e6816c5373db045322ddd432fa273d35150b04cb29d1c7362
```

- 실행중인 컨테이너 확인

[root@demo ~]# `docker container ls` 

```
CONTAINER ID        IMAGE                      COMMAND             CREATED             STATUS              PORTS               NAMES
3af6e61f52f2        docker.io/centos:centos7   "/bin/bash"         10 seconds ago      Up 9 seconds                            centos7
```

또는

[root@demo ~]# `docker container ps`

```
CONTAINER ID        IMAGE                      COMMAND             CREATED             STATUS              PORTS               NAMES
3af6e61f52f2        docker.io/centos:centos7   "/bin/bash"         25 seconds ago      Up 24 seconds                           centos7
```



#### 컨테이너 내부에 명령어 실행

- 컨테이너 내부의 설치되어 있는 CentOS 버전 확인

[root@demo ~]# `docker exec centos7 cat /etc/redhat-release`

```
CentOS Linux release 7.8.2003 (Core)
```



- 실행 중인 컨테이너 내부로 진입

[root@demo ~]# `docker exec -it centos7 /bin/bash`

​													-i: interaction

​													-t: 편집 가능



[root@3af6e61f52f2 /]# `cat /etc/redhat-release`

```
CentOS Linux release 7.8.2003 (Core)
[root@3af6e61f52f2 /]# `exit`
```



#### Ubuntu 설치

[root@demo ~]# `docker search ubuntu`

```
INDEX       NAME                                                                DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
docker.io   docker.io/ubuntu                                                    Ubuntu is a Debian-based Linux operating s...   11301     [OK]
docker.io   docker.io/dorowu/ubuntu-desktop-lxde-vnc                            Docker image to provide HTML5 VNC interfac...   460                  [OK]
docker.io   docker.io/rastasheep/ubuntu-sshd                                    Dockerized SSH service, built on top of of...   247                  [OK]
docker.io   docker.io/consol/ubuntu-xfce-vnc                                    Ubuntu container with "headless" VNC sessi...   225                  [OK]
docker.io   docker.io/ubuntu-upstart                                            Upstart is an event-based replacement for ...   110       [OK]
docker.io   docker.io/ansible/ubuntu14.04-ansible                               Ubuntu 14.04 LTS with ansible                   98                   [OK]
docker.io   docker.io/neurodebian                                               NeuroDebian provides neuroscience research...   69        [OK]
docker.io   docker.io/1and1internet/ubuntu-16-nginx-php-phpmyadmin-mysql-5      ubuntu-16-nginx-php-phpmyadmin-mysql-5          50                   [OK]
docker.io   docker.io/ubuntu-debootstrap                                        debootstrap --variant=minbase --components...   44        [OK]
docker.io   docker.io/i386/ubuntu                                               Ubuntu is a Debian-based Linux operating s...   24
docker.io   docker.io/nuagebec/ubuntu                                           Simple always updated Ubuntu docker images...   24                   [OK]
docker.io   docker.io/1and1internet/ubuntu-16-apache-php-5.6                    ubuntu-16-apache-php-5.6                        14                   [OK]
docker.io   docker.io/1and1internet/ubuntu-16-apache-php-7.0                    ubuntu-16-apache-php-7.0                        13                   [OK]
docker.io   docker.io/eclipse/ubuntu_jdk8                                       Ubuntu, JDK8, Maven 3, git, curl, nmap, mc...   13                   [OK]
docker.io   docker.io/1and1internet/ubuntu-16-nginx-php-phpmyadmin-mariadb-10   ubuntu-16-nginx-php-phpmyadmin-mariadb-10       11                   [OK]
docker.io   docker.io/1and1internet/ubuntu-16-nginx-php-5.6-wordpress-4         ubuntu-16-nginx-php-5.6-wordpress-4             7                    [OK]
docker.io   docker.io/darksheer/ubuntu                                          Base Ubuntu Image -- Updated hourly             5                    [OK]
docker.io   docker.io/1and1internet/ubuntu-16-nginx-php-7.0                     ubuntu-16-nginx-php-7.0                         4                    [OK]
docker.io   docker.io/pivotaldata/ubuntu                                        A quick freshening-up of the base Ubuntu d...   4
docker.io   docker.io/pivotaldata/ubuntu16.04-build                             Ubuntu 16.04 image for GPDB compilation         2
docker.io   docker.io/1and1internet/ubuntu-16-php-7.1                           ubuntu-16-php-7.1                               1                    [OK]
docker.io   docker.io/1and1internet/ubuntu-16-sshd                              ubuntu-16-sshd                                  1                    [OK]
docker.io   docker.io/pivotaldata/ubuntu-gpdb-dev                               Ubuntu images for GPDB development              1
docker.io   docker.io/smartentry/ubuntu                                         ubuntu with smartentry                          1                    [OK]
docker.io   docker.io/pivotaldata/ubuntu16.04-test                              Ubuntu 16.04 image for GPDB testing             0
```

[root@demo ~]# `docker pull docker.io/ubuntu`

```
Using default tag: latest
Trying to pull repository docker.io/library/ubuntu ...
latest: Pulling from docker.io/library/ubuntu	⇐ 로컬 레포지터리에 이미지가 존재하지 않기 때문에 이미지를 가져와서 실행
54ee1f796a1e: Pull complete
f7bfea53ad12: Pull complete
46d371e02073: Pull complete
b66c17bbf772: Pull complete
Digest: sha256:31dfb10d52ce76c5ca0aa19d10b3e6424b830729e32a89a7c6eee2cda2be67a5
Status: Downloaded newer image for docker.io/ubuntu:latest
```

[root@demo ~]# `docker image ls`

```
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
docker.io/ubuntu    latest              4e2eef94cd6b        3 weeks ago         73.9 MB
docker.io/centos    centos7             7e6257c9f8d8        4 weeks ago         203 MB
docker.io/centos    latest              0d120b6ccaa8        4 weeks ago         215 MB
```

[root@demo ~]# `docker run --name ubuntu -itd docker.io/ubuntu`

```
01df9b60c8fc861945dd8d449683c83c20ea590d765c4d1f12550b9b1bbb3d10
```

[root@demo ~]# `docker container ls`

```
CONTAINER ID        IMAGE                      COMMAND             CREATED              STATUS              PORTS               NAMES
01df9b60c8fc        docker.io/ubuntu           "/bin/bash"         About a minute ago   Up 59 seconds                           ubuntu
3af6e61f52f2        docker.io/centos:centos7   "/bin/bash"         15 minutes ago       Up 15 minutes                           centos7
```

[root@demo ~]# `docker exec -it ubuntu /bin/bash`

- 여기서부터 ubuntu

root@01df9b60c8fc:/# `cat /etc/issue`

```
Ubuntu 20.04.1 LTS \n \l
```



#### 컨테이너 정지 재기동

- 컨테이너 정지

[root@demo ~]# `docker container stop centos7`

```
centos7
```

[root@demo ~]# `docker container ls`

```
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
01df9b60c8fc        docker.io/ubuntu    "/bin/bash"         22 minutes ago      Up 22 minutes                           ubuntu
```

[root@demo ~]# `docker container ls -a`		<= 정지 상태의 컨테이너를 포함해서 조회

```
CONTAINER ID        IMAGE                      COMMAND             CREATED             STATUS                       PORTS               NAMES
01df9b60c8fc        docker.io/ubuntu           "/bin/bash"         22 minutes ago      Up 22 minutes                                    ubuntu
3af6e61f52f2        docker.io/centos:centos7   "/bin/bash"         37 minutes ago      Exited (137) 7 seconds ago                       centos7
```



- 컨테이너 재기동

[root@demo ~]# `docker container start centos7`

```
centos7
```

[root@demo ~]# `docker container ls`

```
CONTAINER ID        IMAGE                      COMMAND             CREATED             STATUS              PORTS               NAMES
01df9b60c8fc        docker.io/ubuntu           "/bin/bash"         24 minutes ago      Up 24 minutes                           ubuntu
3af6e61f52f2        docker.io/centos:centos7   "/bin/bash"         38 minutes ago      Up 5 seconds                            centos7
```



#### 컨테이너 삭제

[root@demo ~]# `docker container rm -f centos7`

```
centos7
```

[root@demo ~]# `docker container ls -a`

```
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
01df9b60c8fc        docker.io/ubuntu    "/bin/bash"         25 minutes ago      Up 25 minutes                           ubuntu
```

[root@demo ~]# `docker image ls`		<= 도커 이미지까지 지워지지는 않음

```
REPOSITORY     TAG         IMAGE ID      CREATED       SIZEdocker.io/ubuntu  latest       4e2eef94cd6b    3 weeks ago     73.9 MBdocker.io/centos  centos7       7e6257c9f8d8    4 weeks ago     203 MBdocker.io/centos  latest       0d120b6ccaa8    4 weeks ago     215 MB
```



#### **nginx 컨테이너 기동**

[root@demo ~]# `docker pull nginx`		<= 공식 배포 이미지는 docker.io 생략가능

```
Using default tag: latest
Trying to pull repository docker.io/library/nginx ...
latest: Pulling from docker.io/library/nginx
d121f8d1c412: Pull complete
ebd81fc8c071: Pull complete
655316c160af: Pull complete
d15953c0e0f8: Pull complete
2ee525c5c3cc: Pull complete
Digest: sha256:9a1f8ed9e2273e8b3bbcd2e200024adac624c2e5c9b1d420988809f5c0c41a5e
Status: Downloaded newer image for docker.io/nginx:latest
```

[root@demo ~]# `docker container run -d -p 8000:80 --name nginx-latest nginx`

​																				**-p** 옵션은 포트 포워딩 8000 -> 80

```
cff45200ab3e5e2e1347bd33e68f8a5f481fed41d158552bf723d1b11ff61cf8	<= 컨테이너 ID
```

[root@demo ~]# `docker container ls`

```
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTS                  NAMES
cff45200ab3e        nginx               "/docker-entrypoin..."   About a minute ago   Up About a minute   0.0.0.0:8000->80/tcp   nginx-latest
01df9b60c8fc        docker.io/ubuntu    "/bin/bash"              34 minutes ago       Up 34 minutes                              ubuntu
```

[root@demo ~]# curl http://localhost:8000

​						**localhost**: Vagrant로 생성한 CentOS,

​						**:8000**: nginx container의 80번 포트포워딩 

```
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



#### 컨테이너 표준 출력 로그 확인

[root@demo ~]# `docker container logs -f nginx-latest`

```
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d//docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh10-listen-on-ipv6-by-default.sh: Getting the checksum of /etc/nginx/conf.d/default.conf10-listen-on-ipv6-by-default.sh: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh/docker-entrypoint.sh: Configuration complete; ready for start up172.17.0.1 - - [11/Sep/2020:01:38:31 +0000] "GET / HTTP/1.1" 200 612 "-" "curl/7.29.0" "-"192.168.33.1 - - [11/Sep/2020:01:43:36 +0000] "GET / HTTP/1.1" 200 612 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/85.0.4183.83 Safari/537.36" "-"192.168.33.1 - - [11/Sep/2020:01:43:36 +0000] "GET /favicon.ico HTTP/1.1" 404 555 "http://192.168.33.10:8000/" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/85.0.4183.83 Safari/537.36" "-"2020/09/11 01:43:36 [error] 28#28: *2 open() "/usr/share/nginx/html/favicon.ico" failed (2: No such file or directory), client: 192.168.33.1, server: localhost, request: "GET /favicon.ico HTTP/1.1", host: "192.168.33.10:8000", referrer: "http://192.168.33.10:8000/"
```



#### CentOS Dockerfile

https://github.com/CentOS/sig-cloud-instance-images/blob/f2788ce41161326a18420913b0195d1c6cfa1581/docker/Dockerfile

```
FROM scratch					⇐ 베이스 이미지를 지정
ADD centos-7-x86_64-docker.tar.xz /	<= 이미지파일 추가, ADD압축파일을 풀어서 배치, COPY:단순 복사
LABEL \  	<=컨테이너 관련된 정보
	org.label-schema.schema-version="1.0" \ 
	org.label-schema.name="CentOS Base Image" \  
	org.label-schema.vendor="CentOS" \  
	org.label-schema.license="GPLv2" \  
	org.label-schema.build-date="20200809" \  					 
	org.opencontainers.image.title="CentOS Base Image" \ 
	org.opencontainers.image.vendor="CentOS" \  
	org.opencontainers.image.licenses="GPL-2.0-only" \  
	org.opencontainers.image.created="2020-08-09 00:00:00+01:00"
CMD ["/bin/bash"]	<= 컨테이너가 기동될 떄 실행할 default 프로세스를 지정
```



#### Nginx Dockerfile

https://github.com/nginxinc/docker-nginx/blob/9774b522d4661effea57a1fbf64c883e699ac3ec/mainline/buster/Dockerfile

```
FROM debian:buster-slim		<= 베이스 이미지 지정

LABEL maintainer="NGINX Docker Maintainers <docker-maint@nginx.com>"

ENV NGINX_VERSION   1.19.2
ENV NJS_VERSION     0.4.3
ENV PKG_RELEASE     1~buster

RUN set -x \
# create nginx user/group first, to be consistent throughout docker variants
    && addgroup --system --gid 101 nginx \
    && adduser --system --disabled-login --ingroup nginx --no-create-home --home /nonexistent --gecos "nginx user" --shell /bin/false --uid 101 nginx \
    && apt-get update \
    && apt-get install --no-install-recommends --no-install-suggests -y gnupg1 ca-certificates \
    && \
    NGINX_GPGKEY=573BFD6B3D8FBC641079A6ABABF5BD827BD9BF62; \
    found=''; \
    for server in \
        ha.pool.sks-keyservers.net \
        hkp://keyserver.ubuntu.com:80 \
        hkp://p80.pool.sks-keyservers.net:80 \
        pgp.mit.edu \
    ; do \
        echo "Fetching GPG key $NGINX_GPGKEY from $server"; \
        apt-key adv --keyserver "$server" --keyserver-options timeout=10 --recv-keys "$NGINX_GPGKEY" && found=yes && break; \
    done; \
    test -z "$found" && echo >&2 "error: failed to fetch GPG key $NGINX_GPGKEY" && exit 1; \
    apt-get remove --purge --auto-remove -y gnupg1 && rm -rf /var/lib/apt/lists/* \
    && dpkgArch="$(dpkg --print-architecture)" \
    && nginxPackages=" \
        nginx=${NGINX_VERSION}-${PKG_RELEASE} \
        nginx-module-xslt=${NGINX_VERSION}-${PKG_RELEASE} \
        nginx-module-geoip=${NGINX_VERSION}-${PKG_RELEASE} \
        nginx-module-image-filter=${NGINX_VERSION}-${PKG_RELEASE} \
        nginx-module-njs=${NGINX_VERSION}.${NJS_VERSION}-${PKG_RELEASE} \
    " \
    && case "$dpkgArch" in \
        amd64|i386) \
# arches officialy built by upstream
            echo "deb https://nginx.org/packages/mainline/debian/ buster nginx" >> /etc/apt/sources.list.d/nginx.list \
            && apt-get update \
            ;; \
        *) \
# we're on an architecture upstream doesn't officially build for
# let's build binaries from the published source packages
            echo "deb-src https://nginx.org/packages/mainline/debian/ buster nginx" >> /etc/apt/sources.list.d/nginx.list \
            \
# new directory for storing sources and .deb files
            && tempDir="$(mktemp -d)" \
            && chmod 777 "$tempDir" \
# (777 to ensure APT's "_apt" user can access it too)
            \
# save list of currently-installed packages so build dependencies can be cleanly removed later
            && savedAptMark="$(apt-mark showmanual)" \
            \
# build .deb files from upstream's source packages (which are verified by apt-get)
            && apt-get update \
            && apt-get build-dep -y $nginxPackages \
            && ( \
                cd "$tempDir" \
                && DEB_BUILD_OPTIONS="nocheck parallel=$(nproc)" \
                    apt-get source --compile $nginxPackages \
            ) \
# we don't remove APT lists here because they get re-downloaded and removed later
            \
# reset apt-mark's "manual" list so that "purge --auto-remove" will remove all build dependencies
# (which is done after we install the built packages so we don't have to redownload any overlapping dependencies)
            && apt-mark showmanual | xargs apt-mark auto > /dev/null \
            && { [ -z "$savedAptMark" ] || apt-mark manual $savedAptMark; } \
            \
# create a temporary local APT repo to install from (so that dependency resolution can be handled by APT, as it should be)
            && ls -lAFh "$tempDir" \
            && ( cd "$tempDir" && dpkg-scanpackages . > Packages ) \
            && grep '^Package: ' "$tempDir/Packages" \
            && echo "deb [ trusted=yes ] file://$tempDir ./" > /etc/apt/sources.list.d/temp.list \
# work around the following APT issue by using "Acquire::GzipIndexes=false" (overriding "/etc/apt/apt.conf.d/docker-gzip-indexes")
#   Could not open file /var/lib/apt/lists/partial/_tmp_tmp.ODWljpQfkE_._Packages - open (13: Permission denied)
#   ...
#   E: Failed to fetch store:/var/lib/apt/lists/partial/_tmp_tmp.ODWljpQfkE_._Packages  Could not open file /var/lib/apt/lists/partial/_tmp_tmp.ODWljpQfkE_._Packages - open (13: Permission denied)
            && apt-get -o Acquire::GzipIndexes=false update \
            ;; \
    esac \
    \
    && apt-get install --no-install-recommends --no-install-suggests -y \
                        $nginxPackages \
                        gettext-base \
                        curl \
    && apt-get remove --purge --auto-remove -y && rm -rf /var/lib/apt/lists/* /etc/apt/sources.list.d/nginx.list \
    \
# if we have leftovers from building, let's purge them (including extra, unnecessary build deps)
    && if [ -n "$tempDir" ]; then \
        apt-get purge -y --auto-remove \
        && rm -rf "$tempDir" /etc/apt/sources.list.d/temp.list; \
    fi \
# forward request and error logs to docker log collector
    && ln -sf /dev/stdout /var/log/nginx/access.log \
    && ln -sf /dev/stderr /var/log/nginx/error.log \
# create a docker-entrypoint.d directory
    && mkdir /docker-entrypoint.d

COPY docker-entrypoint.sh /		<= 이미지를 생성할 호스트의 파일을 컨테이너 이미지 내부로 복사
COPY 10-listen-on-ipv6-by-default.sh /docker-entrypoint.d
COPY 20-envsubst-on-templates.sh /docker-entrypoint.d
ENTRYPOINT ["/docker-entrypoint.sh"]

EXPOSE 80		<= 80포트를 사용한다는 것을 외부애 알려줌

STOPSIGNAL SIGTERM

CMD ["nginx", "-g", "daemon off;"]
```



#### Dockerfile을 이용해서 이미지를 생성

1. 컨테이너 이미지 내부로 전달할 파일을 생성

   [root@demo ~]# `echo "Hello, Docker." > hello-docker.txt`

2. Dockerfile을 생성 ⇒ 이미지 생성에 사용

   [root@demo ~]# `vi Dockerfile`

   ```dockerfile
   FROM docker.io/centos:latest        ⇐ 베이스 이미지를 지정
   ADD  hello-docker.txt /tmp          ⇐ 호스트에 있는 hello-docker.txt 파일을 컨테이너 이미지의 /tmp 아래로 복사
   # RUN  yum install -y epel-release    ⇐ 컨테이너 이미지를 만들 때 실행
   CMD  [ "/bin/bash" ]                ⇐ 컨테이너가 실행될 때 실행할 명령어
   ```

3. Dockerfile을 이용해서 이미지를 생성

   [root@demo ~]# `docker image build -t uchan0/centos:1.0 .`

   ​						**docker image build**: Dockerfile을 이용해서 이미지를 생성

   ​						 **-t myanjini/centos**:1.0: 이미지 이름 ⇒ DOCKER_HUB_ID/IMAGE_NAME:TAG_NAME

   ​						**.**:  Dockerfile 위치 ( . ⇒ 현재 위치)

   ```
   Sending build context to Docker daemon 120.3 kB
   Step 1/3 : FROM docker.io/centos:latest
    ---> 0d120b6ccaa8
   Step 2/3 : ADD hello-docker.txt /tmp
    ---> Using cache
    ---> fd8095fee188
   Step 3/3 : CMD /bin/bash
    ---> Running in dc90c611333f
    ---> 071804dd4075
   Removing intermediate container dc90c611333f
   Successfully built 071804dd4075
   ```

4. 생성한 이미지를 이용해서 컨테이너를 실행

   [root@demo ~]# `docker container run -td --name devops-book-1.0 uchan0/centos:1.0`

   ```
   12da4112f6b0964a2c9ca48c0537326b68724df64bf4e035f20d7b2d2246d91f		⇐ 컨테이너 ID
   ```

   [root@demo ~]# `docker container ls`

   ```
   CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                  NAMES
   12da4112f6b0        uchan0/centos:1.0   "/bin/bash"              33 seconds ago      Up 32 seconds                              devops-book-1.0
   cff45200ab3e        nginx               "/docker-entrypoin..."   About an hour ago   Up About an hour    0.0.0.0:8000->80/tcp   nginx-latest
   01df9b60c8fc        docker.io/ubuntu    "/bin/bash"              About an hour ago   Up About an hour                           ubuntu
   ```

   

5. 컨테이너 내부로 진입

   [root@demo ~]# `docker exec -it devops-book-1.0 /bin/bash`

   [root@aa9eab5ad4c1 /]# `cat /tmp/hello-docker.txt`

   ```
   Hello, Docker.
   ```

   [root@aa9eab5ad4c1 /]# `yum install -y epel-release`	<= Dockerfile에서 설치하지 않은 경우

   [root@aa9eab5ad4c1 /]# `rpm -qa | grep epel`

   ```
   epel-release-8-8.el8.noarch
   ```

   

6. 컨테이너 내용을 변경 후 변경된 내용을 이미지로 생성(이미지를 만드는 두번째 방법)

   [root@aa9eab5ad4c1 /]# `yum install -y nginx` 		⇐ 컨테이너 내부에 nginx를 설치

   [root@aa9eab5ad4c1 /]# `exit`

   [root@demo ~]# `docker container commit devops-book-1.0 uchan0/centos:1.1`

   ​						**docker container commit**: 컨테이너의 현재 상태를 이미지로 기록(생성)

   ​						**devops-book-1.0**: 컨테이너 이름

   ​						**uchan0/centos:1.1**: 이미지 이름

   ```
   sha256:6b8a3d4913387d4bc3935422c0e11f49e0fdde5d4a3e4c437b45ef88385616c8
   ```

   [root@demo ~]# `docker image ls`

   

7. 도커 허브에 이미지를 등록

   [root@demo ~]# `docker image push uchan0/centos:1.1`

   

8. 도커 러브에 로그인후 등록된 이미지 확인

   ​	<img src="C:\Users\chan\AppData\Roaming\Typora\typora-user-images\image-20200911132806521.png" alt="image-20200911132806521" style="zoom:80%;" />

   

9. 도커 허브에 등록된 이미지를 이용해서 컨체이너를 실행

   [root@demo ~]# `docker run --name uchan0_centos -dt -p 8888:80 uchan0/centos:1.1`

   ```
   d153ab243198a31a3e08e8578573b42a6c65afb8fbce434cdd94094d4010cf54
   ```

   [root@demo ~]# `docker container ls`

   ```
   CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                  NAMES
   d153ab243198        uchan0/centos:1.1   "/bin/bash"              7 seconds ago       Up 6 seconds        0.0.0.0:8888->80/tcp   uchan0_centos
   12da4112f6b0        uchan0/centos:1.0   "/bin/bash"              About an hour ago   Up About an hour                           devops-book-1.0
   cff45200ab3e        nginx               "/docker-entrypoin..."   2 hours ago         Up 2 hours          0.0.0.0:8000->80/tcp   nginx-latest
   01df9b60c8fc        docker.io/ubuntu    "/bin/bash"              3 hours ago         Up 3 hours                                 ubuntu
   ```

   
   
10. 모든 컨테이너를 삭제

    [root@demo ~]# `docker container rm -f $(docker container ls -a -q)`



## Docker Compose

#### Docker Composer 설치

```
 `[root@demo ~]# curl -L http://github.com/docker/compose/release/download/1.8.0/docker-compose-`uname -s`-`uname -m` > /usr/bin/docker-composs
```



#### docker-compose.yml 파일을 생성

[root@demo ~]# `vim docker-compose.yml`

```
db:
  image: docker.io/mysql               ⇒ 컨테이너 이미지
  ports:
    - "3306:3306"
  environment:
    - MYSQL_ROOT_PASSWORD=password

app:
  image: docker.io/tomcat
  ports:
    - "9090:8080"

web:
  image: docker.io/nginx
  ports:
    - "9000:80"
```

#### 컨테이너 생성 및 확인

[root@demo ~]# `docker-compose up -d`

```
Pulling app (docker.io/tomcat:latest)...
Trying to pull repository docker.io/library/tomcat ...
latest: Pulling from docker.io/library/tomcat
d6ff36c9ec48: Pull complete
c958d65b3090: Pull complete
edaf0a6b092f: Pull complete
80931cf68816: Pull complete
bf04b6bbed0c: Pull complete
8bf847804f9e: Pull complete
6bf89641a7f2: Pull complete
3e97fae54404: Pull complete
10dee6830d45: Pull complete
680b26b7a444: Pull complete
Digest: sha256:7399db46a9f852b20437d1011eeac454e211935c5fd807f027fab6d89b594b5d
Status: Downloaded newer image for docker.io/tomcat:latest
Pulling db (docker.io/mysql:latest)...
Trying to pull repository docker.io/library/mysql ...
latest: Pulling from docker.io/library/mysql
d121f8d1c412: Already exists
f3cebc0b4691: Pull complete
1862755a0b37: Pull complete
489b44f3dbb4: Pull complete
690874f836db: Pull complete
baa8be383ffb: Pull complete
55356608b4ac: Pull complete
dd35ceccb6eb: Pull complete
429b35712b19: Pull complete
162d8291095c: Pull complete
5e500ef7181b: Pull complete
af7528e958b6: Pull complete
Digest: sha256:e1bfe11693ed2052cb3b4e5fa356c65381129e87e38551c6cd6ec532ebe0e808
Status: Downloaded newer image for docker.io/mysql:latest
Creating root_web_1
Creating root_app_1
Creating root_db_1
```

[root@demo ~]# `docker container ls`

```
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTS                               NAMES
6854584948a3        docker.io/mysql     "docker-entrypoint..."   About a minute ago   Up About a minute   0.0.0.0:3306->3306/tcp, 33060/tcp   root_db_1
e13f5ef2f223        docker.io/tomcat    "catalina.sh run"        About a minute ago   Up About a minute   0.0.0.0:9090->8080/tcp              root_app_1
dddc8987faee        docker.io/nginx     "/docker-entrypoin..."   About a minute ago   Up About a minute   0.0.0.0:9000->80/tcp                root_web_1
```



#### 컨테이너 중지

[root@demo ~]# `docker-compose down`

```
Stopping root_db_1 ... done
Stopping root_app_1 ... done
Stopping root_web_1 ... done
Removing root_db_1 ... done
Removing root_app_1 ... done
Removing root_web_1 ... done
```



![image-20200911141517984](C:\Users\chan\AppData\Roaming\Typora\typora-user-images\image-20200911141517984.png)