

# Shell

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

