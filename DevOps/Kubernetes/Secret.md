# Secret

- 민감한 정보를 저장하기 위한 용도
- 네임 스페이스에 종속



#### Secret 생성 방법

- **password = 1q2w3e4r 라는 키-값을 저장하는 my-password 이름의 시크릿 저장**

  - vagrant@ubuntu:~$ kubectl create secret generic my-password --from-literal password=1q2w3e4r

    ```
    secret/my-password created
    ```

  - vagrant@ubuntu:~$ kubectl get secrets

    ```
    NAME                  TYPE                                  DATA   AGE
    default-token-lxlrx   kubernetes.io/service-account-token   3      3d23h
    my-password           Opaque
    ```

    - default-token-lxlrx: ServiceAccount에 의해 네이스페이스별로 자동으로 생성된 시크릿



- **파일로 부터 시크릿을 생성**

  - vagrant@ubuntu:~$ echo mypassword > pw1 && echo yourpassword > pw2

  - vagrant@ubuntu:~$ cat pw1

    ```
    mypassword
    ```

  - vagrant@ubuntu:~$ cat pw2

    ```
    yourpassword
    ```

  - vagrant@ubuntu:~$ kubectl create secret generic out-password --from-file pw1 --from-file pw2

    ```
    secret/out-password created
    ```

  - vagrant@ubuntu:~$ kubectl get secrets

    ```
    NAME                  TYPE                                  DATA   AGE
    default-token-lxlrx   kubernetes.io/service-account-token   3      3d23h
    my-password           Opaque                                1      6m6s
    out-password          Opaque                                2      85s
    ```



#### **시크릿 내용을 확인**

- vagrant@ubuntu:~$ kubectl describe secret my-password

  ```
  Name:         my-password
  Namespace:    default
  Labels:       <none>
  Annotations:  <none>
  
  Type:  Opaque
  
  Data
  ====
  password:  8 bytes	# password키에 해당하는 값을 확인할 수 없음(값의 크키만 출력)
  ```

- vagrant@ubuntu:~$ kubectl get secrets my-password -o yaml

  ```yaml
  apiVersion: v1
  data:
    password: MXEydzNlNHI=
  kind: Secret
  metadata:
    creationTimestamp: "2020-09-22T04:49:58Z"
    managedFields:
    - apiVersion: v1
      fieldsType: FieldsV1
      fieldsV1:
        f:data:
          .: {}
          f:password: {}
        f:type: {}
      manager: kubectl-create
      operation: Update
      time: "2020-09-22T04:49:58Z"
    name: my-password
    namespace: default
    resourceVersion: "41263"
    selfLink: /api/v1/namespaces/default/secrets/my-password
    uid: 13490328-7c1a-4aff-b557-df3a689415a4
  type: Opaque
  ```

- vagrant@ubuntu:~$ echo MXEydzNlNHI= | base64 -d

  ```
  1q2w3e4r
  ```



#### **시크릿에 저장된 키-값 쌍을 포드의 환경변수로 가져오기**

- **시크릿에 저장된 모든 키-값 쌍을 포드의 환경변수로 가져오기**

  - vagrant@ubuntu:~$ vim env-from-secret.yml

    ```yaml
    apiVersion: v1
    kind: Pod
    metadata:
      name: secret-env-example
    spec:
      containers:
        - name: my-container
          image: busybox
          args: ["tail", "-f", "/dev/null"]
          envFrom:
            - secretRef:
                name: my-password
    ```

  - vagrant@ubuntu:~$ kubectl apply -f env-from-secret.yml

    ```
    pod/secret-env-example created
    ```

  - vagrant@ubuntu:~$ kubectl exec secret-env-example -- env

    ```
    PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
    HOSTNAME=secret-env-example
    password=1q2w3e4r
    KUBERNETES_SERVICE_PORT=443
    KUBERNETES_PORT_443_TCP_PORT=443
    KUBERNETES_PORT_443_TCP_ADDR=10.96.0.1
    HOSTNAME_SVC_NODEPORT_PORT=tcp://10.103.63.99:8080
    HOSTNAME_SVC_NODEPORT_PORT_8080_TCP=tcp://10.103.63.99:8080
    HOSTNAME_SVC_NODEPORT_PORT_8080_TCP_PORT=8080
    KUBERNETES_PORT_443_TCP_PROTO=tcp
    HOSTNAME_SVC_NODEPORT_SERVICE_PORT=8080
    HOSTNAME_SVC_NODEPORT_SERVICE_PORT_WEB_PORT=8080
    HOSTNAME_SVC_NODEPORT_PORT_8080_TCP_ADDR=10.103.63.99
    KUBERNETES_SERVICE_HOST=10.96.0.1
    KUBERNETES_PORT_443_TCP=tcp://10.96.0.1:443
    HOSTNAME_SVC_NODEPORT_SERVICE_HOST=10.103.63.99
    HOSTNAME_SVC_NODEPORT_PORT_8080_TCP_PROTO=tcp
    KUBERNETES_SERVICE_PORT_HTTPS=443
    KUBERNETES_PORT=tcp://10.96.0.1:443
    HOME=/root
    ```

    

