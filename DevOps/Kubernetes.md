# Kubernetes

- 대부분의 리소스를 **오브젝트**라고 불리는 형태로 관리
- 컨테이너의 집합 **pod**
- 컨테이너의 집합을 관리하는 컨트롤러 **replica set**
- 사용자 **service account**
- 노드 **node**
- **kubectl** 또는 **YAML**파일을 정의해서 쿠버네티스 사용
- **k8s**



### K8s 구성



### 설치

- **가상머신 생성**

  - C:\kubernetes\Vagrantfile 

  ```
  # -*- mode: ruby -*-
  # vi: set ft=ruby :
  
  Vagrant.configure("2") do |config|
    config.vm.box = "ubuntu/bionic64"
    config.vm.hostname = "ubuntu"
    config.vm.network "private_network", ip: "192.168.111.110"
    config.vm.synced_folder ".", "/home/vagrant/sync", disabled: true
    config.vm.provider "virtualbox" do |vb|
      vb.cpus = 2
      vb.memory = 2048
    end
  end
  ```

  - C:\kubernetes> vagrant up
  - C:\kubernetes> vagrant ssh

- **패키지 업데이트**

  - vagrant@ubuntu:~$ sudo su
  - root@ubuntu:/home/vagrant# cd
  - root@ubuntu:~# apt update
  - root@ubuntu:~# apt upgrade

-  **도커 설치 및 설정**

  - root@ubuntu:~# apt install docker.io -y
  - root@ubuntu:~# usermod -a -G docker vagrant
  - root@ubuntu:~# service docker restart
  - root@ubuntu:~# chmod 666 /var/run/docker.sock
  - root@ubuntu:~# docker version

  ```
  Client:
   Version:           19.03.6
   API version:       1.40
   Go version:        go1.12.17
   Git commit:        369ce74a3c
   Built:             Fri Feb 28 23:45:43 2020
   OS/Arch:           linux/amd64
   Experimental:      false
  
  Server:
   Engine:
    Version:          19.03.6
    API version:      1.40 (minimum version 1.12)
    Go version:       go1.12.17
    Git commit:       369ce74a3c
    Built:            Wed Feb 19 01:06:16 2020
    OS/Arch:          linux/amd64
    Experimental:     false
   containerd:
    Version:          1.3.3-0ubuntu1~18.04.2
    GitCommit:
   runc:
    Version:          spec: 1.0.1-dev
    GitCommit:
   docker-init:
    Version:          0.18.0
    GitCommit:
  ```

- **kubectl 설치**

  - https://kubernetes.io/ko/docs/tasks/tools/install-kubectl/
  - root@ubuntu:~# apt-get update && sudo apt-get install -y apt-transport-https gnupg2
  - root@ubuntu:~# curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
  - root@ubuntu:~# echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee -a /etc/apt/sources.list.d/kubernetes.list
  - root@ubuntu:~# apt-get update
  - root@ubuntu:~# apt-get install -y kubectl
  - root@ubuntu:~# kubectl version

  ```
  Client Version: version.Info{Major:"1", Minor:"19", GitVersion:"v1.19.2", GitCommit:"f5743093fd1c663cb0cbc89748f730662345d44d", GitTreeState:"clean", BuildDate:"2020-09-16T13:41:02Z", GoVersion:"go1.15", Compiler:"gc", Platform:"linux/amd64"}
  ```

- **Minikube 설치**

  - https://kubernetes.io/ko/docs/tasks/tools/install-minikube/
  - root@ubuntu:~# curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 && chmod +x minikube
  - root@ubuntu:~# mkdir -p /usr/local/bin/
  - root@ubuntu:~# install minikube /usr/local/bin/

