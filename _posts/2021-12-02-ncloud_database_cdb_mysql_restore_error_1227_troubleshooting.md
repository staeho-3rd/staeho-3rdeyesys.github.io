---
date: 2021-12-02
last_modified_at: 2021-12-02
title: MySQL 복구 시 ERROR 1227 (42000) 문제 원인과 해결방법
categories:
  - 5.database
description: 네이버 클라우드 Cloud DB for MySQL 복구(Restore)시에 발생하는 오류 ERROR 1227 (42000) 문제 원인과 해결방법입니다.
type: Document
set: database
order_number: 14
---

## 개요
네이버 클라우드(Ncloud) Cloud DB for MySQL을 사용하면서 백업 데이터를 복구 하려고 할 때 ERROR 1227 (42000) 오류가 발생하는 경우가 있습니다. 
오류 메시지로만 보면 계정 권한에 문제가 있는 것처럼 보이는데 정확하게 문제 원인이 무엇인지, 그리고 해결방법은 어떤 것이 있는지 정리해보겠습니다.

## 오류 메시지
전체 오류 메시지는 아래와 같습니다.  
이 오류 메시지의 원인은 크게 2가지로 구분할 수 있는데, 상황에 따라 각각의 원인 1가지에 해당하는 경우도 있고, 2가지 원인이 모두 해당되는 경우도 있습니다.

``` bash
ERROR 1227 (42000) at line 77: Access denied; you need (at least one of) the SUPER privilege(s) for this operation
```

## 원인 1
첫번째 원인은 GTID(global transaction identifier)와 관련되어 있습니다.  
네이버 클라우드(Ncloud) Cloud DB for MySQL 상품은 GTID(Global Transaction IDentifier)를 사용하는데, 
보통의 mysql db 복구(Restore)는 GTID를 사용하지 않는 방법이기 때문에 백업(Backup) 단계에서 [**--set-gtid-purged=OFF**] 옵션을 추가해야 하는데 이 옵션을 사용하지 않았기 때문입니다.

## GTID 란?
GTID는 Global Transaction Identifier의 약자로 MySQL 복제에서 서버의 각 트랜잭션을 구분하는 고유한 식별자입니다. 
GTID는 모든 트랜잭션과 1:1 관계이며, GTID를 활용하면 복제본으로 장애 조치, 계층적 복제, 특정 시점으로 백업 복구하는 등의 작업을 더 쉽게 구현할 수 있으며, 오류 발생 빈도도 줄일 수 있습니다.

## 오류 상황 재현
아래와 같이 서버에서 mysqldump 명령으로 Cloud DB for MySQL DB를 백업 받는 상황을 가정해보겠습니다. 여기서는 [**--set-gtid-purged=OFF**] 옵션을 사용하지 않았습니다.  
[**--set-gtid-purged=OFF**] 옵션을 사용하지 않고, 백업을 받으면 백업은 정상 진행되지만, 아래와 같이 [**--set-gtid-purged=OFF**] 해야 한다는 Warning 메시지가 표시됩니다.

``` bash
~# mysqldump -u user -p -h db-......vpc-cdb.ntruss.com -S /var/lib/mysql/mysql.sock --single-transaction --routines --triggers --events --databases test  > dumpfile1.sql
```

<img src="../../images/ncloud_database_cdb_mysql_restore_error_1227_troubleshooting_01.png" alt="네이버 클라우드 Cloud DB for MySQL 복구(Restore)시에 발생하는 오류 ERROR 1227 (42000) 문제 원인과 해결방법" style="width:770px;align:center">

위에서 생성한 [**--set-gtid-purged=OFF**] 옵션을 사용하지 않은 백업 파일(dumpfile1.sql)을 이용해서 복구(Restore)시에 아래와 같이 ERROR 1227 (42000) 오류가 발생하면서 복구가 되지 않습니다.

``` bash
~# mysql -h db-......vpc-cdb.ntruss.com -u user -p < ./dumpfile1.sql
```
<img src="../../images/ncloud_database_cdb_mysql_restore_error_1227_troubleshooting_03.png" alt="네이버 클라우드 Cloud DB for MySQL 복구(Restore)시에 발생하는 오류 ERROR 1227 (42000) 문제 원인과 해결방법" style="width:770px;align:center">

