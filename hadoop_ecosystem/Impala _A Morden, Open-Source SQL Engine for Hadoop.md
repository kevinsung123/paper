
## Impala : A Morden, Open-Source SQL Engine for Hadoop  
### source : Cloudera http://impala.io  
***  
**ABSTRACT**  
***  
**1. INTRODUCTION**  
***  
**2. USER VIEW OF IMPALA**  
  
**2-1. Physical schema design**  
- 테이블을 생성시 partition column을 지정 할 수 있다  
>ex) **CREATE TABLE** T(...) **PARTIONBED BY** (day int, month int)**LOCATION** <hdfs-path> **STORED AS PARQUET;**  
  
- unparitioned table에 대해서는 root 다이렉토리에 저장  
- partitioned table는 <root>/day=17/month=2에 저장  
-paritioned table이 인접 parition에 저장은 아니고, HDFS DATA NODE에 랜덤하게 분산하여 저장  
- Impala는 현재 다양한 파일포맷을 지원  
- compressed and uncompressed text files  
- sequence file(a spliitable form of text file)  
- RCFile(a legacy columnar format)  
- Avro(a binary row format)  
- Parquet  
- 각 partition마다 다른 파일 포맷 저장 가능  
>ALTER TABLE PARITION(day=17, month=2) SET FILEFORMAT PARQUET  
  
**2-2. SQL Support**  
- SQL-92 SELECT 구문문법 지원  
- SQL-2003 analytics functions  
- standard scalar data type : integer, floating point types, STRING, CHAR, VARCHAR, TIMESTAMP  
- HDFS storage manager의 제약으로 인해 UDPATE, DELTE 구문은 지원하지 않음  
- bulk insertion만 지원 : INSERT INTO .. SELECT  
- 전통적인 RDBMS와 달리 user는 테이블에 간단히 쉽게 data를 copying/moving 가능 => HDFS API 이용  
- bulk insert와 유사하게 Impala는 bulk data deletion 지원  
- dropping a table parititon (**ALTER TABLE DROP PARTITION**)  
- Impala는 UPDATE 구문을 지원하지 않기때문에 위를 지원  
- 대신에 dropping 그리고 re-adding partition 으로 지원  
- 초기데이터 적재 이후, 테이블의 데이터가 변화가 있다면  
- 사용자는 **COMPUTE STATS <table>** 구문사용 가능  
- Impala에 테이블에대한 통계정보를 모아서 query optimization에 사용가능  
***  
**3. ARCHITECTURE**  
  
- Impala는 massively-parallel query engine (대용량 병렬처리 쿼리)이다  
- 전통적인 RDBMS와 storage machine으로부터 분리  
![impala_architecture](./impala_architecture.PNG)  
- Impala는 3가지 서비스로구성  
1. **Impalad(Impala daemon)**  
- client 프로세스로부터 쿼리들을 받아들임  
- cluster에서 실해을 조정  
- 다른 impala데몬을 대신하여 개별 쿼리조각을 실행  
- impala데몬이 쿼리실행을 관리하여 첫번째 역할을 수행하면 coordinator라고 부른다  
- 모든 impala데몬은 대칭적. 모든 역할에서 작동  
- 위의 속성을 통해서 fault-tolerance와 load-balancing이 가능  
- ※ **ODBC(Open Database Connectivity)란?**  
- 어떤 응용프포그램을 사용하느지 관계없이, 데이터베이스를 자유롭게 사용하기위해 만든 응용프로그램 표준방법.  
- DBMS에 관계없이 어떤 응용 프로그램에서나 모두 접근하여 사용할 수 있도록 하기 위하여, 마이크로소프트에서 개발한 표준방법을 말한다. (응용프로그램과 DBMS 중간에서 데이터베이스 처리 프로그램을둔다)  
![ODBC](./ODBC.PNG)  
2. **Statestored(Statestore daemon)**  
- Impala의 publish-subscribe 서비스  
- cluster의 넓은 메타데이터를 모든 Impala 프로세스에 배포한다  
3. **Catalog(Catalog daemon)**
- Impala의 catalog repository 그리고 metadata를 접근하는 게이트웨이 역할
- catalogd를 통해 Impala데몬들은 외부카탈로그에 반영된 DDL명령을 실행
- 시스템변경 catalog는 statestore를 통해 브로드캐스트된