---
date: 2021-08-05
update: 2021-08-05
title: 설치형 MySQL DB에서 root Password 설정, 변경하는 방법
categories:
  - 5.database
description: 네이버 클라우드 설치형 MySQL DB에서 root 패스워드 설정, 변경하는 방법 가이드입니다
type: Document
set: database
order_number: 13
---

## 개요
네이버 클라우드에서는 MySQL Password 정책에 따라 처음 MySQL DB를 설치할 때 root의 초기 패스워드가 설정되지 않습니다. 
그러므로 보안 침해 방지를 위해 설치 후에 root Password를 설정한 후에 DB를 사용하는 것이 안전합니다.  
여기서는 Classic환경에서 MySQL 5.6과 5.7 버전의 root Password를 설정, 변경하는 방법에 대해 정리해보겠습니다.

## MySQL 버전
네이버 클라우드 Classic환경에서 지원하는 설치형 MySQL 버전은 5.6과 5.7 입니다.

<img src="../../images/ncp_database_mysql_root_password_update_00.jpg" alt="네이버 클라우드 설치형 MySQL DB에서 root 패스워드 설정, 변경하는 방법" style="width:770px;align:center">

>네이버 클라우드는 Classic환경에서는 DB 서버 이미지를 제공하지만, VPC 환경에서는 제공하지 않습니다. 
>그러므로 VPC 환경에서 DB서버를 사용하려면 OS에 사용자가 직접 DB를 설치해서 사용하는 방법과 Cloud DB를 사용하는 방법 중에서 선택해야 합니다.


## MySQL 5.6 설정
우선 mysql 5.6 DB서버를 생성한 후 root 계정의 패스워드 상태를 확인해봅니다. 그 이후에 패스워드를 설정합니다.

``` bash
#mysql 접속
~# mysql -u root

#계정 정보 조회
mysql> select host, user, password from mysql.user;

#패스워드 설정-변경
mysql> set password = password('패스워드');

#변경된 패스워드 조회
mysql> select host, user, password from mysql.user;
```
<br />
초기 패스워드가 설정되어있지 않기 때문에 -p 옵션을 사용하지 않고 바로 접속 해서 user 테이블에 있는 계정 정보를 확인해보면 password 값이 비어 있는 것을 확인할 수 있습니다.

<img src="../../images/ncp_database_mysql_root_password_update_01.jpg" alt="네이버 클라우드 설치형 MySQL DB에서 root 패스워드 설정, 변경하는 방법" style="width:770px;align:center">

패스워드를 설정하고 다시 확인을 해보면 **localhost의 root 계정**에 패스워드가 설정된 것을 확인할 수 있습니다.

>MySQL의 계정은 host-user 두개를 합쳐서 키로 사용하게 되어 있습니다. 같은 user라도 접속하는 host 값이 다르면 별도의 계정처럼 인식합니다.

<img src="../../images/ncp_database_mysql_root_password_update_02.jpg" alt="네이버 클라우드 설치형 MySQL DB에서 root 패스워드 설정, 변경하는 방법" style="width:770px;align:center">

그런데 위에서 사용한 **set password = password('패스워드')**는 localhost@root에 대해서만 패스워드를 설정-변경합니다.  
만약 외부에서 접속하기 위해 특정 IP를 추가했다거나 외부 접속을 모두 허용하기 위해 host에 %값을 설정했을 경우에는 패스워드가 설정-변경되지 않습니다. 
이때 사용하는 방법은 아래와 같습니다.

``` bash
mysql> update mysql.user set password = password('패스워드') where user = 'root';

mysql> select host, user, password from mysql.user;
```

쿼리를 실행한 후에 조회를 해보면 user값이 root인 모든 계정의 password 값이 동일하게 설정된 것을 확인할 수 있습니다.

<img src="../../images/ncp_database_mysql_root_password_update_03.jpg" alt="네이버 클라우드 설치형 MySQL DB에서 root 패스워드 설정, 변경하는 방법" style="width:770px;align:center">


## MySQL 5.7 설정
마찬가지로 mysql 5.7 DB서버를 생성한 후 root 계정의 패스워드 상태를 확인해봅니다. 그 이후에 패스워드를 설정합니다.  
MySQL 5.7은 패스워드 칼럼이 password가 아니고 authentication_string로 변경되었습니다.

``` bash
#mysql 접속
~# mysql -u root

#계정 정보 조회
mysql> select host, user, authentication_string from mysql.user;

#패스워드 설정-변경
mysql> ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '패스워드';

#변경된 패스워드 조회
mysql> select host, user, authentication_string from mysql.user;
```
<br />
초기 패스워드가 설정되어있지 않기 때문에 -p 옵션을 사용하지 않고 바로 접속 해서 user 테이블에 있는 계정 정보를 확인해보면 password 값이 비어 있는 것을 확인할 수 있습니다.

<img src="../../images/ncp_database_mysql_root_password_update_04.jpg" alt="네이버 클라우드 설치형 MySQL DB에서 root 패스워드 설정, 변경하는 방법" style="width:770px;align:center">

이때 MySQL 5.6 이하 버전처럼 update 명령어로 authentication_string 칼럼 값을 변경하려고 해도 적용되지 않습니다.  
위에 적은 것처럼 ALTER 명령으로 변경해야 적용되니 주의해야 합니다.

<img src="../../images/ncp_database_mysql_root_password_update_05.jpg" alt="네이버 클라우드 설치형 MySQL DB에서 root 패스워드 설정, 변경하는 방법" style="width:770px;align:center">


## MySQL 패스워드 정책
MySQL은 아래와 같이 기본 패스워드 정책이 설정되어 있습니다. 그러므로 패스워드 설정 시에는 아래 조건에 맞게 입력하셔야 합니다.

- 최소 길이 8자 이상
- 특수문자 1개 이상
- 숫자 1개 이상
- 대소문자 조합 1개 이상

MySQL DB에 접속해서 아래와 같이 설정된 정책을 조회할 수 있습니다.

``` bash
mysql> SHOW VARIABLES LIKE 'validate_password%';
```
<img src="../../images/ncp_database_mysql_root_password_update_06.jpg" alt="네이버 클라우드 설치형 MySQL DB에서 root 패스워드 설정, 변경하는 방법" style="width:770px;align:center">


## 참고 URL
1. 설치형 MySQL 기본 가이드
	- <a href="https://guide.ncloud-docs.com/docs/database-database-1-1" target="_blank" style="word-break:break-all;">https://guide.ncloud-docs.com/docs/database-database-1-1</a>

1. MySQL 패스워드 정책 가이드
	- <a href="https://dev.mysql.com/doc/refman/5.7/en/validate-password-options-variables.html" target="_blank" style="word-break:break-all;">https://dev.mysql.com/doc/refman/5.7/en/validate-password-options-variables.html</a>


> 문서 최종 수정일 : 2021-08-05