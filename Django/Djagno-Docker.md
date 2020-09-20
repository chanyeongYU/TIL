# Django - Docker

### 환경

- vagrant
- centos
- docker







#### Dockerfile

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