## 해결 방법 1
이번에는 Warning 메시지에 나온 것처럼 [**--set-gtid-purged=OFF**] 옵션을 사용해서 백업을 받아 보겠습니다.

``` bash
~# mysqldump --set-gtid-purged=OFF -u user -p -h db-......vpc-cdb.ntruss.com -S /var/lib/mysql/mysql.sock --single-transaction --routines --triggers --events --databases test  > dumpfile2.sql
```

<img src="../../images/ncloud_database_cdb_mysql_restore_error_1227_troubleshooting_02.png" alt="네이버 클라우드 Cloud DB for MySQL 복구(Restore)시에 발생하는 오류 ERROR 1227 (42000) 문제 원인과 해결방법" style="width:770px;align:center">

[**--set-gtid-purged=OFF**] 옵션을 사용해서 생성한 백업 파일(dumpfile2.sql)을 이용해서 복구를 하면 아래와 같이 문제없이 복구가 잘됩니다.  
그런데 이렇게 옵션을 적용해도 동일한 오류가 발생하는 경우가 있는데 이에 대해서는 아래쪽 [원인 2]에서 확인해보겠습니다.

``` bash
~# mysql -h db-......vpc-cdb.ntruss.com -u user -p < ./dumpfile2.sql
```
<img src="../../images/ncloud_database_cdb_mysql_restore_error_1227_troubleshooting_04.png" alt="네이버 클라우드 Cloud DB for MySQL 복구(Restore)시에 발생하는 오류 ERROR 1227 (42000) 문제 원인과 해결방법" style="width:770px;align:center">


## 해결 방법 2
그런데 서비스를 하다 보면 DB 사이즈가 너무 커서 다시 백업을 진행하기 어렵다던가 하는 경우가 있습니다. 이럴 때는 백업 파일에 있는 GTID 관련 내용을 삭제하고 복구하시면 문제가 해결됩니다.  
백업 파일 상단과 하단에 각각 아래와 같은 코드가 포함되어 있는데 이 내용을 삭제하거나 주석처리한 후에 복구를 시도하면 문제 없이 복구가 완료됩니다.
``` bash
# 백업 파일 상단에 위치
SET @MYSQLDUMP_TEMP_LOG_BIN = @@SESSION.SQL_LOG_BIN;
SET @@SESSION.SQL_LOG_BIN= 0;

# 백업 파일 하단에 위치
SET @@GLOBAL.GTID_PURGED='207****-4***-7**-9*****c:1-12,
2****-4**-9**-b**-d*****:1-19';
```


## 원인 2
위에서 설명한 [**--set-gtid-purged=OFF**]을 적용한 백업 파일을 이용해서 복구를 진행해도 동일한 오류가 발생하는 경우가 있는데 
원인은 Trigger, Stored Routines (Procedures and Functions), View, Event 생성과 관련된 [**DEFINER**] 권한 때문입니다.  즉,  **Trigger, Stored Routines (Procedures and Functions), View, Event를 생성한 계정과 DB 복구를 시도하는 계정이 다른 경우에 발생하는 문제**입니다.  

[원인 1]에서는 복구를 시도할 때 [user] 계정을 사용했었는데 이 [user]계정으로 Trigger, Stored Routines (Procedures and Functions), View, Event 등을 생성한 상황에서 다른 계정 [new_user]를 이용해서 복구를 시도하는 상황을 가정보겠습니다. 
[new_user] 계정으로 복구를 시도하면 [**--set-gtid-purged=OFF**]을 적용한 백업 파일(dumpfile2.sql)을 이용했음에도 동일한 ERROR 1227 (42000) 오류가 발생하는 것을 확인할 수 있습니다.

<img src="../../images/ncloud_database_cdb_mysql_restore_error_1227_troubleshooting_05.png" alt="네이버 클라우드 Cloud DB for MySQL 복구(Restore)시에 발생하는 오류 ERROR 1227 (42000) 문제 원인과 해결방법" style="width:770px;align:center">

