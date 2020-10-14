# UTILIZING HDFS

### Command-Line Interface

- Basic Filesystem Operations

  - reading files, creating directories, moving files, deleting data, listing directories, …

  - `hadoop fs –help`

    - 모든 명령어의 도움말을 볼 수 있음

  - 로컬 파일 시스템에서 HDFS로 파일을 복사

    ```
    % hadoop fs -copyFromLocal input/docs/quangle.txt \
    	hdfs://localhost/user/tom/quangle.txt	-> HDFS URI
    
    % hadoop fs -copyFromLocal input/docs/quangle.txt /user/tom/quangle.txt
    												   ~~~~~~~~~~~~~~~~~~~~
    												   Using default HDFS
    % hadoop fs -copyFromLocal input/docs/quangle.txt quangle.txt
    												  ~~~~~~~~~~~
    										Relative path to home directory
    ```

    **:star:HDFS is not POSIX-compliant!!**

    - POSIX: **P**ortable **O**perating **S**ystem **I**nterface / 이식 가능 운영 체제 인터페이스
    - 리눅스 파일 시스템 명령어는 사용 안됨

  - HDFS에서 로컬 파일 시스템으로 파일을 복사

    ```
    % hadoop fs -copyToLocal quangle.txt quangle.copy.txt
    % md5 input/docs/quangle.txt quangle.copy.txt
    ```

    -> 출력을 보면 원본과 복사본의 md5 해쉬값이 같게 나옴

  - HDFS에서 디렉토리 생성 후 파일 목록 보기

    ```
    % hadoop fs -mkdir books
    % hadoop fs -ls
    ```

     ![image-20201015014527456](..\..\img\image-20201015014527456.png)



### Hadoop Filesystems

- Hadoop은 파일 시스템에 대한 **추상적인** 개념을 가지고 있음
  - **HDFS는 Hadoop 파일 시스템의 하나의 구현물**
  - Java 추상 클래스인 **org.apache.hadoop.fs.FileSystem**이 Hadoop 파일 시스템의 Client Interface
  - 다양한 스토리지와 함께 Hadoop Ecosystem Applications의 코드 수정 없이 사용 가능<img src="..\..\img\image-20201015015145445.png" alt="image-20201015015145445" style="zoom:80%;" />
    -  Cloud Connector를 통해 Amazon S3, Azure ADLS 및 Azure WASB 스토리지 서비스, Google 클라우드 스토리지에 저장된 데이터에 액세스하고 작업할 수 있음
- Interfaces
  - Hadoop은 Java로 만들어져 대부분 Hadoop 파일 시스템 상호 작용은 **Java API를 통해** 이뤄짐
    - Filesystem Shell (hadoop fs)은 Java FileSystem Class를 사용하여  파일 시스템 작업을 제공하는 Java application
    - 자바를 사용하지 않는 개발자들은 불편함
      - 다양한 파일 시스템 Interface의 필요성
  - HTTP
    - WebHDFS 프로토콜에 의해 HTTP REST API형태로 제공
    - 네이티브 Java 클라이언트보다 느림
      - 대용량의 데이터 전송에는 피하는게 좋음
  - C
    - Java FileSystem interface와 비슷하게 C Library인 "libhdfs"제공
    - WebHDFS interface를 사용하는 "libwebhdfs" Library제공
  - NFS
    - NFSv3 게이트웨이를 사용하여 로컬 파일 시스템에 HDFS를 마운트할 수 있음
    - Unix 유틸리티(ex: ls 및 cat)를 사용하여 파일 시스템과 상호 작용
    - 파일에 Apeend는 가능하지만, **파일 수정은 불가능**
  - FUSE
    - Filesystem in Userspace(FUSE)
    - 사용자 공간에 구현된 파일 시스템을 Unix 파일 시스템으로 통합
    -  Hadoop NFS gateway가 더욱 안정적인 솔루션