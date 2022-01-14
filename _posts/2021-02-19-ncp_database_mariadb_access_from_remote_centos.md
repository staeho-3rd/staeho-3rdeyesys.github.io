---
date: 2021-02-19
last_modified_at: 2021-02-19
title: CentOS에서 mariaDB 외부접속 허용, 원격접속하기 with HeidiSQL
image: ../../images/ncp_database_mariadb_access_from_remote_ubuntu_03.jpg
categories:
  - 5.database
description: 네이버 클라우드 CentOS에서 mariaDB 외부접속 허용, 원격접속하기 with HeidiSQL
type: Document
set: database
order_number: 5
---

## 개요
네이버 클라우드 CentOS에서 mariaDB 외부접속을 허용하고, mariaDB용 클라이언트 HeidiSQL을 이용해서 원격접속하는 방법을 정리해보겠습니다.  
여기서 원격접속이라 함은 SSH의 Tunnels를 이용하지 않고, 외부 클라이언트 등을 이용한 직접 접속을 뜻합니다.  
CentOS는 Ubuntu와 달리 mariaDB 환경설정 파일 my.cnf에서 외부 IP에서 접근이 막혀 있지 않기에 환경설정 파일은 수정하지 않습니다.

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

## ACG 포트 추가
네이버 클라우드 ACG에 mariaDB가 사용하는 포트 3306을 추가해줍니다.

<img src="../../images/ncp_database_mariadb_access_from_remote_ubuntu_04.jpg" alt="네이버 클라우드 Ubuntu에서 mariaDB 외부접속 허용, 원격접속하기 with HeidiSQL" style="width:800px;align:center">


## HeidiSQL 다운로드 
mariaDB용 클라이언트 중에서 대표적인 HeidiSQL을 사용합니다.

다운로드 경로 : <a href="https://www.heidisql.com/download.php" target="_blank" style="word-break:break-all;">https://www.heidisql.com/download.php</a>

<img src="../../images/ncp_database_mariadb_access_from_remote_ubuntu_01.jpg" alt="네이버 클라우드 CentOS에서 mariaDB 외부접속 허용, 원격접속하기 with HeidiSQL" style="width:800px;align:center">


## HeidiSQL 설정
HeidiSQL를 실행하면 DB접속을 위한 세션관리자가 먼저 나타납니다.  
왼쪽 하단의 [신규] 버튼을 누르고 서버 IP, 사용자, 암호를 입력하고 열기 버튼을 누르면 DB에 접속할 수 있습니다.

<img src="../../images/ncp_database_mariadb_access_from_remote_ubuntu_02.jpg" alt="네이버 클라우드 CentOS에서 mariaDB 외부접속 허용, 원격접속하기 with HeidiSQL" style="width:800px;align:center">

## DB 접속
mariaDB 접속에 성공하면 아래와 같은 화면을 볼 수 있습니다.

<img src="../../images/ncp_database_mariadb_access_from_remote_ubuntu_03.jpg" alt="네이버 클라우드 CentOS에서 mariaDB 외부접속 허용, 원격접속하기 with HeidiSQL" style="width:800px;align:center">


## 참고 URL
- <a href="/5.database/ncp_database_mariadb_access_from_remote_ubuntu/" target="_blank" style="word-break:break-all;">Ubuntu에서 mariaDB 외부접속 허용, 원격접속하기 with HeidiSQL</a>
- <a href="https://guide.ncloud-docs.com/docs/database-database-7-1" target="_blank" style="word-break:break-all;">https://guide.ncloud-docs.com/docs/database-database-7-1.html</a>