- **시크릿에 저장된 특정 키-값 쌍을 포드의 환경변수로 가져오기**

  - vagrant@ubuntu:~$ cp env-from-secret.yml selective-env-from-secret.yml

  - vagrant@ubuntu:~$ cat selective-env-from-secret.yml

    ```yaml
    apiVersion: v1
    kind: Pod
    metadata:
      name: secret-env-example
    spec:
      containers:
        - name: my-container
          image: busybox
          args: ["tail", "-f", "/dev/null"]
          env:
            - name: YOUR_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: out-password
                  key: pw2
    ```

  - vagrant@ubuntu:~$ kubectl apply -f selective-env-from-secret.yml

    ```
    pod/secret-env-example created
    ```

  - vagrant@ubuntu:~$ kubectl exec secret-env-example -- env | grep YOUR_PASSWORD

    ```
    YOUR_PASSWORD=yourpassword
    ```

    

- **시크릿의 저장된 모든 키-값 데이터를 파일로 포드의 볼륨에 마운트**

  - vagrant@ubuntu:~$ cp env-from-secret.yml volume-mount-secret.yml

  - vagrant@ubuntu:~$ vim volume-mount-secret.yml

    ```yaml
    apiVersion: v1
    kind: Pod
    metadata:
      name: secret-env-example
    spec:
      containers:
        - name: my-container
          image: busybox
          args: ["tail", "-f", "/dev/null"]
          volumeMounts:
            - name: secret-volume
              mountPath: /etc/secret	
      volumes:
        - name: secret-volume
          secret:
            secretName: out-password
    ```

    - **mountPath: /etc/secre**t: 컨테이너 내부에 /etc/secret/ 디렉터리 아래에 시크릿에 저장된 키 이름의 파일을 생성 (파일 내용은 키에 해당하는 값)

  - vagrant@ubuntu:~$ kubectl apply -f volume-mount-secret.yml

    ```
    pod/secret-env-example created
    ```

  - vagrant@ubuntu:~$ kubectl exec secret-env-example -- ls /etc/secret

    ```
    pw1
    pw2
    ```

  - vagrant@ubuntu:~$ kubectl exec secret-env-example -- cat /etc/secret/pw1 /etc/secret/pw2

    ```
    mypassword
    yourpassword
    ```

    

- **시크릿의 저장된 특정 키-값 데이터를 파일로 포드의 볼륨에 마운트**

  - vagrant@ubuntu:~$ cp volume-mount-secret.yml selective-volume-secret.yml

  - vagrant@ubuntu:~$ vim selective-volume-secret.yml

    ```
    apiVersion: v1
    kind: Pod
    metadata:
      name: secret-env-example
    spec:
      containers:
        - name: my-container
          image: busybox
          args: ["tail", "-f", "/dev/null"]
          volumeMounts:
            - name: secret-volume
              mountPath: /etc/secret
      volumes:
        - name: secret-volume
          secret:
            secretName: out-password
            items:
              - key: pw1
                path: password1
    ```

  - vagrant@ubuntu:~$ kubectl apply -f selective-volume-secret.yml

    ```
    pod/secret-env-example created
    ```

  - vagrant@ubuntu:~$ kubectl exec secret-env-example -- cat /etc/secret/password1

    ```
    mypassword
    ```



#### 시크릿은 사용 목적에 따라 여러 종류의 스크릿을 사용할 수 있음

- **Opaque 타입**
  - 시크릿 종류를 명시하지 않으면 자동으로 설정되는 타입
  - kubectl create secret generic 명령으로 생성
  - 사용자가 정의한 데이터를 저장할 수 있는 일반적인 목적의 시크릿
- **TLS 타입** 
  - TLS 연결에 사용되는 공개키와 비밀키 등을 저장하는데 사용
  - 포드 내의 애플리케이션이 보안 연결을 위해 인증서나 비밀키 등을 가져와야 할 때 TLS 타입의 시크릿을 제공



#### private registry에 접근할 때 사용하는 인증 정보를 시크릿으로 생성, 관리

- ~/.docker/config.json 파일을 이용해서 시크릿을 생성

  - vagrant@ubuntu:~$ docker login

    ```
    Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
    Username: uchan0
    Password:
    WARNING! Your password will be stored unencrypted in /home/vagrant/.docker/config.json.
    Configure a credential helper to remove this warning. See
    https://docs.docker.com/engine/reference/commandline/login/#credentials-store
    
    Login Succeeded
    ```

  - vagrant@ubuntu:~$ ls -al ~/.docker

    ```
    total 12
    drwx------ 2 vagrant vagrant 4096 Sep 22 06:10 .
    drwxr-xr-x 9 vagrant vagrant 4096 Sep 22 06:10 ..
    -rw------- 1 vagrant vagrant  165 Sep 22 06:10 config.json
    ```

    - config.json
      - docker login 성공하면 도커 엔진이 자동으로 config.json 파일에 인증 정보를 저장
      - config.json 파일을 그대로 시크릿으로 생성

  - vagrant@ubuntu:~$ cat ~/.docker/config.json

    ```
    {
            "auths": {
                    "https://index.docker.io/v1/": {
                            "auth": "[PASSOWORD]"	# BASE64로 인코딩
                    }
            },
            "HttpHeaders": {
                    "User-Agent": "Docker-Client/19.03.6 (linux)"
            }
    }
    ```

  - vagrant@ubuntu:~$ kubectl create secret generic registry-auth --from-file=.dockerconfigjson=/home/vagrant/.docker/config.json --type=kubernetes.io/dockerconfigjson

    ```
    secret/registry-auth created
    ```

  - vagrant@ubuntu:~$ kubectl get secrets

    ```
    NAME                  TYPE                                  DATA   AGE
    default-token-lxlrx   kubernetes.io/service-account-token   3      4d1h
    my-password           Opaque                                1      84m
    out-password          Opaque                                2      80m
    registry-auth         kubernetes.io/dockerconfigjson        1      43s
    ```

    

