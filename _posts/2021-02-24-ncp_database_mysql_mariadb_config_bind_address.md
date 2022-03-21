---
date: 2021-02-24
last_modified_at: 2021-02-24
title: mysql, mariadb 외부접속을 위한 환경설정 bind-address 위치
categories:
  - 5.database
description: 네이버 클라우드 mysql, mariadb 외부접속을 위한 환경설정 bind-address 위치
type: Document
set: database
order_number: 9
v2_link: /database/ncloud_database_mysql_mariadb_config_bind_address.html
---

## 개요
네이버 클라우드 DB중에서 mysql과 mariadb를 외부에서 접속하기 위해서는 여러 설정이 필요한데 그 중에서 bind-address 설정 항목이 어느 파일에 위치하고 있는지 정리해보겠습니다.  
OS중에서 CentOS는 기본 설정이 허용이지만, Ubuntu는 기본 설정이 localhost 만 접속 가능하도록 되어 있기 때문에 외부 접속을 허용해주기 위해서는 bind-address 설정을 수정해야 합니다.  
그래서 여기서는 Ubuntu에 대해서 살펴보겠습니다.

## mysql 5.6
mysql 5.6에서는 bind-address 설정이 **/etc/mysql/my.cnf** 파일에 있습니다.  
OS는 Ubuntu 14.04만 제공됩니다.

<img src="/images/ncp_database_mysql_mariadb_config_bind_address_01.jpg" alt="네이버 클라우드 mysql, mariadb 외부접속을 위한 환경설정 bind-address 위치" style="width:800px;align:center">

## mysql 5.7
mysql 5.7에서는 bind-address 설정이 **/etc/mysql/mysql.conf.d/mysqld.cnf** 파일에 있습니다.  
OS는 Ubuntu 14.04와 16.04 두 버전이 있는데 모두 동일합니다.

<img src="/images/ncp_database_mysql_mariadb_config_bind_address_02.jpg" alt="네이버 클라우드 mysql, mariadb 외부접속을 위한 환경설정 bind-address 위치" style="width:800px;align:center">
<img src="/images/ncp_database_mysql_mariadb_config_bind_address_03.jpg" alt="네이버 클라우드 mysql, mariadb 외부접속을 위한 환경설정 bind-address 위치" style="width:800px;align:center">

## mariaDB
mariaDB는 10.2 버전만 있으며 bind-address 설정이 **/etc/mysql/my.cnf** 파일에 있습니다.  
OS는 Ubuntu 16.04만 제공됩니다.

<img src="/images/ncp_database_mysql_mariadb_config_bind_address_04.jpg" alt="네이버 클라우드 mysql, mariadb 외부접속을 위한 환경설정 bind-address 위치" style="width:800px;align:center">

## 기타 - mariaDB CentOS
그 외에 mysql의 경우 CentOS는 개요에서 말씀드렸듯이 기본적으로 외부 접속을 차단하는 bind-address 항목이 존재하지 않는데
mariaDB의 경우 외부 접속을 차단하지는 않지만, bind-address 항목이 주석처리된 상태로 포함되어 있습니다.  
혹시나 차단하고 싶을 경우 사용하기 쉽게 미리 준비해둔 것으로 보입니다.
주석처리된 bind-address의 위치는 **/etc/my.cnf.d/server.cnf** 입니다.

<img src="/images/ncp_database_mysql_mariadb_config_bind_address_05.jpg" alt="네이버 클라우드 mysql, mariadb 외부접속을 위한 환경설정 bind-address 위치" style="width:800px;align:center">


## 참고 URL
- <a href="/5.database/ncp_database_mariadb_access_from_remote_ubuntu/" target="_blank" style="word-break:break-all;">Ubuntu에서 mariaDB 외부접속 허용, 원격접속하기 with HeidiSQL</a>
- <a href="/5.database/ncp_database_mariadb_access_from_remote_centos/" target="_blank" style="word-break:break-all;">CentOS에서 mariaDB 외부접속 허용, 원격접속하기 with HeidiSQL</a>
