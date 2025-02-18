---
date: 2021-04-12
last_modified_at: 2021-04-12
title: Object Storage 데이터를 Archive Storage로 자동으로 이동시키는 방법
categories:
  - 4.storage
description: 네이버 클라우드 Object Storage 데이터를 Archive Storage로 이동시키는 방법
type: Document
set: storage
order_number: 8
v2_link: /storage/ncloud_storage_object_storage_transfer_to_archive_storage.html
---

## 개요
네이버 클라우드 Object Storage는 일반 디스크인 Block Storage나 NAS에 비해 가격도 1/2 ~ 1/4정도이면서도 안정적이기 때문에 데이터 저장 특히 백업 용도로 많이 사용합니다.  
그럼에도 불구하고 많이 양의 데이터가 저장되면 비용에 대한 부담이 생길 수 밖에 없는데, 이럴 때 Archive Storage를 이용하면 Object Storage의 1/5정도로 비용이 줄어들기에 매우 효과적입니다.  
그래서 이번에는 Object Storage에 있는 데이터를 Archive Storage로 이동시키는 즉, 이관하는 방법에 대해 정리해보겠습니다.

## 스토리지 가격 비교
위 개요에서도 간단하게 설명했지만, Archive Storage는 Object Storage에 비해 비용이 1/5 정도입니다.  
이 두가지 뿐만 아니라 여러 스토리지들의 가격과 용도에 대한 비교는 아래 문서에서 확인할 수 있습니다.

<a href="/4.storage/ncp_storage_compare/" target="_blank" style="word-break:break-all;">https://docs.3rdeyesys.com/4.storage/ncp_storage_compare/</a>

## 스토리지 용도 구분
Object Storage와 Archive Storage 2가지 스토리지 모두 데이터를 백업하는 용도로 많이 사용됩니다.  
비슷하기는 하지만 전혀 다르기도 한 2가지 스토리지에 대한 용도를 간단하게 구분해보겠습니다.

#### Object Storage
- 데이터 저장, 삭제가 수시로 이루어지는 경우
- 저장된 데이터에 대한 조회가 빈번한 경우
- 앱을 사용하는 일반 유저들이 앱을 통해 데이터에 접근하는 경우
- 네이버 클라우드의 다른 서비스에서 데이터를 저장, 조회해야 하는 경우

#### Archive Storage
- 저장된 데이터에 대한 조회가 거의 없는 경우
- 당장 사용할 일은 없으나 오랜 기간 데이터를 저장해야 하는 경우
- 매우 저렴한 비용으로 데이터를 보관하고 싶은 경우

## 데이터 이관
Object Storage에 있는 데이터를 자동으로 Archive Storage로 이관하는 방법은 수명주기 관리(LifeCycle Mangement)를 이용하면 됩니다.  
[ **네이버 클라우드 콘솔 - Object Storage - Lifecycle Management - 수명주기 정책 추가** ] 기능을 이용하시면 설정할 수 있습니다.

<img src="/images/ncp_storage_object_storage_transfer_to_archive_storage_01.jpg" alt="네이버 클라우드 Object Storage 데이터를 Archive Storage로 이동시키는 방법" style="width:738px;align:center">

### 수명주기 정책 추가
수명주기 정책 추가 화면에서 정책 유형은 [이관] 또는 [이관 후 삭제], 관리대상은 Object Storage에 있는 대상 버킷, 이동 위치는 Archive Storage에 있는 버킷을 선택하시면 됩니다.  
이렇게 설정하시면 대상 버킷에 있는 데이터 중에서 이름 규칙에 해당하는 데이터가 Archive Storage로 이동하게 됩니다.

<img src="/images/ncp_storage_object_storage_transfer_to_archive_storage_02.jpg" alt="네이버 클라우드 Object Storage 데이터를 Archive Storage로 이동시키는 방법" style="width:650px;align:center">

#### 정책
정책 유형은 다음의 3가지가 있습니다.
- 만료 삭제 : 설정된 기간이 지난 파일을 삭제
- 이관 : 설정된 기간이 지난 파일을 Archive Storage로 이동
- 이관 후 삭제 : 설정된 기간이 지난 파일을 Archive Storage로 이동한 후 Object Storage에서 삭제

그리고 이동 시점은 파일이 Object Storage에 저장-생성된 후 경과한 일자를 기준으로 하며 1일 ~ 3,650일 사이의 값을 입력합니다.

<img src="/images/ncp_storage_object_storage_lifecycle_01.jpg" alt="네이버 클라우드 Object Storage Lifecycle Management의 관리대상(Source) Object 접두어 설정 방법" style="width:650px;align:center">

#### 관리대상 (Source)
관리대상의 버킷(Bucket)을 선택하고 Object 이름의 규칙을 접두어 방식으로 입력합니다.

<img src="/images/ncp_storage_object_storage_lifecycle_02.jpg" alt="네이버 클라우드 Object Storage Lifecycle Management의 관리대상(Source) Object 접두어 설정 방법" style="width:650px;align:center">

#### 이동위치 (Target)
이동할 위치는 Archive Storage로 고정이며, Archive Storage의 컨테이너(버킷)을 선택하고 세부경로 즉, 폴더를 입력합니다.  
세부경로에 아무것도 입력하지 않으면 Source 즉, Object Storage의 위치, 폴더 구조 그대로 이동됩니다.

<img src="/images/ncp_storage_object_storage_lifecycle_03.jpg" alt="네이버 클라우드 Object Storage Lifecycle Management의 관리대상(Source) Object 접두어 설정 방법" style="width:650px;align:center">

#### 정책 실행 시간
Lifecycle Management의 수명주기 정책 실행시간은 아래와 같습니다. 
 - 01:00~02:00,  07:00~08:00, 13:00~14:00, 19:00~20:00
 (※ 파일용량이 클 경우 일부 변동될 수 있음)

예시) 정책 유형(만료 삭제)﻿, 이동 시점(생성 후 1일)로 정책을 생성하고 대상 파일이 15시에 업로드 되었다면 정책실행은 익일 19~20시 사이에 이관 완료

## 관리대상 이름 규칙
관리대상인 Object를 이관하는 이름 규칙은 접두어 방식인데 자세한 규칙 설명은 다음 문서를 참고 하시면 됩니다.

<a href="/4.storage/ncp_storage_object_storage_lifecycle_management/" target="_blank" style="word-break:break-all;">https://docs.3rdeyesys.com/4.storage/ncp_storage_object_storage_lifecycle_management/</a>


## 참고 URL
1.  Object Storage 가이드
	- <a href="https://guide.ncloud-docs.com/docs/storage-storage-6-1" target="_blank" style="word-break:break-all;">https://guide.ncloud-docs.com/docs/storage-storage-6-1</a>
2.  CentOS에서 mysql DB를 Object Storage로 자동 백업하기
	- <a href="/5.database/ncp_database_mysql_object_storage_auto_backup_centos/" target="_blank" style="word-break:break-all;">/5.database/ncp_database_mysql_object_storage_auto_backup_centos/</a>
