# Django - Docker

### 환경

- vagrant
- centos
- docker
- python:3.7.6
- django



#### 1. Vagrant

```
# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "centos/7"
  config.vm.hostname = "chan"
  config.vm.network "private_network", ip: "192.168.33.10"
  config.vm.synced_folder "C:/workspace/Docker/Shared", "/home/sync"
  config.vm.provider "virtualbox" do |vb|
    vb.cpus = 2
    vb.memory = "2048"
  end

  config.vm.provision "shell", inline: <<-SHELL
    sudo yum update -y
    sudo yum install -y epel-release
    sudo yum install wget -y
    sudo yum install net-tools -y
  SHELL
end
```



#### 2. 도커 실행 및 권한 설정

`sudo chmod 666 /var/run/docker.sock`

`sudo systemctl start docker`



#### 3. 프로젝트 폴더 공유

- vagrant에서 설정한 공유 폴더에 프로젝트 폴더를 넣어 가상환경에서 공유



#### 4. Dockerfile

```dockerfile
FROM python:3.7.6

RUN mkdir /code
WORKDIR /code

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        postgresql-client \
    && rm -rf /var/lib/apt/lists/*

ADD requirements.txt /code/
RUN pip install --upgrade pip && pip install --no-cache-dir -r requirements.txt
COPY . /code/

EXPOSE 8000

WORKDIR /code/django_src
CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]
```



#### 5. Docker Image 및 Container 실행

- `$docker image build -t django-test .`
- `$docker container run -p 8000:8000 -idt django-test`



#### 6. VirtualBox에서 포트포워딩

​	<img src="..\img\image-20200921000651812.png" alt="image-20200921000910461"/>



#### 7. setting.py에서 ALLOWED_HOSTS 추가

```python
ALLOWED_HOSTS = [
    "127.0.0.1",
    "0.0.0.0"
]
```





#### 7. 호스트PC 브라우저에서 127.0.0.1:8000

​	<img src="..\img\image-20200921000910461.png" alt="image-20200921000910461" style="zoom: 50%;" />





