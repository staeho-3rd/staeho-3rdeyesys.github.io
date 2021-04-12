---
date: 2021-02-19
title: Ubuntu에서 mariaDB 외부접속 허용, 원격접속하기 with HeidiSQL
categories:
  - 5.database
description: 네이버 클라우드 Ubuntu에서 mariaDB 외부접속 허용, 원격접속하기 with HeidiSQL
type: Document
set: database
order_number: 5
---

## 개요
네이버 클라우드 Ubuntu에서 mariaDB 외부접속을 허용하고, mariaDB용 클라이언트 HeidiSQL을 이용해서 원격접속하는 방법을 정리해보겠습니다.  
여기서 원격접속이라 함은 SSH의 Tunnels를 이용하지 않고, 외부 클라이언트 등을 이용한 직접 접속을 뜻합니다.

## 계정 비밀번호 생성
여기서는 네이버 클라우드에서 서버를 생성했을 때 자동으로 설정되는 root 계정을 이용한 방법을 정리하게 됩니다.   
네이버 클라우드에서는 처음 mariaDB를 설치하면 root 계정에 비밀번호가 설정되어 있지 않습니다.

``` bash
# mariadb 실행
~# mysql

# 비밀번호 설정
MariaDB [(None)]> set password=password('비밀번호');
```

## 계정 권한 부여
외부에서 해당 계정(여기서는 root)으로 접속할 수 있도록 계정에 권한을 부여하는 쿼리입니다.
``` bash
MariaDB [(None)]> GRANT ALL PRIVILEGES ON *.* to 'root'@'%' IDENTIFIED BY '비밀번호';
```

## 환경설정 파일 수정
mariaDB의 환경설정 파일 위치는 /etc/mysql/my.cnf 입니다.  
CentOS와 달리 Ubuntu에서 mariaDB는 기본적으로 외부 IP에 대한 접속이 차단되어 있고, 127.0.0.1 즉, localhost만 접속이 허용되어 있는 상태입니다.
``` bash
~# vi /etc/mysql/my.cnf
```
<br />
아래는 환경 설정 파일의 일부입니다.
``` bash
# MariaDB database server configuration file.
#
# 중략 #

[mysqld]
#
# * Basic Settings
#
port            = 3306
basedir         = /usr
datadir         = /var/lib/mysql
tmpdir          = /tmp
#
# Instead of skip-networking the default is now to listen only on
# localhost which is more compatible and is not less secure.
bind-address            = 127.0.0.1
#
```
<br />
위 환경설정 파일중에서 bind-address 항목을 주석처리하면 외부에서 접속이 가능합니다.
``` bash
# bind-address            = 127.0.0.1

# 다른 방법
bind-address            = 0.0.0.0

# 특정 IP만 허용
bind-address            = 허용할 IP 리스트
bind-address            = 192.168.1.1,10.0.0.1
```

## DB 재시작
``` bash
~# systemctl restart mysql.service
```

## ACG 포트 추가
네이버 클라우드 ACG에 mariaDB가 사용하는 포트 3306을 추가해줍니다.

<img src="../../images/ncp_database_mariadb_access_from_remote_ubuntu_04.jpg" alt="네이버 클라우드 Ubuntu에서 mariaDB 외부접속 허용, 원격접속하기 with HeidiSQL" style="width:800px;align:center">


## HeidiSQL 다운로드 
mariaDB용 클라이언트 중에서 대표적인 HeidiSQL을 사용합니다.

다운로드 경로 : <a href="https://www.heidisql.com/download.php" target="_blank" style="word-break:break-all;">https://www.heidisql.com/download.php</a>

<img src="../../images/ncp_database_mariadb_access_from_remote_ubuntu_01.jpg" alt="네이버 클라우드 Ubuntu에서 mariaDB 외부접속 허용, 원격접속하기 with HeidiSQL" style="width:800px;align:center">


## HeidiSQL 설정
HeidiSQL를 실행하면 DB접속을 위한 세션관리자가 먼저 나타납니다.  
왼쪽 하단의 [신규] 버튼을 누르고 서버 IP, 사용자, 암호를 입력하고 열기 버튼을 누르면 DB에 접속할 수 있습니다.

<img src="../../images/ncp_database_mariadb_access_from_remote_ubuntu_02.jpg" alt="네이버 클라우드 Ubuntu에서 mariaDB 외부접속 허용, 원격접속하기 with HeidiSQL" style="width:800px;align:center">

## DB 접속
mariaDB 접속에 성공하면 아래와 같은 화면을 볼 수 있습니다.

<img src="../../images/ncp_database_mariadb_access_from_remote_ubuntu_03.jpg" alt="네이버 클라우드 Ubuntu에서 mariaDB 외부접속 허용, 원격접속하기 with HeidiSQL" style="width:800px;align:center">


## 참고 URL
- <a href="/5.database/ncp_database_mariadb_access_from_remote_centos/" target="_blank" style="word-break:break-all;">CentOS에서 mariaDB 외부접속 허용, 원격접속하기 with HeidiSQL</a>
- <a href="https://guide.ncloud-docs.com/docs/database-database-7-1" target="_blank" style="word-break:break-all;">https://guide.ncloud-docs.com/docs/database-database-7-1.html</a>

> 문서 최종 수정일 : 2021-02-19