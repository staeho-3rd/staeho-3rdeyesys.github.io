---
date: 2021-07-27
last_modified_at: 2021-07-27
title: NAS 볼륨을 생성하고 Linux 서버에 마운트하기 가이드
categories:
  - 4.storage
description: 네이버 클라우드 NAS 볼륨을 생성하고 Linux 서버에 마운트하기 가이드입니다
type: Document
set: storage
order_number: 9
creator: ljh0519
v2_link: /storage/ncloud_storage_nas_vpc_guide.html
---

## 개요
NAS (Network Attached Storage)는 다수의 서버, 사용자가 함께 사용하는 네트워크 저장공간으로, 
서버 간 데이터 공유, 대용량 스토리지, 유연한 용량 확대/축소, 스냅샷 백업 등이 필요한 경우에 주로 사용하며, 
네이버 클라우드 NAS 서비스의 주요 기능을 활용해 안전하고 편리하게 데이터를 관리할 수 있습니다.  
이번 가이드에서는 NAS 볼륨을 생성하고, Linux 즉, CentOS와 Ubuntu 서버에 마운트하는 방법을 정리해보겠습니다.

## 특징
- 용량: 500GB ~ 10,000GB까지 가능하며, 확장은 100GB단위로 가능
- 접근제어 설정 가능
- 스냅샷 설정: 자동생성의 경우  최대 7개까지 보관 가능
- 볼륨 암호화: 볼륨 단위로 AES-256 알고리즘 기반의 암호화 키를 사용하여 FIPS-140-2 레벨 1 수준의 암호화를 제공
- 모니터링 및 이벤트 설정 가능

## VPC-Subnet  생성

#### VPC 생성
VPC환경에서 작업할 것이므로 우선 VPC를 생성합니다.

<img src="../../images/ncp_storage_nas_guide_01.jpg" alt="네이버 클라우드 NAS 볼륨을 생성하고 Linux 서버에 마운트하기 가이드" style="width:770px;align:center">

<img src="../../images/ncp_storage_nas_guide_02.jpg" alt="네이버 클라우드 NAS 볼륨을 생성하고 Linux 서버에 마운트하기 가이드" style="width:680px;align:center">


#### Subnet 생성
Subnet 설정은 [Public]과 [일반]을 선택합니다.

<img src="../../images/ncp_storage_nas_guide_03.jpg" alt="네이버 클라우드 NAS 볼륨을 생성하고 Linux 서버에 마운트하기 가이드" style="width:770px;align:center">

<img src="../../images/ncp_storage_nas_guide_04.jpg" alt="네이버 클라우드 NAS 볼륨을 생성하고 Linux 서버에 마운트하기 가이드" style="width:680px;align:center">

## 서버 생성
NAS 볼륨을 마운트할 서버 2개를 CentOS 7.8과 Ubuntu 18.04로 생성합니다.

<img src="../../images/ncp_storage_nas_guide_05.jpg" alt="네이버 클라우드 NAS 볼륨을 생성하고 Linux 서버에 마운트하기 가이드" style="width:770px;align:center">

## NAS 생성
[NAS] - [Volume]에서 [NAS 볼륨 생성] 버튼을 클릭합니다.

<img src="../../images/ncp_storage_nas_guide_06.jpg" alt="네이버 클라우드 NAS 볼륨을 생성하고 Linux 서버에 마운트하기 가이드" style="width:770px;align:center">

NAS 볼륨 이름과 용량을 입력하고, 리눅스용 프로토콜인 NFS를 선택합니다.  CIFS는 윈도우용 프로토콜입니다.  
용량은 500GB ~ 10,000GB까지 가능하며, 100GB단위로 추가할 수 있습니다.

<img src="../../images/ncp_storage_nas_guide_07.jpg" alt="네이버 클라우드 NAS 볼륨을 생성하고 Linux 서버에 마운트하기 가이드" style="width:770px;align:center">

#### NFS 접근 제어 설정
NFS 접근 제어 설정에서는 NAS 볼륨을 마운트할 장비를 선택해서 ACL(네트워그 접근제어) 설정을 하게 됩니다.

<img src="../../images/ncp_storage_nas_guide_08.jpg" alt="네이버 클라우드 NAS 볼륨을 생성하고 Linux 서버에 마운트하기 가이드" style="width:770px;align:center">

NAS 볼륨을 마운트할 장비를 선택하고, [ > ] 버튼을 클릭해 오른쪽으로 이동시킵니다.
<img src="../../images/ncp_storage_nas_guide_09.jpg" alt="네이버 클라우드 NAS 볼륨을 생성하고 Linux 서버에 마운트하기 가이드" style="width:770px;align:center">

