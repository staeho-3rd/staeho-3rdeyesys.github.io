---
date: 2021-02-22
last_modified_at: 2021-02-22
title: mysql, mariadb 환경설정 파일 my.cnf 위치
categories:
  - 5.database
description: 네이버 클라우드 mysql, mariadb 환경설정 파일 my.cnf 위치
type: Document
set: database
order_number: 8
v2_link: /database/ncloud_database_mysql_mariadb_config_my_cnf.html
---

## 개요
네이버 클라우드 DB중에서 mysql과 mariadb의 환경설정 파일인 my.cnf 파일이 OS 별로 어떤 경로에 위치하고 있는지 정리해보겠습니다.

## CentOS
CentOS에서는 my.cnf 파일이 /etc/ 바로 밑에 위치합니다.
``` bash
/etc/my.cnf
```
<br />
#### mysql
<img src="../../images/ncp_database_mysql_mariadb_config_my_cnf_01.jpg" alt="네이버 클라우드 mysql, mariadb 환경설정 파일 my.cnf 위치" style="width:500px;align:center">

#### mariadb
<img src="../../images/ncp_database_mysql_mariadb_config_my_cnf_03.jpg" alt="네이버 클라우드 mysql, mariadb 환경설정 파일 my.cnf 위치" style="width:500px;align:center">

## Ubuntu 
Ubuntu에서는 my.cnf 파일이 /etc/mysql 밑에 위치합니다.
``` bash
/etc/mysql/my.cnf
```
<br />
#### mysql
<img src="../../images/ncp_database_mysql_mariadb_config_my_cnf_02.jpg" alt="네이버 클라우드 mysql, mariadb 환경설정 파일 my.cnf 위치" style="width:500px;align:center">

#### mariadb
<img src="../../images/ncp_database_mysql_mariadb_config_my_cnf_04.jpg" alt="네이버 클라우드 mysql, mariadb 환경설정 파일 my.cnf 위치" style="width:500px;align:center">


## 참고 URL
- <a href="https://guide.ncloud-docs.com/docs/database-database-1-1" target="_blank" style="word-break:break-all;">https://guide.ncloud-docs.com/docs/database-database-1-1.html</a>
- <a href="https://guide.ncloud-docs.com/docs/database-database-7-1" target="_blank" style="word-break:break-all;">https://guide.ncloud-docs.com/docs/database-database-7-1.html</a>
