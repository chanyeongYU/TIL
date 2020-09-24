# Kubernetes

- 대부분의 리소스를 **오브젝트**라고 불리는 형태로 관리
- 컨테이너의 집합 **pod**
- 컨테이너의 집합을 관리하는 컨트롤러 **replica set**
- 사용자 **service account**
- 노드 **node**
- **kubectl** 또는 **YAML**파일을 정의해서 쿠버네티스 사용
- **k8s**



### K8s 구성

​	![image-20200918225439851](..\img\image-20200918225439851.png)

##### Master Node

- 전체 쿠버네티스 시스템을 제어하고 관리하는 쿠버네티스 컨트롤 플래인(**Control Plane**)을 실행



##### Control Plane

- 클러스터를 제어하고 작동시킴
- 어플리케이션을 실행하지는 않음
  - 실행은 노드에서
- 구성요소
  - **k8s API Server**: 사용자 control plane 구성요소와 통신
  - **Scheduler**: 어플리케이션의 배포를 담당 (어플리케이션의 배포 가능한 각 구성 요소를 워커 노드에 할당)
  - **Controler Manager**: 구성요소 복제본, 워커노드 추적, 노드 장애 처리 등과 같은 클러스터 단의 기능을 수행
  - **etcd**: 클러스터 구성을 지속적으로 저장하는 신뢰할 수 있는 분산 데이터 저장소



##### Worker Node

- 실제 배포되는 컨테이너 어플리케이션을 실행
- 어플리케이션을 실행하고 모니터링하며 어플리케이션에 서비스를 제공
- 구성요소
  - **Container Runtime**: 컨테이너를 실행하는 Docker
  - **Kubelet**: API서버와 통신하고 노드의 컨테이너를 관리
  - **Kube-Proxy**: 어플리케이션 구성요소간 네트워크 트래픽을 로드 밸런싱하는 쿠버네티스 서비스 프록시



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



## Replica Set

- 참고자료
  - https://kubernetes.io/ko/docs/concepts/workloads/controllers/replicaset/



#### pods 삭제

- vagrant@ubuntu:~$ kubectl get pods

  ```
  NAME           READY   STATUS    RESTARTS   AGE
  my-nginx-pod   2/2     Running   4          2d17h
  ```

  - 두 개의 컨테이너가 실행중
    - my-nginx-container
    - ubuntu-sidecar-container

- vagrant@ubuntu:~/kube01$ kubectl delete -f nginx-pod-with-ubuntu.yml

  또는

- vagrant@ubuntu:~/kube01$ kubectl delete pods my-nginx-pod

- vagrant@ubuntu:~/kube01$ kubectl get pods

  ```
  No resources found in default namespace.
  ```



#### pods 생성

- vagrant@ubuntu:~/kube01$ kubectl apply -f nginx-pod-with-ubuntu.yml

- vagrant@ubuntu:~/kube01$ kubectl get pods

  ```
  NAME           READY   STATUS    RESTARTS   AGE
  my-nginx-pod   2/2     Running   0          31s
  ```

  

#### **레플리카셋 정의**

- vagrant@ubuntu:~/kub01$ vim replicaset-nginx.yml

  ```yaml
  apiVersion: apps/v1
  kind: ReplicaSet
  metadata:
          name: replicaset-nginx
  spec:
          replicas: 3			⇐ 유지해야 하는 파드 개수
          selector:			⇐ 획득 가능한 파드를 식별하는 방법
                  matchLabels:
                          app: my-nginx-pods-label
          template:			⇐ 신규 생성되는 파드에 대한 데이터를 명시
                  metadata:
                          name: my-nginx-pod
                          labels:
                                  app: my-nginx-pods-label
                  spec:
                          containers:
                                  - name: my-nginx-container
                                    image: nginx:latest
                                    ports:
                                            - containerPort: 80
                                              protocol: TCP
  ```



#### 레플리카셋 생성

- vagrant@ubuntu:~/kube01$ kubectl apply -f replicaset-nginx.yml

- vagrant@ubuntu:~/kube01$ kubectl get pods

  ```
  NAME                     READY   STATUS    RESTARTS   AGE
  replicaset-nginx-86xg5   1/1     Running   0          23s
  replicaset-nginx-8fzh5   1/1     Running   0          23s
  replicaset-nginx-p9m2w   1/1     Running   0          23s
  ```

  

#### 레플리카셋 삭제

- vagrant@ubuntu:~/kub01$ kubectl delete replicaset replicaset-nginx

- vagrant@ubuntu:~/kub01$ kubectl get pods

  - pod도 함께 삭제

  ```
  No resources found in default namespace.
  ```



#### **레플리카셋은 라벨 셀렉터로 정의한 파드가 일정 개수가 되도록 유지**





## Deployment

- 레플리카셋, 파드의 배포를 관리
  - 애플리케이션의 업데이트와 배포를 쉽게 하기 위해 만든 개념(객체)



#### 사용하는 이유

