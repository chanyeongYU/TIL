# Practice of Hadoop1

### 컨테이너 기반 하둡 구성

#### 하둡 설치 방식

- 독립 실행(Standalone) 모드
  - 하둡의 기본 실행 모드
  - 로컬 장비에서만 실행
  - 분산 환경을 고려한 테스트 불가능
- **가상 분산(Pseudo-distributed) 모드**
  - 하나의 장비에 모든 하둡 환경을 설정하고 서비스를 제공
  - HDFS, MapReduce와 관련된 데몬들을 하나의 장비에서 실행
- 완전 분산(Fully distributed) 모드
  - 여러 대의 장비에 하둡을 설치
  - 하둡으로 라이브 서비스 제공 가능



#### 하둡 컨테이너 실행

|     파일명      |                             용도                             |
| :-------------: | :----------------------------------------------------------: |
|  hadoop-env.sh  | 하둡을 실행하는 쉘 스크립트 파일(bin 디렉토리)에서 필요한 환경변수 설정 (JDK 경로, 클래스 패스, 데몬 실행 옵션 등) |
|     masters     |              보조 네임노드를 실행할 서버를 설정              |
|     slaves      |        데이터노드(태스크트랙커)를 실행할 서버를 설정         |
|  core-site.xml  | HDFS와 MapReduce에서 공통적으로 사용할 환경 정보를 설정 (hadoop-core-1.2.1.jar에 포함되어 있는 core-default.xml을 오버라이드) |
|  hdfs-site.xml  | HDFS에서 사용할 환경 정보를 설정 (hadoop-core-1.2.1.jar에 포함되어 있는 hdfs-default.xml을 오버라이드) |
| mapred-site.xml | MapReduce에서 사용할 환경 정보를 설정 (hadoop-core-1.2.1.jar에 포함되어 있는 mapred-default.xml을 오버라이드) |

- `# cat conf/core-site.xml`

  ```xml
  <?xml version="1.0"?>
  <?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
  
  <!-- Put site-specific property overrides in this file. -->
  
  <configuration>
  	<property>
  		<name>fs.default.name</name>
  		<value>hdfs://localhost:9000</value>
  	</property>
  	<property>
  		<name>hadoop.tmp.dir</name>
  		<value>/root/data-hadoop</value>
  	</property>
  </configuration>
  ```

  - hadoop.tmp.dir
    - hdfs의 데이타 또는 Namenode의 Metadata를 저장할 위치를 설정하는 환경변수



### 하둡 실행하기

#### NameNode 초기화

- **하둡 설치 디렉토리에서 실행**

- `# ./bin/hadoop namenode -format`

  ```
  20/10/15 03:45:04 INFO namenode.NameNode: STARTUP_MSG: 
  /************************************************************
  STARTUP_MSG: Starting NameNode
  STARTUP_MSG:   host = bd8faec09f41/172.17.0.2
  STARTUP_MSG:   args = [-format]
  STARTUP_MSG:   version = 1.2.1
  STARTUP_MSG:   build = https://svn.apache.org/repos/asf/hadoop/common/branches/branch-1.2 -r 1503152; compiled by 'mattf' on Mon Jul 22 15:23:09 PDT 2013
  STARTUP_MSG:   java = 1.8.0_252
  ************************************************************/
  20/10/15 03:45:04 INFO util.GSet: Computing capacity for map BlocksMap
  20/10/15 03:45:04 INFO util.GSet: VM type       = 64-bit
  20/10/15 03:45:04 INFO util.GSet: 2.0% max memory = 932184064
  20/10/15 03:45:04 INFO util.GSet: capacity      = 2^21 = 2097152 entries
  20/10/15 03:45:04 INFO util.GSet: recommended=2097152, actual=2097152
  20/10/15 03:45:04 INFO namenode.FSNamesystem: fsOwner=root
  20/10/15 03:45:04 INFO namenode.FSNamesystem: supergroup=supergroup
  20/10/15 03:45:04 INFO namenode.FSNamesystem: isPermissionEnabled=true
  20/10/15 03:45:04 INFO namenode.FSNamesystem: dfs.block.invalidate.limit=100
  20/10/15 03:45:04 INFO namenode.FSNamesystem: isAccessTokenEnabled=false accessKeyUpdateInterval=0 min(s), accessTokenLifetime=0 min(s)
  20/10/15 03:45:04 INFO namenode.FSEditLog: dfs.namenode.edits.toleration.length = 0
  20/10/15 03:45:04 INFO namenode.NameNode: Caching file names occuring more than 10 times 
  20/10/15 03:45:04 INFO common.Storage: Image file /root/data-hadoop/dfs/name/current/fsimage of size 110 bytes saved in 0 seconds.
  20/10/15 03:45:04 INFO namenode.FSEditLog: closing edit log: position=4, editlog=/root/data-hadoop/dfs/name/current/edits
  20/10/15 03:45:04 INFO namenode.FSEditLog: close success: truncate to 4, editlog=/root/data-hadoop/dfs/name/current/edits
  20/10/15 03:45:04 INFO common.Storage: Storage directory /root/data-hadoop/dfs/name has been successfully formatted.
  20/10/15 03:45:04 INFO namenode.NameNode: SHUTDOWN_MSG: 
  /************************************************************
  SHUTDOWN_MSG: Shutting down NameNode at bd8faec09f41/172.17.0.2
  ************************************************************/
  ```



#### 하둡 데몬 실행 및 확인

- **하둡 설치 디렉토리에서 실행**

- `# ./bin/start-all.sh`

  ```
  starting namenode, logging to /root/hadoop-1.2.1/libexec/../logs/hadoop--namenode-bd8faec09f41.out
  localhost: starting datanode, logging to /root/hadoop-1.2.1/libexec/../logs/hadoop-root-datanode-bd8faec09f41.out
  localhost: starting secondarynamenode, logging to /root/hadoop-1.2.1/libexec/../logs/hadoop-root-secondarynamenode-bd8faec09f41.out
  starting jobtracker, logging to /root/hadoop-1.2.1/libexec/../logs/hadoop--jobtracker-bd8faec09f41.out
  localhost: starting tasktracker, logging to /root/hadoop-1.2.1/libexec/../logs/hadoop-root-tasktracker-bd8faec09f41.out
  ```

- `# jps` (java process)

  ```
  433 SecondaryNameNode
  721 Jps
  307 DataNode
  516 JobTracker
  199 NameNode
  637 TaskTracker
  ```



#### 하둡 실행

- http://[Hadoop IP Address]:50070
  - 50070 Port: HDFS 웹 인터페이스 접근하는 Port Number
- http://[Hadoop IP Address]:50030
  - 50030 Port: MapReduce 웹 인터페이스 접근하는 Port Number



### 하둡 종료

#### 하둡 종료하기

- 하둡 설치 디렉토리에서 실행
- `# ./bin/stop-all.sh`



#### 하둡 컨테이너 종료하기

- `# exit`
- `docker exec` 명령어를 통해 컨테이너를 실행했을 경우
  - `sudo docker stop hadoop-practice`



#### 하둡 컨테이너 재시작하기

- `sudo docker exec -it hadoop-practice /bin/bash`
