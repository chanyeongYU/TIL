# django

- django_extensions 사용법 익히기? 검색
- `settings.py`에 django_extensions 추가

- `python manage.py shell`
- 되도록 timestamp 붙이기



- ```python
  def __str__(self):
      return "str"
  ```

  -> 객체 프린트를 바꿈 (overriding)

- 디버그 툴? 검색

- django를 왜 만들었는지 철학? 검색

- 앱을 기능별로 나누는게 좋음

- QuerySet이 중요

- 핵심은 Data

- https://docs.djangoproject.com/en/3.1/

- https://docs.djangoproject.com/en/3.1/topics/db/aggregation/

- 배포한번 시도해보기!

- many to many relation? 검색

  -> models.ManyToMany(settings....)

- AbstractUser Class? 검색

- Form, ModelForm 차이점?

- bootstrap 적용

- 로그인, 회원 기능 같은 경우 계정 앱에서 관리

- 모델 시각화 툴 ERD model visualization ()

- shell plus

- 변수명 함수명 코드 인덴트 중요!!!!!!



## 프로젝트 관리 순서(Django Project)

0. `mkdir [작업폴더]`

1. `touch .gitignore` 

   

2. `python -m venv venv`

3. `source venv/Scripts/activate` 인터프리터 설정

4. `python -m pip install --upgrade pip`

5. Package 설치

   ​	ex) `pip install django`

6. django project 시작 

   - `django-admin startproject django_git .`
   - 맨뒤에 `.`을 붙이면 현재 폴더에 바로 django project 생성

7. `.gitignore` 작성 (언어, 개발 환경, 가상환경 등..)

   7.1 https://www.toptal.com/developers/gitignore

   ![image-20200820165428380](C:\Users\i\AppData\Roaming\Typora\typora-user-images\image-20200820165428380.png)

8. `git init`

9. `git commit -m "first commit"`

10. 원격 레포지토리 연결

    - `git remote add origin https://github.com/chanyeongYU/DJANGO_GIT.git`

    - `git push origin master`