- 디플로이먼트는 컨테이너 애플리케이션을 배포하고 관리하는 역할을 담당
- 애플리케이션을 업데이트 할 때 레플리카셋의 변경 사항을 저장하는 리비전을 남겨 롤백 가능하게 해 주고, 무중단 서비스를 위한 포드의 롤링 업데이트 전략을 지정할 있음
  - **롤백**
  - **이미지 변경**



## Service

- 쿠버네티스 클러스터 안에서 파드의 집합에 대한 경로나 서비스 디스커버리를 제공하는 리소스



#### Type

- 서비스 타입에 따라 포드에 접근하는 방법이 달라짐
  - **ClusterIP 타입**
    - 쿠버네티스 내부에서만 포들에 접근할 때 사용
    - 외부로 포드를 노출하지 않기 때문에 쿠버네티스 클러스터 내부에서만 사용되는 포드에 적합
  - **NodePort 타입**
    - 포드에 접근할 수 있는 포트를 클러스터의 모드 노드에 동일하게 개방
    - 외부에서 포드에 접근할 수 있는 서비스 타입
    - 접근할 수 있는 포트는 랜덤으로 정해지지만, 특정 포트로 접근하도록 설정할 수 있음
  - **LoadBalancer 타입**
    - 클라우드 플랫폼에서 제공하는 로드 밸러서를 동적으로 프로비저닝해서 포드에 연결
    - 외부에서 포드에 접근할 수 있는 서비스 타입
    - 일반적으로 AWS, GCP, … 등과 같은 클라우드 플랫폼 환경에서 사용



#### Deployment 생성

##### 모든 리소스 삭제

- vgrant@ubuntu:~/kube01$ vim deployment-hostname.yml

  ```yaml
  apiVersion: apps/v1
  kind: Deployment
  metadata:
          name: hostname-deployment
  spec:
          replicas: 3
          selector:
                  matchLabels:
                          app: webserver
          template:
                  metadata:
                          name: my-webserver
                          labels:
                                  app: webserver
                  spec:
                          containers:
                                  - name: my-webserver
                                    image: alicek106/rr-test:echo-hostname
                                    ports:
                                            - containerPort: 80
  ```

- vagrant@ubuntu:~/kube01$ kubectl apply -f deployment-hostname.yml

- vagrant@ubuntu:~/kube01$ kubectl get pods

  ```
  NAME                                   READY   STATUS              RESTARTS   AGE
  hostname-deployment-7dfd748479-brkxl   0/1     ContainerCreating   0          6s
  hostname-deployment-7dfd748479-g2cj5   0/1     ContainerCreating   0          6s
  hostname-deployment-7dfd748479-p7dh6   0/1     ContainerCreating   0          6s
  ```

- **o wide 옵션을 이용해서 각 파드에 할당된 IP를 확인 ⇒** **-o, --output='' : 출력 포맷을 지정**

  - vagrant@ubuntu:~/kube01$ kubectl get pods -o wide



##### 하나의 pod를 임시로 생성 후 deployment-hostname으로 생성된 pod로 http 요청 전달

- vagrant@ubuntu:~/kube01$ kubectl run -it --rm debug --image=alicek106/ubuntu:curl --restart=Never curl 172.18.0.3 | grep Hello

  ```html
  <p>Hello,  hostname-deployment-7dfd748479-g2cj5</p>     </blockquote>
  ```





#### NodePort 타입의 서비스 → 서비스를 이용해 파드를 외부에 노출이 가능

- **매니페스트 파일을 작성**

  - vagrant@ubuntu:~/kube01$ vi hostname-svc-nodeport.yml

    ```yaml
    apiVersion: v1
    kind: Service
    metadata:
      name: hostname-svc-nodeport
    spec:
      ports:
        - name: web-port
          port: 8080
          targetPort: 80
      selector:
        app: webserver
      type: NodePort
    ```

- **서비스 생성 및 확인**

  - vagrant@ubuntu:~/kube01$ kubectl apply -f hostname-svc-nodeport.yml

  - vagrant@ubuntu:~/kube01$ kubectl get services

    ```
    NAME                    TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
    hostname-svc-nodeport   NodePort    10.103.63.99   <none>        8080:31582/TCP   7s
    kubernetes              ClusterIP   10.96.0.1      <none>        443/TCP          3d2h
    ```

  - vagrant@ubuntu:~/kube01$ kubectl get nodes -o wide

    ```
    NAME       STATUS   ROLES    AGE    VERSION   INTERNAL-IP   EXTERNAL-IP   OS-IMAGE           KERNEL-VERSION       CONTAINER-RUNTIME
    minikube   Ready    master   3d2h   v1.19.0   172.17.0.2    <none>        Ubuntu 20.04 LTS   4.15.0-117-generic   docker://19.3.8
    ```

- **노드(172.17.0.2)에서 서비스 포토(31582)로 접근**

  - vagrant@ubuntu:~/kube01$ curl http://172.17.0.2:31582  -s| grep Hello

