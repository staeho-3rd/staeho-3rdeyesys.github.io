---
date: 2021-04-13
title: MYSQL(MARIADB) replication 생성하기
categories:
  - 5.database
description: 네이버 클라우드 MYSQL(MARIADB) replication 생성하기
type: Document
set: database
order_number: 10
---

## 선행조건

- master, slave 장비의 mysql설치가 사전에 완료된 조건
- mysql 버전 5.7이상 조건에서 작성됨
- mysql 리플리케이션작업을 진행시 masterdb의 데이터베이스는 쓰기작업이 없는 서비스 미진행 상태이어야함.

## MASTER 장비 구성

my.cnf파일 내용 추가

``` bash
[mysqld]
log-bin=mysql-bin
binlog_format = mixed

# 해당 ID값은 마스터장비만 1을 설정할수있음
server-id = 1 

# 마스터 장비의 bin로그 특정일자(예시는10일) 이후 삭제
# (해당 내역이없을 경우 지속적으로 기록되어 디스크 사용)
expire_logs_days = 10 
```

 mysql접속 후 리플리케이션을 진행할 계정 생성

``` bash
GRANT REPLICATION SLAVE ON *.* TO '리플리케이션계정명'@'%' IDENTIFIED BY '패스워드';
FLUSH PRIVILEGES;
```

## SLAVE 장비 구성

my.cnf파일 내용 추가

``` bash
[mysqld]
server-id=2 #해당ID값은 1을 제외한 숫자지정
slave-skip-errors = all  
```
<br />
## 데이터베이스 사전 동기화작업

1. master장비의 데이터베이스가 생성되어 있고 데이터가 있을 경우

	A. master장비 데이터베이스 백업 진행
``` bash
# 데이터베이스가 추가로 있다면 남은 데이터베이스도 백업
mysqldump -u root -p --databases 데이터베이스명 > 백업파일.sql 
```
<br />
	B. slave장비 데이터베이스 복구 진행
``` bash
create database 데이터베이스명; #mysql 접속 후 생성
mysql -u root -p 데이터베이스명 < 백업파일.sql  
```
<br />
2. master 장비의 데이터베이스가 없을 경우

	A. 별도 작업 없으며, 데이터베이스만 생성되어 데이터가 없을경우도 slave장비에 동일한 데이터베이스 생성으로 마무리 

## 리플리케이션 작업

1. master 장비 mysql 접속 후 현재 로그파일번호와 포지션 넘버 확인

   ``` bash
   show master status;
   file| Position #내역을 확인 후 별도 기입.
   ```

2. slave 장비 mysql 접속 후 아래 명령어 실행 하여 리플리케이션 master정보 기입

   ``` bash
   CHANGE MASTER TO MASTER_HOST='마스터IP',MASTER_USER='생성한리플리케이션계정명',MASTER_PASSWORD='패스워드',MASTER_LOG_FILE='위에서확인된 file이름',MASTER_LOG_POS=위에서 확인된 포지션번호;
   ``` 

3. slave 리플리케이션 시작및 확인

   ``` bash
   start slave;
   show slave status\G; #실행 후 에러가 없다면 정상.
   ```

> 문서 최종 수정일 : 2021-04-13  