---
date: 2021-07-30
last_modified_at: 2021-12-13
title: VPC환경에서 Cloud DB for MySQL 생성하기
categories:
  - 5.database
description: 네이버 클라우드 VPC환경에서 Cloud DB for MySQL 생성하기 가이드입니다
type: Document
set: database
order_number: 11
creator: ljh0519
v2_link: /database/ncloud_database_cloud_db_for_mysql_guide.html
---

## 개요
네이버 클라우드의 Cloud DB for MySQL 서비스는 MySQL 데이터베이스를 쉽고 간편하게 구축하고 관리할 수 있고 자동 Fail-Over, 자동백업, 네이버 서비스에서 검증된 최적화된 설정 등을 제공해주는 
완전 관리형 클라우드 데이터베이스 서비스입니다.  
여기서는 VPC환경에서 Cloud DB for MySQL 서비스를 생성하는 과정을 정리해보겠습니다.

{: .warning }
네이버 클라우드는 Classic환경에서는 DB 서버 이미지를 제공하지만, VPC 환경에서는 제공하지 않습니다. 
그러므로 VPC 환경에서 DB서버를 사용하려면 OS에 사용자가 직접 DB를 설치해서 사용하는 방법과 Cloud DB를 사용하는 방법 중에서 선택해야 합니다.

## 특징
- 기본 10GB 데이터 스토리지를 제공하며, 10GB 단위로 6,000GB까지 자동으로 용량이 증가합니다. 
- 하나의 마스터 DB마다 최대 10대의 슬레이브 DB를 생성할 수 있습니다.
- Load Balancer 상품을 통해 슬레이브 DB 서버들을 읽기 전용 복제본으로 사용함으로써 데이터베이스의 읽기 부하를 분산 시킬 수 있습니다.
- 매일 1회 고객이 원하는 시간에 DB를 자동으로 백업하며, 백업한 데이터를 최대 30일까지 보관할 수 있습니다.
- VPC 환경에서는 멀티 존으로 구성해 높은 안정성을 보장받을 수 있습니다.
- Cloud DB for MySQL 서비스는 완전 관리형 상품으로 사용자는 DB서버의 운영체제에 접근할 수 없습니다.

## DB 접속 방법 3가지
1. Private 도메인을 이용해 접속하는 방법
2. SSL VPN을 이용해 접속하는 방법
3. Public 도메인을 이용해 접속하는 방법

아래에서는 VPC환경에서 Private 도메인을 이용해 접속하는 방법을 설명하도록 하겠습니다.  
만약 네이버 클라우드 외부 환경에서  Cloud DB for MySQL로 접속하려면 Public 도메인을 사용해야 합니다.

## VPC-Subnet  생성

#### VPC 생성
VPC환경에서 작업할 것이므로 우선 VPC를 생성합니다.

<img src="/images/ncp_database_cloud_db_for_mysql_01.jpg" alt="네이버 클라우드 VPC환경에서 Cloud DB for MySQL 생성하기 가이드" style="width:770px;align:center">

<img src="/images/ncp_database_cloud_db_for_mysql_02.jpg" alt="네이버 클라우드 VPC환경에서 Cloud DB for MySQL 생성하기 가이드" style="width:680px;align:center">

#### Subnet 생성
Subnet은 Cloud DB for MySQL을 위한 Private Subnet과 DB 접속 테스트를 위한 Server용 Public Subnet을 각각 생성합니다.

<img src="/images/ncp_database_cloud_db_for_mysql_03.jpg" alt="네이버 클라우드 VPC환경에서 Cloud DB for MySQL 생성하기 가이드" style="width:770px;align:center">

#### Cloud DB for MySQL을 위한 Private Subnet
<img src="/images/ncp_database_cloud_db_for_mysql_05.jpg" alt="네이버 클라우드 VPC환경에서 Cloud DB for MySQL 생성하기 가이드" style="width:680px;align:center">

#### 테스트용 Server를 위한 Public Subnet
<img src="/images/ncp_database_cloud_db_for_mysql_04.jpg" alt="네이버 클라우드 VPC환경에서 Cloud DB for MySQL 생성하기 가이드" style="width:680px;align:center">