- **클러스터 시작**

  - vagrant@ubuntu:~$ minikube start

  ```
  😄  minikube v1.13.0 on Ubuntu 18.04 (vbox/amd64)
  ✨  Automatically selected the docker driver
  
  ⛔  Requested memory allocation (1992MB) is less than the recommended minimum 2000MB. Deployments may fail.
  
  
  🧯  The requested memory allocation of 1992MiB does not leave room for system overhead (total system  memory: 1992MiB). You may face stability issues.
  💡  Suggestion: Start minikube with less memory allocated: 'minikube start --memory=1992mb'
  
  👍  Starting control plane node minikube in cluster minikube
  🚜  Pulling base image ...
  💾  Downloading Kubernetes v1.19.0 preload ...
      > preloaded-images-k8s-v6-v1.19.0-docker-overlay2-amd64.tar.lz4: 486.28 MiB
  🔥  Creating docker container (CPUs=2, Memory=1992MB) ...
  🐳  Preparing Kubernetes v1.19.0 on Docker 19.03.8 ...
  🔎  Verifying Kubernetes components...
  🌟  Enabled addons: default-storageclass, storage-provisioner
  🏄  Done! kubectl is now configured to use "minikube" by default
  ```

  - vagrant@ubuntu:~$ minikube status

  ```
  minikube
  type: Control Plane
  host: Running
  kubelet: Running
  apiserver: Running
  kubeconfig: Configured
  ```

  - vagrant@ubuntu:~$ kubectl version

  ```
  Client Version: version.Info{Major:"1", Minor:"19", GitVersion:"v1.19.2", GitCommit:"f5743093fd1c663cb0cbc89748f730662345d44d", GitTreeState:"clean", BuildDate:"2020-09-16T13:41:02Z", GoVersion:"go1.15", Compiler:"gc", Platform:"linux/amd64"}
  Server Version: version.Info{Major:"1", Minor:"19", GitVersion:"v1.19.0", GitCommit:"e19964183377d0ec2052d1f1fa930c4d7575bd50", GitTreeState:"clean", BuildDate:"2020-08-26T14:23:04Z", GoVersion:"go1.15", Compiler:"gc", Platform:"linux/amd64"}
  ```

  - vagrant@ubuntu:~$ kubectl version --short

  ```
  Client Version: v1.19.2
  Server Version: v1.19.0
  ```

- **minikube 중지**

  - vagrant@ubuntu:~$ minikube stop

- **minikube 삭제**

  - vagrant@ubuntu:~$ minikube delete



### 기본 사용법

##### K8s에서 사용할 수 있는 오브젝트 확인

- vagrant@ubuntu:~$ kubectl api-resources

```
NAME                              SHORTNAMES   APIGROUP                       NAMESPACED   KIND
bindings                                                                      true         Binding
componentstatuses                 cs                                          false        ComponentStatus
configmaps                        cm                                          true         ConfigMap
endpoints                         ep                                          true         Endpoints
events                            ev                                          true         Event
limitranges                       limits                                      true         LimitRange
namespaces                        ns                                          false        Namespace
nodes                             no                                          false        Node
...
```



##### 특정 오브젝트에 대한 상세 설명 확인

- vagrant@ubuntu:~$ kubectl explain pod

```
KIND:     Pod
VERSION:  v1

DESCRIPTION:
     Pod is a collection of containers that can run on a host. This resource is
     created by clients and scheduled onto hosts.

FIELDS:
   apiVersion   <string>
     APIVersion defines the versioned schema of this representation of an
     object. Servers should convert recognized schemas to the latest internal
     value, and may reject unrecognized values. More info:
     https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#resources

   kind <string>
     Kind is a string value representing the REST resource this object
     represents. Servers may infer this from the endpoint the client submits
     requests to. Cannot be updated. In CamelCase. More info:
     https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#types-kinds

   metadata     <Object>
     Standard object's metadata. More info:
     https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#metadata

   spec <Object>
     Specification of the desired behavior of the pod. More info:
     https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#spec-and-status

   status       <Object>
     Most recently observed status of the pod. This data may not be up to date.
     Populated by the system. Read-only. More info:
     https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#spec-and-status
```

