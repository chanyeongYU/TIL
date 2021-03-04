# ConfigMap, Secret

- 환경에 따라 변하는 값이 있을 경우 각 환경마다 컨테이너 이미지를 가지고 있으면 부담이 됨
- 이런 환경에 따라 변할 필요가 있는 값들을 별도로 외부에서 결정할 수 있게 해줌
- ConfigMap: 상수 값
- Secret: 보안 Key와 같이 보안적인 관리가 필요한 값

- Key-Value 쌍으로 이루어져 있음
- Secret은 값을 넣을 때 Base64로 인코딩
  - Pod에 설정될 때는 Decoding



**Secret의 보안적 요소**

- Memory에 저장
- 1Mb 까지 생성할 수 있음



### Key-value 상수 값을 환경변수로 정의

**ConfigMap**

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: cm-dev
data:
  SSH: 'false'    # Key-Value는 모두 String이기 때문에 ''으로 감싸야함, 안하면 에러
  User: dev
```



**Secret**

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: sec-dev
data:
  Key: MTIzNA==    # base64 인코딩, 안하면 에러
```



**Pod**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-file
spec:
  containers:
  - name: container
    image: kubetm/init
    env:
    - name: file-c
      valueFrom:
        configMapKeyRef:
          name: cm-file
          key: file-c.txt
    - name: file-s
      valueFrom:
        secretKeyRef:
          name: sec-file
          key: file-s.txt
```





### 파일을 환경변수로 정의

- 파일명이 Key, 파일 내용이 Value



**ConfigMap**

`echo "Content" >> file-c.txt`

`kubectl create configmap cm-file --from-file=./file-c.txt`



**Secret**

`echo "Content" >> file-s.txt`

`kubectl create secret generic sec-file --from-file=./file-s.txt`



**Pod**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-file
spec:
  containers:
  - name: container
    image: kubetm/init
    env:
    - name: file-c    # Pod 내에서는 name을 별도로 지정해 줌
      valueFrom:
        configMapKeyRef:
          name: cm-file
          key: file-c.txt
    - name: file-s
      valueFrom:
        secretKeyRef:
          name: sec-file
          key: file-s.txt
```





### 파일을 마운팅하는 방법

**ConfigMap**

`echo "Content" >> file-c.txt`

`kubectl create configmap cm-file --from-file=./file-c.txt`



**Secret**

`echo "Content" >> file-s.txt`

`kubectl create secret generic sec-file --from-file=./file-s.txt`



**Pod**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-mount
spec:
  containers:
  - name: container
    image: kubetm/init
    volumeMounts:
    - name: file-volume
      mountPath: /mount
  volumes:
  - name: file-volume
    configMap:    # volume 내부에 ConfigMap 설정
      name: cm-file
```





### 환경변수 설정 방식과 Volume Mount 방식의 차이점

- 이미 Pod에 설정된 상태에서 ConfigMap/Secret 값이 바뀌게 된다면?

  - **환경변수 설정 방식은 한번 주입을 하면 끝이기 때문에 Pod의 환경변수 값은 영향이 없음**

    - **Pod가 재생성 된다면 영향을 받음**

    

  - **Volume Mount 방식은 Pod 내부의 설정 값도 바뀜**