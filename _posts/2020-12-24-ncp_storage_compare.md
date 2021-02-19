---
date: 2020-12-24
title: 스토리지 비교
categories:
  - 4.storage
description: 네이버 클라우드 스토리지 비교
type: Document
set: storage
order_number: 1
---

## 개요
네이버 클라우드에서 제공하는 스토리지들의 주요 기능과 용도를 QnA 형식으로 비교 정리해보겠습니다.

## 비교 대상 스토리지
- Block Storage
- Object Storage
- NAS
- Archive Storage

## 가격 비교

| 스토리지 | 구분 | 과금 단위 | 시간 당 요금 | 500G 기준 요금 | 기타 사항|
| :----: | :----: | -----: | ----: | -----: | :----: |
| Block Storage | HDD | 10G | 0.8원 | 40원 | |
|  | SDD | 10G | 1.6원 | 80원 | |
| NAS | | 500G | 50원 | 50원 | |
| Object Storage | 1PB 이하 | 1G | 0.039원 | 19.5원 | 트래픽, API요청수 요금 별도 |
|  | 1PB 초과 | 1G | 0.036원 | 18원 | 트래픽, API요청수 요금 별도 |
| Archive Storage |  | 1G | 0.0076원 | 3.8원 | 트래픽, API요청수 요금 별도 |


## QnA
#### 서버에 디스크를 추가하고 싶을 때는 어떤 스토리지를 사용하면 되나요?
Block Storage를 사용하면 됩니다.  
Console - Server - 서버 상세 정보 - 스토리지 생성 메뉴에서 스토리지를 추가하고 서버에 마운트해서 사용하시면 됩니다.  

#### 서버당 디스크는 최대 얼마까지 추가할 수 있나요?
네이버 클라우드에서 서버에 직접 추가되는 디스크는 Block Storage로 최대 2,000GB를 지원하며, 서버 1대당 최대 16개의 스토리지를 추가할 수 있습니다.

#### 디스크를 추가할 수 없는 서버도 있나요?
네이버 클라우드에서 서버에 직접 추가되는 디스크는 Block Storage로 Micro 타입의 서버, Bare Metal 서버, Application Server Launcher는 Block Storage를 추가할 수 없습니다.


#### 여러 서버에서 공용으로 사용할 스토리지가 필요합니다.
NAS 서비스를 이용하시면 됩니다.  
서버 간 데이터 공유, 대용량 스토리지, 유연한 용량 확대/축소, 스냅샷 백업 등 NAS 서비스의 주요 기능을 활용해 안전하고 편리하게 데이터를 관리할 수 있습니다.  
특히, 프로토콜에 따른 인증 설정으로 높은 보안성을 제공하고, 이중화된 Controller 및 Disk Array Raid 구성으로 강력한 서비스 안정성을 확보하고 있습니다.

#### 유저가 업로드 하는 이미지를 저장하고 싶습니다.
Block Storage, Object Storage, NAS 모두 가능합니다만 용도에 따라 선택하시면 되겠습니다.  
매우 빠른 응답 속도가 필요하면 Block Storage.  
저렴한 비용과 여러 서버에서 동시에 이미지를 저장해야 한다면 Object Storage.  

#### 백업 자료를 오랜기간 보관해 두고 싶습니다.
Archive Storage를 이용하시면 됩니다.  
Archive Storage는 높은 내구성과 저렴한 비용이 특징인 데이터 아카이빙 및 장기 백업에 최적화된 스토리지 서비스입니다.

#### AWS S3와 비슷한 스토리지는 어떤 건가요?
Object Storage입니다.  
네이버 클라우드의 Object Storage는 AWS의 S3에서 사용하는 API와 호환이 되므로 쉽게 사용하실 수 있습니다.

#### CDN를 서비스를 이용하려면 어떤 스토리지를 사용해야 하나요?
Object Storage를 사용하시면 됩니다.  
물론 CDN의 원본 서버로 설정할 수 있는 것은 자체 웹 서버 및 네이버 클라우드 Object Storage, Server 등이 있는데, 그 중에서도 Object Storage를 사용하시는 것이 가장 쉽고 안정적입니다.


## 참고 URL
<a href="https://docs.ncloud.com/ko/storage/storage-5-1.html" target="_blank" style="word-break:break-all;">https://docs.ncloud.com/ko/storage/storage-5-1.html</a>


> 문서 최종 수정일 : 2020-12-28