아래와 같이 MySQL은 보안 강화를 위해 Trigger, Stored Routines (Procedures and Functions), View, Event를 처음 생성한 계정을 [**DEFINER**]로 명시해 둠으로써 다른 계정으로 접근하지 못하도록 하는 것이 기본 설정이 되어 있습니다.  
그러므로 DB 복구를 시도할 때는 [**DEFINER**] 관련 내용을 삭제하거나 동일한 계정 또는 SUPER privilege를 가진 계정으로 복구해야 합니다.

<img src="../../images/ncloud_database_cdb_mysql_restore_error_1227_troubleshooting_08.png" alt="네이버 클라우드 Cloud DB for MySQL 복구(Restore)시에 발생하는 오류 ERROR 1227 (42000) 문제 원인과 해결방법" style="width:770px;align:center">

## 해결 방법 1
첫번째 해결 방법은 백업 파일에서 [**DEFINER**] 관련 내용을 모두 찾아서 삭제하는 방법이 있습니다. 
여기서는 대표적으로 FUNCTION, PROCEDURE, VIEW에 해당하는 예시를 살펴보겠습니다.

#### FUNCTION
DB에서 FUNCTION을 사용했다면 아래와 같이 FUNCTION 생성 코드에 [**CREATE DEFINER=\`user\`@\`%\` FUNCTION**]처럼 계정이 표시되는데, 여기서 [**DEFINER=\`user\`@\`%\`**] 이 부분을 삭제하시면 됩니다.

<img src="../../images/ncloud_database_cdb_mysql_restore_error_1227_troubleshooting_06.png" alt="네이버 클라우드 Cloud DB for MySQL 복구(Restore)시에 발생하는 오류 ERROR 1227 (42000) 문제 원인과 해결방법" style="width:770px;align:center">

#### PROCEDURE
DB에서 PROCEDURE를 사용했다면 아래와 같이 PROCEDURE 생성 코드에 [**CREATE DEFINER=\`user\`@\`%\` PROCEDURE**]처럼 계정이 표시되는데, 여기서 [**DEFINER=\`user\`@\`%\`**] 이 부분을 삭제하시면 됩니다.
<img src="../../images/ncloud_database_cdb_mysql_restore_error_1227_troubleshooting_07.png" alt="네이버 클라우드 Cloud DB for MySQL 복구(Restore)시에 발생하는 오류 ERROR 1227 (42000) 문제 원인과 해결방법" style="width:770px;align:center">

#### VIEW
DB에서 VIEW를 사용했다면 아래와 같이 VIEW 생성 코드에 [**/*!50013 DEFINER=\`user\`@\`%\` SQL SECURITY DEFINER */**]처럼 계정이 표시되는데, 여기서는 이 라인을 모두 삭제하시면 됩니다.
<img src="../../images/ncloud_database_cdb_mysql_restore_error_1227_troubleshooting_08.png" alt="네이버 클라우드 Cloud DB for MySQL 복구(Restore)시에 발생하는 오류 ERROR 1227 (42000) 문제 원인과 해결방법" style="width:770px;align:center">


## 해결 방법 2
두번째 해결 방법은 백업 파일 [**DEFINER=**]에 표시되어 있는 계정과 동일한 계정으로 복구를 진행하면 됩니다.

## 참고 URL
1. Cloud DB for MySQL 백업, 복구 가이드
	- <a href="https://guide.ncloud-docs.com/docs/database-database-5-4" target="_blank" style="word-break:break-all;">https://guide.ncloud-docs.com/docs/database-database-5-4</a>

2. GTID를 이용한 Mysql 복제 가이드
	- <a href="https://dev.mysql.com/doc/refman/5.7/en/replication-gtids.html" target="_blank" style="word-break:break-all;">https://dev.mysql.com/doc/refman/5.7/en/replication-gtids.html</a>

3. GTID를 이용한 MariaDB 복제 가이드
	- <a href="http://mariadb.com/kb/en/gtid/" target="_blank" style="word-break:break-all;">http://mariadb.com/kb/en/gtid/</a>

4. MySQL Stored Object Access Control : DEFINER 안내
	- <a href="https://dev.mysql.com/doc/refman/5.7/en/stored-objects-security.html" target="_blank" style="word-break:break-all;">https://dev.mysql.com/doc/refman/5.7/en/stored-objects-security.html</a>