<img src="../../images/ncp_storage_nas_guide_10.jpg" alt="네이버 클라우드 NAS 볼륨을 생성하고 Linux 서버에 마운트하기 가이드" style="width:770px;align:center">

마지막으로 설정 내용을 확인하고 [볼륨 생성] 버튼을 클릭합니다.

<img src="../../images/ncp_storage_nas_guide_11.jpg" alt="네이버 클라우드 NAS 볼륨을 생성하고 Linux 서버에 마운트하기 가이드" style="width:770px;align:center">

## CentOS 설정

#### NFS 관련 패키지 설치
NAS 볼륨을 서버에 마운트하기 위해 우선 서버에 NFS 프로토콜 관련 패키지를 설치합니다.

``` bash
~# yum install nfs-utils
```

<img src="../../images/ncp_storage_nas_guide_12.jpg" alt="네이버 클라우드 NAS 볼륨을 생성하고 Linux 서버에 마운트하기 가이드" style="width:660px;align:center">

#### NAS 볼륨 마운트하기
NAS 볼륨을 마운트할 디렉토리를 생성하고 {NAS 볼륨 마운트 정보}를 이용해 마운트한 후에 상태를 확인합니다.  
네이버 클라우드에서는 안정성이 높은 NFS v3(-o vers=3)로 마운트하여 사용할 것을 권고하고 있습니다.

``` bash
~# mkdir /nas
~# mount -t nfs -o vers=3 {NAS 볼륨 마운트 정보} /nas
~# df -Th
```
<img src="../../images/ncp_storage_nas_guide_20.jpg" alt="네이버 클라우드 NAS 볼륨을 생성하고 Linux 서버에 마운트하기 가이드" style="width:660px;align:center">

<img src="../../images/ncp_storage_nas_guide_13.jpg" alt="네이버 클라우드 NAS 볼륨을 생성하고 Linux 서버에 마운트하기 가이드" style="width:660px;align:center">

#### fstab 설정
부팅 후에도 마운트가 될 수 있도록 /etc/fstab 파일에 추가합니다.

<img src="../../images/ncp_storage_nas_guide_14.jpg" alt="네이버 클라우드 NAS 볼륨을 생성하고 Linux 서버에 마운트하기 가이드" style="width:660px;align:center">

## Ubuntu 설정

#### NFS 관련 패키지 설치
우분투에서도 우선 NFS 관련 패키지를 설치합니다.

``` bash
~# apt-get install nfs-common -y
```
<img src="../../images/ncp_storage_nas_guide_15.jpg" alt="네이버 클라우드 NAS 볼륨을 생성하고 Linux 서버에 마운트하기 가이드" style="width:660px;align:center">

#### NAS 마운트하기
NAS 볼륨을 마운트할 디렉토리를 생성하고 {NAS 볼륨 마운트 정보}를 이용해 마운트한 후에 상태를 확인합니다.  
네이버 클라우드에서는 안정성이 높은 NFS v3(-o vers=3)로 마운트하여 사용할 것을 권고하고 있습니다.

``` bash
~# mkdir /nas
~# mount -t nfs -o vers=3 {NAS 볼륨 마운트 정보} /nas
~# df -Th
```
<img src="../../images/ncp_storage_nas_guide_16.jpg" alt="네이버 클라우드 NAS 볼륨을 생성하고 Linux 서버에 마운트하기 가이드" style="width:660px;align:center">

#### fstab 설정
부팅 후에도 마운트가 될 수 있도록 /etc/fstab 파일에 추가합니다.

<img src="../../images/ncp_storage_nas_guide_17.jpg" alt="네이버 클라우드 NAS 볼륨을 생성하고 Linux 서버에 마운트하기 가이드" style="width:660px;align:center">

## 이벤트 설정
이벤트 설정에서는 NAS 볼륨 사용량 임계치를 설정하고 이벤트 발생 시 SMS나 Email로 통보를 받습니다.  
볼륨 설정에서 이벤트 설정을 클릭합니다.

<img src="../../images/ncp_storage_nas_guide_18.jpg" alt="네이버 클라우드 NAS 볼륨을 생성하고 Linux 서버에 마운트하기 가이드" style="width:770px;align:center">

이벤트 통보 방법과 휴대폰 또는 이메일 등을 입력하고 설정을 완료합니다.

<img src="../../images/ncp_storage_nas_guide_19.jpg" alt="네이버 클라우드 NAS 볼륨을 생성하고 Linux 서버에 마운트하기 가이드" style="width:770px;align:center">


## 참고 URL
1.  NAS 사용 가이드
	- <a href="https://guide.ncloud-docs.com/docs/ko/storage-storage-4-1" target="_blank" style="word-break:break-all;">https://guide.ncloud-docs.com/docs/ko/storage-storage-4-1</a>
