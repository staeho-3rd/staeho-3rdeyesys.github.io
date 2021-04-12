---
date: 2021-01-22
title: AWS CLI를 이용한 Object Storage 접속 방법
categories:
  - 4.storage
description: 네이버 클라우드 Object Storage를 AWS CLI를 이용해서 접속하는 방법
type: Document
set: storage
order_number: 3
---

## 개요
네이버 클라우드 Object Storage는 AWS의 스토리지 서비스 S3와 호환이 되도록 설계되어 있습니다.  
그래서 Object Storage에 접속, 관리할 때 AWS의 CLI(Command Line Interface)를 사용할 수 있는데 이번에 설치와 사용방법에 대해 정리해보겠습니다.

## pip 설치
aws cli를 설치하려면 pip가 먼저 설치되어 있어야 합니다.  
설치 방법은 CentOS와 Ubuntu가 다르니 각각의 OS에 맞게 설치하시면 됩니다.  
혹시 이미 pip가 설치되어 있다면 아래에 있는 <a href="#aws-cli-%EC%84%A4%EC%B9%98">AWS CLI설치</a>로 바로 이동하시면 되겠습니다.
``` bash
# CentOS
~# yum -y install python-pip

# Ubuntu
~# apt-get install python-pip
```
> **CentOS 6.x** 버전은 기술지원 종료로 인해 위 방법대로 설치가 되지 않습니다. 다음 경로에 나온 방법대로 설치하면 됩니다.
<a href="/1.compute/ncp_server_pip_python_install_centos6/" target="_blank" style="word-break:break-all;">CentOS6에서 pip - Python 설치하기</a>


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
위 단계에서 확인한 Access Key ID와 Secret Key를 아래 화면에서 입력하고 나머지 2가지 항목은 입력하지 않으셔도 됩니다.
``` bash
~# aws configure

AWS Access Key ID [None]: 3frEtFjfkdsj89243nkfv89s
AWS Secret Access Key [None]: 0kr23-0vsijr2390fw:L?K23-0vcdsjr2390fchnr123[]vl/fwsh
Default region name [None]: [Enter]
Default output format [None]: [Enter]
```

## Object Storage 접속
이제 Object Storage로 접속해보겠습니다. 얼핏 명령어만 보면 AWS에 접속하는 것처럼 보입니다. 그래서 네이버 클라우드로 접속하기 위한 --endpoint-url= 로 시작하는 옵션이 반드시 필요합니다.
``` bash
# s3 ls 명령으로 Object Storage에 존재하는 버킷 리스트를 조회합니다.
~# aws --endpoint-url=https://kr.objectstorage.ncloud.com s3 ls

# 로컬에 백업된 데이터를 Object Storage에 백업-동기화하는 명령어입니다.
~# aws --endpoint-url=https://kr.objectstorage.ncloud.com s3 sync /data_backup/ s3://data-back-up/
```

## 참고 URL
<a href="https://cli.ncloud-docs.com/docs/guide-objectstorage" target="_blank" style="word-break:break-all;">https://cli.ncloud-docs.com/docs/guide-objectstorage</a>


> 문서 최종 수정일 : 2021-01-22