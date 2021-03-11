---
date: 2021-02-02
title: Object Storage 접속용 Windows Client Tool - S3 Browser
categories:
  - 4.storage
description: 네이버 클라우드 Object Storage 접속용 Windows Client Tool - S3 Browser
type: Document
set: storage
order_number: 5
---



## 개요
네이버 클라우드의 Object Storage에 접속, 관리하는 방법은 aws cli 등 여러가지 있지만 Windows PC에서 간편하게 접속해서 관리할 수 있는 클라이언트 툴이 몇개 있습니다.  
그 중에서 이번에는 S3 Browser의 사용법에 대해 간단히 정리해보겠습니다.

## S3 Browser
S3 Browser는 Amazon S3 and Amazon CloudFront를 위한 클라이언트입니다
이 버전은 무료버전이기는 하지만, 정확히는 **personal use only**, **non-commecial use only**라고 명시되어 있습니다.  
그래서 라이선스와 관계없이 사용 가능하고, 더 많은 기능이 포함된 유료 버전도 있습니다. 

상세한 정보와 다운로드는 아래 링크에서 확인하시면 됩니다.

<a href="https://s3browser.com/" target="_blank" style="word-break:break-all;">https://s3browser.com/</a>


## 사용법

프로그램을 다운 받아서 설치하고, 실행을 하면 여러 종류의 스토리지 중에서 원하는 것을 선택하게 됩니다.  
이 클라이언트는 AWS S3를 위한 것으로 CloudBerry Explorer와는 다르게 AWS의 다양한 S3 서비스들만 접속이 가능한데, 이용 가능한 스토리지 리스트는 마지막에서 정리해보겠습니다. 

<img src="../../images/ncp_storage_object_storage_s3_client_s3browser_01.jpg" alt="Object Storage 접속용 Windows Client Tool - S3 Browser" style="width:800px;align:center">


Account Type은  **S3 Compatible Storage**를 선택하면 됩니다.  
- Account name: 여기는 알아보기 쉬운 이름을 적으면 됩니다. 예를 들어 Naver Cloud
- REST Endpoint: 여기는 네이버 클라우드의 endpoint-url을 적습니다. **http://kr.objectstorage.ncloud.com**
- Access Key ID: 여기는 Access Key ID
- Secret Access Key: 여기는 Secret Key

<img src="../../images/ncp_storage_object_storage_s3_client_s3browser_02.jpg" alt="Object Storage 접속용 Windows Client Tool - S3 Browser" style="width:800px;align:center">

> 네이버 클라우드 포탈 -> 마이페이지 -> 계정관리 -> 인증키 관리 - API 인증키 관리 메뉴에서 Access Key ID와 Secret Key를 가져오셔야 하며, 아직 만들어진 Key가 없다면 새로 만드셔야 합니다.


계정 정보를 입력하고 접속을 하면 버킷 리스트가 나타나고 원하는 버킷을 선택해서 들어가면 다음처럼 파일들을 확인할 수 있습니다.

<img src="../../images/ncp_storage_object_storage_s3_client_s3browser_03.jpg" alt="Object Storage 접속용 Windows Client Tool - S3 Browser" style="width:800px;align:center">


Object Storage에 있는 파일을 로컬PC로 가져오려면 원하는 파일을 선택하고 마우스 오른쪽 버튼을 눌러서 **Download** 명령을 선택하면 됩니다.

<img src="../../images/ncp_storage_object_storage_s3_client_s3browser_04.jpg" alt="Object Storage 접속용 Windows Client Tool - S3 Browser" style="width:800px;align:center">


그 외 여러 가지 기능들이 있는데 그리 어려운 기능은 아니므로 직접 사용해보시면 금방 알 수 있습니다.

마지막으로 S3 Browser로 접속 가능한 S3 리스트를 확인해보겠습니다.

<img src="../../images/ncp_storage_object_storage_s3_client_s3browser_05.jpg" alt="Object Storage 접속용 Windows Client Tool - S3 Browser" style="width:800px;align:center">

- Amazon S3 Storage
- S3 Compatible Storage
- Amazon S3 in China
- Amazon S3 GovCloud Storage
- Amazon S3 GovCloud Storage (FIPS 140-2)
- Amazon S3 via EC2 IAM Role
- Amazon S3 via AssumeRole
- Amazon S3 (Credentials from Environment Variables)
- Amazon S3 (Credentials from AWS Config or Credential file)


## CloudBerry Explorer
또 다른 클라이언트인 CloudBerry Explorer입니다. 자세한  내용은 아래 링크에서 확인 가능합니다.

<a href="/4.storage/ncp_storage_object_storage_s3_client_cloudberry_explorer/" target="_blank" style="word-break:break-all;">Object Storage 접속용 Windows Client Tool - CloudBerry Explorer</a>

> 문서 최종 수정일 : 2021-02-02