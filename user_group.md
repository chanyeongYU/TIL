# 사용자 관리

- 한 리눅스 서버에 여러 사용자가 사용 가능
- SUPERUSER는 모든 권한

- 모든 사용자는 혼자 존재하는 것이 아니라 하나 이상의 그룹에 소속

- RBAC(Role-Based Access Control)



**`cat /etc/passwd`**

- user_name : user_password : UID : GID : 추가정보 : Home Directory : 기본 셸
- password는 x로 표시, shadow 파일로 이전



**`cat /etc/group`**

- group_name : group_password : GID : 보조 그룹 사용자
- password는 x로 표시



## 사용자 관련 명령어

**`adduser`**명령어

- 사용자 추가

- `adduser user_name`

- `adduser --uid UID user_name`: UID 지정 옵션
- `adduser --gid GID user_name`: GID 지정 옵션

- `adduser --shell SHELL user_name`: SHELL 지정 옵션
- 자세한 옵션은 **`adduser --help`**
- 별도로 그룹을 지정하지 않으면 자동으로 사용자 이름과 동일한 그룹이 생성 (좋은 방식은 아님, 그룹생성 후, 유저 생성)



**`passwd user_name`** 

- 비밀번호 설정 및 변경



**`usermod`**

- user의 속성 변경
- `usermod [--shell SHELL] [--groups GNAME] user_name`



**`userdel user_name`**

- USER DELETE
- `userdel -r user_name`: user삭제 및 홈 디렉토리 삭제



**`change`**

- 사용자의 비밀번호를 주기적으로 변경하도록 설정하는 명령어



**`groups`**

- 사용자가 소속된 그룹을 보여주는 명령어



사용자가 생성 될 때마다 `/etc/skel`에 있는 example.desktop이 사용자 홈 디렉토리로 복사



## 그룹 관련 명령어

- 사용자가 있는데 그룹을 삭제하면?



## 파일 소유와 허가권

#### `-rw-r--r-- 1 root root 0  9월  8 10:42 test`

##### 파일유형

- d: Directory
- -: 일반 파일
- b: 블럭 디바이스(HDD, USB, CD...)
- c: 문자 디바이스(키보드, 마우스, 프린터)
- l: Link



##### 파일 허가권

​	![image-20200908104546461](C:\Users\chan\AppData\Roaming\Typora\typora-user-images\image-20200908104546461.png)

- read / write / execute



**`chmod`**

- 파일의 허가권을 변경

- `chmod 777 file_name`

  - 777 -> 111 111 111(2진수): 모두가 읽고 쓰고 실행가능

- chmod "사용자유형" "+ or -" "권한"

  - Symbolic Method

  - | Symbol |  Function  | Description  |
    | :----: | :--------: | :----------: |
    |   u    |    Who     | User (owner) |
    |   g    |    Who     |    Group     |
    |   o    |    Who     |    Others    |
    |   a    |    Who     |     All      |
    |   =    |  Operator  |    Assign    |
    |   +    |  Operator  |     Add      |
    |   -    |  Operator  |    Remove    |
    |   r    | Permission |     Read     |
    |   w    | Permission |    Write     |
    |   x    | Permission |   Execute    |

    