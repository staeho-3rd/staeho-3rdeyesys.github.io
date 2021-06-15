---
date: 2021-01-22
update: 2021-06-11
title: Ubuntu에서 mysql DB를 Object Storage로 자동 백업하기
categories:
  - 5.database
description: 네이버 클라우드 mysql DB를 Object Storage로 자동 백업하기 - Ubuntu버전입니다
type: Document
set: database
order_number: 4
---

## 개요
네이버 클라우드 Ubuntu에서 설치형 mysql  DB를 매일 일정한 시간에 Object Storage로 자동으로 백업 받는 방법에 대해 정리해보았습니다.  
순서는 로컬에 백업 파일 생성 후에 Object Storage로 저장하는 단계로 진행됩니다.

## 백업 폴더 생성
``` bash
~# mkdir /data_backup
~# mkdir /data_backup/db
```

## mysql DB 로컬 백업 스크립트 작성
우선은 mysql DB를 로컬에 백업 받는 스크립트를 작성합니다.
``` bash
~# vi /bin/db_backup.sh

#!/bin/bash
DATE=$(date +%Y%m%d%H%M%S)
BACKUP_DIR=/data_backup/db/
# 전체 DB를 백업할 경우
mysqldump -u root -p디비패스워드 --all-databases > $BACKUP_DIR"backup_"$DATE.sql

# 특정 DB를 백업할 경우
# mysqldump -u root -p디비패스워드 --databases DB명  > $BACKUP_DIR"backup_"$DATE.sql

find $BACKUP_DIR -ctime +7 -exec rm -f {} \;

# 백업 파일을 202001224505 와 같은 형식으로 저장하고, 생성된지 7일이 지난 백업 파일을 삭제하는 코드입니다.
```
<br />
``` bash
# 백업 스크립트에 실행 권한을 부여합니다.
~# chmod 755 /bin/db_backup.sh
```

## pip 설치
aws cli를 설치하려면 pip가 먼저 설치되어 있어야 합니다.  
혹시 이미 pip가 설치되어 있다면 아래에 있는 <a href="#aws-cli-%EC%84%A4%EC%B9%98">AWS CLI설치</a>로 바로 이동하시면 되겠습니다.
``` bash
~# apt-get install python-pip
```

## AWS CLI 설치
네이버 클라우드의 설명에 따르면 aws cli 1.16이후 버전은 일부 기능을 사용할 수 없어서 1.15버전을 사용한다고 합니다.
``` bash
~# pip install awscli==1.15.85
```

## API 인증키 생성
네이버 클라우드 포탈 -> 마이페이지 -> 계정관리 -> 인증키 관리 - API 인증키 관리 메뉴에서 Access Key ID와 Secret Key를 가져오셔야 하며, 아직 만들어진 Key가 없다면 새로 만드셔야 합니다.

<img src="../../images/ncp_storage_object_storage_api_key_01.jpg" alt="AWS CLI를 이용한 Object Storage 접속 방법" style="width:800px;align:center">

## AWS CLI 환경 설정
이제 AWS CLI로 접속하기 위해 환경설정을 해야 합니다.  
위 단계에서 확인한 Access Key ID와 Secret Key를 아래 화면에서 입력하고 나머지 2가지 항목을 입력하지 않으셔도 됩니다.
``` bash
~# aws configure

AWS Access Key ID [None]: 3frEtFjfkdsj89243nkfv89s
AWS Secret Access Key [None]: 0kr23-0vsijr2390fw:L?K23-0vcdsjr2390fchnr123[]vl/fwsh
Default region name [None]: [Enter]
Default output format [None]: [Enter]
```

## Object Storage Bucket 생성
Object Storage에 data-back-up Bucket을 생성하고 그 아래에 db 폴더를 생성합니다.

<img src="../../images/ncp_database_mysql_backup_to_object_storage_01.jpg" alt="AWS CLI를 이용한 Object Storage 접속 방법" style="width:800px;align:center">

