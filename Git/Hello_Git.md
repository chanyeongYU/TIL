# Git

- 참고자료
  - **Pro Git (https://git-scm.com/book/ko/v2)**



## git init

- git 을 처음 초기화 할 때 

- `git init`



## git Status

- git 상태 중간점검? 같은 개념

- `got status`



## git clone

- Github Repository에서 Http 복사 후 git bash에
- `git clone [repo url]`



## git pull

- **push를 하기 전에 pull**

- `git pull origin master`



## git commit --amend

- commit 내용을 변경 할 때

- vim 편집기

- 3개 파일 중 2개 파일 만 커밋 하고 1개 파일을 까먹 었을 때 합쳐줄수 있음

  - `git add [two files]``
  - ``git commit -m "add two files"`
  - `git add [forgotten file]`
  - `git commit --amend`



## git restore -- [file]

- `git checkout -- [file]`과 같음

  ![image-20200821110929604](C:\Users\i\AppData\Roaming\Typora\typora-user-images\image-20200821110929604.png)

- 수정된 파일 되돌리기



## git restore --staged [file]

- git add 취소



## git branch

- 현재 branch 확인
- `git branch [new branch]` branch 생성
- 생성 전에 반드시 `git commit`
- `git checkout [new branch]` **=** `git switch [new branch]`
- `git branch -d [branch]` : 브랜치 삭제



## git checkout -b [new branch]

- 새로운 브랜치를 만들고 바로 이동



## git log --oneline --decorate --graph --all

- git log 그래프로 보기



## git diff HEAD

- `git add` 전과 차이점 확인



## git merge [brach]

- 다른 브랜치가 서로 다른 파일을 만졌을 때는 바로 merge

- 다른 브랜치가 같은 파일을 만졌을 때는 **conflict**



## 원격 저장소 연동

- `git remote add origin https://github.com/chanyeongYU/learn_git.git`

- `git push origin master`

- `git push -u origin master`

- 옵션 `-u` : master branch에 push



#### 로컬에 저장된 Github 계정 바꾸기 (Windows)

자격 증명 관리자

윈도우 자격 증명

![image-20200820162410910](C:\Users\i\AppData\Roaming\Typora\typora-user-images\image-20200820162410910.png)

일반 자격 증명에 github삭제



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

9.  `git commit -m "first commit"`

10. 원격 레포지토리 연결

    - `git remote add origin https://github.com/chanyeongYU/DJANGO_GIT.git`
    
    - `git push origin master`



## TIL

#### Today I Learned

`touch .gitignore`

`touch README.md`

`git init`



## 참고

- **origin/master 는 원격 Repo**
  - origin은 remote를 뜻함 
  - `git remote add origin [Repo Https]`
    - origin에 [Repo Https]를 넣는 다는 뜻
    - origin 대신 다른 이름 사용 가능
    - 여러 remote(다른 Https)를 사용 가능

- **HEAD -> master 는 로컬 Repo**

- **git 은 커밋만 봄: 커밋 기준**

- `warning: LF will be replaced by CRLF in test.py.`
  - unix -> window  간의 문제

  - 무시해도 되는 error
- **주의할 점**
  - `git add .`은 해당 폴더 안에 파일만 add
  - 브랜치 나누기 전 최초 커밋은 필수
  - docker 할 때는 `.dockerignore`
- **CI/CD**
  - Settings -> Actions

- **README.md 작성법**
  - [https://www.quantumdl.com/entry/Github-READMEmd-%EC%9E%91%EC%84%B1%EB%B2%95](https://www.quantumdl.com/entry/Github-READMEmd-작성법)