## 테스트 Server 생성
DB 서버 접속을 테스트 하기 위한 Server를 생성합니다.

<img src="/images/ncp_database_cloud_db_for_mysql_06.jpg" alt="네이버 클라우드 VPC환경에서 Cloud DB for MySQL 생성하기 가이드" style="width:770px;align:center">

## DB 서버 생성
[Database] - [Cloud DB for MySQL]에서 [DB Server 생성] 버튼을 클릭합니다.

<img src="/images/ncp_database_cloud_db_for_mysql_11.jpg" alt="네이버 클라우드 VPC환경에서 Cloud DB for MySQL 생성하기 가이드" style="width:670px;align:center">

#### DB 서버 엔진 버전
DB 엔진 버전은 MySQL 최신 버전 중 네이버에서 안정성이 검증된 버전인 5.7버전과 8.0버전을 제공합니다. (기본값 5.7.32)

<img src="/images/ncp_database_cloud_db_for_mysql_12-1.jpg" alt="네이버 클라우드 VPC환경에서 Cloud DB for MySQL 생성하기 가이드" style="width:770px;align:center">

#### DB 서버 이름과 DB 서비스 이름
- DB Server 이름은 고객이 DB 서버를 구분하기 위한 명칭으로, 사용자가 입력한 이름 뒤에 001, 002와 같이 숫자를 붙여 서버를 구분하게 됩니다.
- 예를 들어 DB 서버 이름을 mydb라고 입력하면 생성되는 DB 서버 이름은 mydb-001, mydb-002와 같습니다.
- DB 서비스 이름은 역할별 DB 서버를 구분하기 위한 이름입니다.
- 일반적으로 하나의 액티브 마스터 DB, 스탠바이 마스터 DB, 다수의 슬레이브 DB로 구성되는 DB 서버군을 말하며, 동일한 데이터를 갖고 있는 DB 서버들을 하나의 DB 서비스라 말합니다.
- 예를 들어 "쇼핑 메인 DB", "게임 유저 DB"와 같은 식으로 DB 서비스의 역할을 구분하기 위해 사용합니다.

<img src="/images/ncp_database_cloud_db_for_mysql_12-2.jpg" alt="네이버 클라우드 VPC환경에서 Cloud DB for MySQL 생성하기 가이드" style="width:770px;align:center">

{: .success }
Cloud DB를 위한 ACG는 자동 생성됩니다(예: cloud-mysql-*)

#### DB 서버 설정
DB 이름과 계정. 비번, 접속 포트 등을 설정합니다.  
HOST(IP) 설정에는 DB에 접근을 허용할 IP대역을 입력합니다. 여기서는 테스트용 서버의 Subnet 대역을 모두 허용하기 위해 [192.168.2.%]를 입력합니다.  
만약 특정 서버 1대만 허용하려고 할 경우에는 앞에서 생성한 테스트 서버 IP처럼 [192.168.2.6]을 입력합니다.

{: .error.box }
DB 접속포트는 한번 설정하면 이후에 변경할 수 없으니 신중하게 설정하셔야 합니다.

<img src="/images/ncp_database_cloud_db_for_mysql_12-3.jpg" alt="네이버 클라우드 VPC환경에서 Cloud DB for MySQL 생성하기 가이드" style="width:770px;align:center">


## ACG 설정
Cloud DB for MySQL을 생성할 때 자동 생성된 ACG [cloud-mysql-*]을 선택하고 ACG 설정을 클릭합니다.

<img src="/images/ncp_database_cloud_db_for_mysql_14.jpg" alt="네이버 클라우드 VPC환경에서 Cloud DB for MySQL 생성하기 가이드" style="width:770px;align:center">

Inbound 설정에 테스트용 Server의 Subnet 대역인 192.168.2.0/24를 접근소스에 입력합니다.  
또는 특정 서버 1대만 허용하려고 할 경우에는 앞에서 생성한 테스트 서버 IP처럼 [192.168.2.6]을 입력합니다.