## Object Storage 접속 테스트
이제 Object Storage로 접속해보겠습니다. 얼핏 명령어만 보면 AWS에 접속하는 것처럼 보입니다. 그래서 네이버 클라우드로 접속하기 위한 --endpoint-url= 로 시작하는 옵션이 반드시 필요합니다.
``` bash
# s3 ls 명령으로 Object Storage에 존재하는 버킷 리스트를 조회합니다.
~# aws --endpoint-url=https://kr.objectstorage.ncloud.com s3 ls

2021-01-21 15:34:07 data-back-up
```

## mysql DB 백업 스크립트 수정
이제 위 단계에서 만들었던 DB 로컬 백업 스크립트에 DB백업 파일을 Object Storage로 백업-동기화 하는 명령을 추가하겠습니다.
``` bash
~# vi /bin/db_backup.sh

#!/bin/bash
DATE=$(date +%Y%m%d%H%M%S)
BACKUP_DIR=/data_backup/db/
# 전체 DB를 백업할 경우
mysqldump -u root -p디비패스워드 --all-databases > $BACKUP_DIR"backup_"$DATE.sql

# 특정 DB를 백업할 경우
# mysqldump -u root -p디비패스워드 --databases DB명  > $BACKUP_DIR"backup_"$DATE.sql

find $BACKUP_DIR -ctime +7 -exec rm -f {} \;

# 여기서부터 추가되는 명령입니다.
# 로컬에 백업된 데이터를 Object Storage에 백업-동기화하는 명령어입니다.
# aws 명령어를 crontab에서 실행하기 위해 aws 파일의 전체 경로를 적어줍니다
/usr/local/bin/aws --endpoint-url=https://kr.objectstorage.ncloud.com s3 sync /data_backup/ s3://data-back-up/
```
> 여기서 db 폴더만 백업-동기화를 하는 것이 아닌 상위 폴더인 /data_backup/ 폴더부터 백업-동기화를 한 이유는 이후에 db 말고도 개발소스 파일 등도 압축해서 백업하기 위해서입니다.

## 스케쥴링을 위한 crontab 설정
이제 마지막으로 완성된 스크립트를 일정한 시간, 여기서는 매일 새벽 6시에 실행되도록 설정합니다.
``` bash
~# crontab -e

# 매일 새벽 6시에 백업이 진행되는 코드입니다.
00 06 * * * /bin/db_backup.sh > /dev/null 2>&1
```
> crontab에 > /dev/null 2>&1를 추가하지 않으면 /var/log/syslog 파일에 (CRON) info (No MTA installed, discarding output) 라는 오류 메시지가 계속 쌓입니다.   
postfix를 설치하면 해결된다는 이야기도 있는데 굳이 필요하지 않은 것을 설치하기 보다는 필요하지 않은 오류 메시지는 없애는 것이 나을 듯합니다.

## 백업 결과 확인
백업이 진행되고 나면 아래와 같이 db 백업 파일이 Object Storage에 저장된 것을 확인할 수 있습니다.  
스샷에서는 빠른 확인을 위해 새벽 6시가 아닌 5분 단위로 백업한 내역입니다.
<img src="../../images/ncp_database_mysql_backup_to_object_storage_02.jpg" alt="AWS CLI를 이용한 Object Storage 접속 방법" style="width:800px;align:center">

## 참고 URL
1. mysql DB 자동백업 방법
	- <a href="/5.database/ncp_database_mysql_auto_backup/" target="_blank" style="word-break:break-all;">/5.database/ncp_database_mysql_auto_backup/</a>

2. AWS CLI를 이용한 Object Storage 접속 방법
	- <a href="/4.storage/ncp_storage_object_storage_aws_cli_connect/" target="_blank" style="word-break:break-all;">/4.storage/ncp_storage_object_storage_aws_cli_connect/</a>


> 문서 최종 수정일 : 문서 최종 수정일 : 2021-06-11