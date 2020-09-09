

# Shell Script

- 명령어 프로그램을 실행할 때 사용하는 인터페이스
- 사용자가 입력한 명령을 해석하여 커널에 전달하거나 커널의 처리 결과를 사용자에게 전달하는 역할
- ubuntu에서 기본적으로 사용하는 shell은 **bash shell**



## 변수

- 대입할 때 `=` 앞뒤에 공백이 없어야함

- 모든 값은 문자열 취급

- 대소문자 구별

- 미리 선언할 필요없음

- 값에 공백이 들어가면 안됨

  ```shell
  $var=var test (X)
  $var="var test" (O)
  ```

- ubuntu@server-b:~$ `vi var1.sh`

    ```shell
    #!/bin/bash

    myvar="Hi Woo"         <= (0)
    echo $myvar            <= (1)
    echo "$myvar"          <= (2)
    echo '$myvar'          <= (3)
    echo \$myvar           <= (4)
    echo 값 입력:
    read myvar
    echo '$myvar' = $myvar	<= 사용자가 입력한 값이 저장

    exit 0
    ```

- ubuntu@server-b:~$ `sh var1.sh`

    ```
    Hi Woo		<= (1) $myvar를 변수로 인식해서 (0)에서 정의한 변수의 값이 출력
    Hi Woo      <= (2)
    $myvar		<= (3) $myvar를 변수로 인식하지 않고 문자열로 인식 (방법1)
    $myvar		<= (4) $myvar를 변수로 인식하지 않고 문자열로 인식 (방법2)
    값 입력:
    abcd		<= 입력한 값
    $myvar = abcd
    ```



### 숫자 계산

- 연산자 사용시 `expr`사용
- 수식과 함께 `(백쿼트)로 묶어야 함
-  \* 연산자는 예외적으로 \를 앞에 넣어야함

- ubuntu@server-b:~$ `vi numcalc.sh` 

  ```shell
  num1=100
  num2=$num1+200
  echo $num2
  
  num3=`expr $num1 + 200`
  echo $num3
  
  num4=`expr \( $num1 + 200 \) / 10 \* 2`
  echo $num4
  
  exit 0
  ```

- ubuntu@server-b:~$`sh numcalc.sh`  ouput

  ```she
  100+200
  300
  60
  ```



### 파라미터 변수



## 조건문

### if문



### case문

- 분기의 끝은 `;;`(세미콜론 2개)





## 반복문

### for~in

- 수 나열

    ```shell
    for 변수 in 값1, 값2, 값3, ...
    do
        반복할 문장
    done
    ```

- **`seq start_num end_num`**

    ```shell
    # vi sigma.sh
    
    hap=0
    
    for i in $(seq 1 100)
    do
        hap=`expt $hap +$i`
    done
    
    echo "1부터 100까지 합은 " $hap
    
    exit 0
    ```

    `sh sigma.sh`

- **{start_num..end_num}**
  
  - bash shell에서만 실행 가능
  
  ```shell
  # vi sigma.sh
  
    hap=0

    for i in {1..100}
    do
    hap=`expt $hap +$i`
    done

    echo "1부터 100까지 합은 " $hap

    exit 0
  ```
  
  `bash sigma.sh`



## 함수

### 함수 파라미터 사용

- 함수에 별로의 인자를 주지않음
- $1, $2, $3, ...



#### `eval`

- 문자열을 명령문으로 인식하여 실행

  ```shell
  #!/bin/sh
  
  str="ls -l"
  
  echo $str	=> 문자열 "ls -l" 출력
  eval $str	=> 명령문 ls -l 실행, 파일 목록 출력
  ```




#### `export`

- 변수를 전역 변수로 만들어 모든 shell에서 사용 가능

  `ex1.sh`

  ```shell
  #!/bin/sh
  
  echo $var1
  echo $var2
  
  exit 0
  ```

  `ex2.sh`

  ```shell
  #!/bin/sh
  
  var1="Local"
  export var2="Global"
  
  sh ex1.sh	=> "Global"만 출력한다
  
  exit 0
  ```

  - output

  ```
  Global
  ```

  

#### `printf`



#### `set`, `$(명령)`

- $(명령): Linux 명령을 결과로 사용
- set
  - 결과를 파라미터로 사용
  - 공백을 기준으로 나눔
  - $1, $2, $3, ...에 저장



#### `shift`

- 파라미터 변수를 왼쪽으로 한 단계씩 아래로 시프트

  ```shell
  #!/bin/sh
  
  myfunc(){
  	str""
  	while [ "$1" != "" ]; do
  		str="$str $1"
  		shift
  	done
  	
  	echo $str
  }
  
  myfunc AAA BBB CCC DDD EEE FFF GGG HHH III
  
  exit 0
  ```

  - output

  ```
  AAA BBB CCC DDD EEE FFF GGG HHH III
  ```

- 함수의 파라미터 수가 많을 때

- 또는 파라미터 수를 예측할 수 없을 때



#### `crontab`

- 주기적으로 실행

- `crontab -l`: 등록된 크론 확인

- `crontab -e`: 크혼 등록, 수정

  ```
  0	5	*	*	1	tar -zcf	/var/backups/home.tgz	/home/
  ```

  ```
  0(분)	5(시)	*(일)	*(월)	1(요일, 0~6:일~토)	tar -zcf(명령어)	/var/backups/home.tgz	/home/
  ```




- `backup.sh`생성

```shell
#!/bin/bash
set $(date)
fname="backup$1$2$3tar.xz"			⇐ 2020.09.09.tar.xz
tar cfJ /backup/$fname /home
```

- 크론에 등록 `vim /etc/crontab`

```shell
# /etc/crontab: system-wide crontab
# Unlike any other crontab you don't have to run the `crontab'
# command to install the new version when you edit this file
# and files in /etc/cron.d. These files also have username fields,
# that none of the other crontabs do.

SHELL=/bin/sh
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

# m h dom mon dow user  command
17 *    * * *   root    cd / && run-parts --report /etc/cron.hourly
25 6    * * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.daily )
47 6    * * 7   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.weekly )
52 6    1 * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.monthly )

# 매월 16일 새벽 3시 20분에 백업을 수행
20   03   16   *   *    root    /root/backup.sh
#
```