<img src="/images/ncp_database_cloud_db_for_mysql_15.jpg" alt="네이버 클라우드 VPC환경에서 Cloud DB for MySQL 생성하기 가이드" style="width:770px;align:center">



## MySQL Client 설치
DB 접속 테스트를 위해 생성한 서버에서 MySQL Client를 설치합니다.

``` bash
# mariadb
~# yum install -y mysql mysql-server

# mysql 5.7
~# yum install -y https://dev.mysql.com/get/mysql80-community-release-el7-3.noarch.rpm
~# yum install -y --disablerepo="mysql80-community" --enablerepo="mysql57-community" mysql-community-server
~# mysqld --initialize-insecure --user=mysql
~# systemctl start mysqld
```
{: .info }
CentOS 7부터는 yum으로 설치하는 MySQL의 기본 데이터베이스가 MariaDB로 변경되었습니다

<img src="/images/ncp_database_cloud_db_for_mysql_19.jpg" alt="네이버 클라우드 VPC환경에서 Cloud DB for MySQL 생성하기 가이드" style="width:770px;align:center">


## DB 서버 접속
Cloud DB for MySQL에 접속하기 위한 주소인 [Private 도메인]을 확인 합니다.

<img src="/images/ncp_database_cloud_db_for_mysql_13.jpg" alt="네이버 클라우드 VPC환경에서 Cloud DB for MySQL 생성하기 가이드" style="width:770px;align:center">

테스트용 Server에서 Cloud DB for MySQL로 접속하는 방법은 다음과 같습니다.

``` bash
~# mysql -h [Private 도메인명] -u [user_id] -p
```
<br />
DB에 접속해보면 처음 Cloud DB for MySQL 생성할 때 입력한 테이터베이스 [test]를 확인할 수 있습니다.

<img src="/images/ncp_database_cloud_db_for_mysql_18.jpg" alt="네이버 클라우드 VPC환경에서 Cloud DB for MySQL 생성하기 가이드" style="width:650px;align:center">

## DB 서버 상세보기
DB 서버 상세보기 메뉴에서는 [Process list], [Variables], [Status], [Database 관리], [DB Config 관리], [DB User 관리], [Backup 설정 관리], [DB Server Logs] 등을 확인할 수 있습니다.

<img src="/images/ncp_database_cloud_db_for_mysql_16.jpg" alt="네이버 클라우드 VPC환경에서 Cloud DB for MySQL 생성하기 가이드" style="width:770px;align:center">

<img src="/images/ncp_database_cloud_db_for_mysql_17.jpg" alt="네이버 클라우드 VPC환경에서 Cloud DB for MySQL 생성하기 가이드" style="width:770px;align:center">


#### DB User 관리
DB 서버를 이용하다보면 여러 계정이 필요하게 됩니다. 이때 계정을 추가하기 위해 [DB 서버 상세보기] - [DB User 관리] 메뉴를 클릭합니다. 

USER_ID, HOST, DB 권한, 암호를 입력하고 DB User 추가 버튼을 클릭합니다.  
- 사용자가 변경한 DB 계정은 DB 서비스 전체에 적용됩니다.
- USER_ID + HOST(IP) 단위로 계정 추가 및 권한 관리를 합니다.
- DB 권한에서 DDL 권한은 CRUD 권한을 포함합니다.
- 최대 1,000개까지 계정을 추가 및 조회 할 수 있습니다.

<img src="/images/ncp_database_cloud_db_for_mysql_20.jpg" alt="네이버 클라우드 VPC환경에서 Cloud DB for MySQL 생성하기 가이드" style="width:770px;align:center">


## 참고 URL
1. Cloud DB for MySQL 기본 가이드
	- <a href="https://guide.ncloud-docs.com/docs/database-database-5-6" target="_blank" style="word-break:break-all;">https://guide.ncloud-docs.com/docs/database-database-5-6</a>

2. Cloud DB 서버 외부 접근 가이드
	- <a href="https://guide.ncloud-docs.com/docs/database-database-5-10" target="_blank" style="word-break:break-all;">https://guide.ncloud-docs.com/docs/database-database-5-10</a>
