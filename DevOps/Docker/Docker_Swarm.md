# Docker Swarm

#### 1. 모든 가상머신 중지



#### 2. 아래 3개 폴더에 vagrant파일 추가

- C:\HasiCorp\swarm-manager
- C:\HasiCorp\swarm-worker1
- C:\HasiCorp\swarm-worker2



#### 3. 아래 3개 폴더 아래에 Vagrantfile을 생성

- C:\HasiCorp\swarm-manager\Vagrantfile

```
# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
# config.vm.box = "centos/7"
  config.vm.box = "generic/centos7"
  config.vm.hostname = "swarm-manager"
  config.vm.network "private_network", ip: "192.168.111.100"
  config.vm.synced_folder ".", "/home/vagrant/sync", disabled: true
end
```

- C:\HasiCorp\swarm-worker1\Vagrantfile

```
# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
# config.vm.box = "centos/7"
  config.vm.box = "generic/centos7"
  config.vm.hostname = "swarm-worker1"
  config.vm.network "private_network", ip: "192.168.111.101"
  config.vm.synced_folder ".", "/home/vagrant/sync", disabled: true
end
```

- C:\HasiCorp\swarm-worker2\Vagrantfile

```
# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
# config.vm.box = "centos/7"
  config.vm.box = "generic/centos7"
  config.vm.hostname = "swarm-worker2"
  config.vm.network "private_network", ip: "192.168.111.102"
  config.vm.synced_folder ".", "/home/vagrant/sync", disabled: true
end
```



#### 4. 각 폴더에서 vagrant up 명령으로 가상머신을 생성

C:\HashiCorp\swarm> vagrant up





##### 도커 스웜 모드의 구조

- 매니저(manager) 노드와 워커(worker) 노드로 구성
- 워커 노드 ⇒ 실제 컨테이너가 생성되고 관리되는 도커 서버
- 매니저 노드 ⇒ 워커 노드를 관리하기 위한 도커 서버
- 매니저 노드는 워커 노드의 역할을 포함
- 클러스터를 구성하기 위해서는 최소 1개 이상의 매니저 노드가 존재해야 함



#### 5.  3에서 생성한 가상머신에 SSH로 접속 후 Docker 설치

C:\HashiCorp\swarm> vagrant ssh

[vagrant@swarm ~]$ sudo su

[root@swarm vagrant]# cd

[root@swarm ~]# yum install -y docker

[root@swarm ~]# systemctl start docker.service

[root@swarm ~]# docker version



#### 6. 매니저 역할의 서버에서 스웜 클러스터를 시작

- [root@swarm-manager ~]# docker swarm init --advertise-addr 192.168.111.100
  - `192.168.111.100`: manage노드의 IP

```
To add a worker to this swarm, run the following command:

    docker swarm join \
    --token SWMTKN-1-4m5j55p4v9p7nf30q9dmxtao6hlswkt4cy6y1mn19ij44t7767-0hggdzhrx95glo1nhzxxjoooq \
    192.168.111.100:2377
#   -> 새로운 워커 노드를 클러스터에 추가할 때 사용하는 비밀키(토큰)

To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.
```



#### 7. 워커 노드를 추가

[root**@swarm-worker1** ~]#docker swarm join \
    --token SWMTKN-1-4m5j55p4v9p7nf30q9dmxtao6hlswkt4cy6y1mn19ij44t7767-0hggdzhrx95glo1nhzxxjoooq \
    192.168.111.100:2377

```
This node joined a swarm as a worker.
```

[root**@swarm-worker2** ~]# docker swarm join \
    --token SWMTKN-1-4m5j55p4v9p7nf30q9dmxtao6hlswkt4cy6y1mn19ij44t7767-0hggdzhrx95glo1nhzxxjoooq \
    192.168.111.100:2377

```
This node joined a swarm as a worker.
```



#### 8. 스웜 클러스터에 정상적으로 추가되었는지 확인

[root@swarm-manager ~]# docker node ls

```
ID                           HOSTNAME       STATUS  AVAILABILITY  MANAGER STATUS
cvsni3s27i2w56ca69nybi550    swarm-worker1  Ready   Active
jbpses19ubexiqwrfxmpm9uvb    swarm-worker2  Ready   Active
yqbh4sbsw8bvxyqb6ly33sxy6 *  swarm-manager  Ready   Active        Leader
```



#### 9. 토큰 확인 및 변경 방법

##### 토큰 확인

[root@swarm-manager ~]# docker swarm join-token manager

```
To add a manager to this swarm, run the following command:

    docker swarm join \
    --token SWMTKN-1-4m5j55p4v9p7nf30q9dmxtao6hlswkt4cy6y1mn19ij44t7767-2tocisb2l8arxufmg8sm64ku6 \
    192.168.111.100:2377
```

[root@swarm-manager ~]# docker swarm join-token worker

```
To add a worker to this swarm, run the following command:

    docker swarm join \
    --token SWMTKN-1-4m5j55p4v9p7nf30q9dmxtao6hlswkt4cy6y1mn19ij44t7767-0hggdzhrx95glo1nhzxxjoooq \
    192.168.111.100:2377
```



##### 토큰 변경

[root@swarm-manager ~]# docker swarm join-token --rotate manager

```
Successfully rotated manager join token.

To add a manager to this swarm, run the following command:

    docker swarm join \
    --token SWMTKN-1-4m5j55p4v9p7nf30q9dmxtao6hlswkt4cy6y1mn19ij44t7767-95ce3ta4qhj77b0aa53kbx611 \
    192.168.111.100:2377
```

