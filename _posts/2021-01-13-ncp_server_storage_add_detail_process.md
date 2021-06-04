---
date: 2021-01-13
update: 2021-06-04
title: Linux 스토리지(디스크) 추가 상세 가이드
categories:
  - 1.compute
description: 네이버 클라우드 Linux 스토리지(디스크) 추가 상세 가이드
type: Document
set: server
order_number: 6
---


## 개요
네이버 클라우드에서 리눅스 서버에 디스크를 추가하는 것은 스토리지 즉, Block Storage를 생성해서 서버에 연결하는 작업이 필요합니다.

## 전체 과정 요약
``` bash

~# fdisk -l

~# fdisk /dev/xvdb

~# mkfs.xfs /dev/xvdb1

  - CentOS 5.x: mkfs.ext3 /dev/xvdb1
  - CentOS 6.x: mkfs.ext4 /dev/xvdb1
  - CentOS 7.x: mkfs.xfs /dev/xvdb1
  - Ubuntu : mkfs.ext4 /dev/xvdb1

~# mkdir /mnt/data

~# mount /dev/xvdb1 /mnt/data

~# df -k

~# vi /etc/fstab
### =============================
UUID=1fd5s61f5d-*** 중략 ***-f84ew13 /mnt/data xfs defaults 1 2

# 또는
/dev/xvdb1 /mnt/data ext4 defaults 1 2
### =============================
...

```

## 스토리지 생성, 할당
우선은 네이버 클라우드 콘솔 [Server] - [Server]에서 해당 서버를 선택하고 
[서버 관리 및 설정 변경] - [스토리지 생성]을 선택하거나  
[Server] - [Storage]에서 [스토리지 생성]을 클릭합니다.

<img src="../../images/ncp_server_storage_add_detail_process_01.jpg" alt="네이버 클라우드 Linux 스토리지(디스크) 추가 상세 가이드" style="width:770px;align:center">

<img src="../../images/ncp_server_storage_add_detail_process_02.jpg" alt="네이버 클라우드 Linux 스토리지(디스크) 추가 상세 가이드" style="width:770px;align:center">

다음으로 [스토리지 생성] 화면에서 스토리지 종류, 이름, 적용서버, 크기 (최소 10GB, 최대 2000GB) 등을 선택하고 [추가] 버튼을 클릭합니다.

<img src="../../images/ncp_server_storage_add_detail_process_03.jpg" alt="네이버 클라우드 Linux 스토리지(디스크) 추가 상세 가이드" style="width:770px;align:center">

앞에서 설정한 스토리지 정보를 다시 살펴보고 이상이 없으면 [확인] 버튼을 클릭합니다.

<img src="../../images/ncp_server_storage_add_detail_process_04.jpg" alt="네이버 클라우드 Linux 스토리지(디스크) 추가 상세 가이드" style="width:770px;align:center">

추가된 스토리지는 [Server] - [Server] 리스트에서 해당 서버를 클릭해 상세정보에서 확인하거나
[Server] - [Storage] 리스트에서 연결정보까지 포함해서 확인할 수 있습니다.

<img src="../../images/ncp_server_storage_add_detail_process_05.jpg" alt="네이버 클라우드 Linux 스토리지(디스크) 추가 상세 가이드" style="width:770px;align:center">

<img src="../../images/ncp_server_storage_add_detail_process_06.jpg" alt="네이버 클라우드 Linux 스토리지(디스크) 추가 상세 가이드" style="width:770px;align:center">

## 스토리지 할당 확인
네이버 클라우드 콘솔에서 할당한 스토리지를 확인하기 위해 putty를 실행해 서버에 접속합니다.  
이후 과정은 모두 서버에 접속한 상태에서 진행하게 됩니다.

fdisk -l 명령어를 실행해보면 아래 화면처럼 /dev/xvdb 디스크가 할당된 것을 확인할 수 있습니다.

``` bash
~# fdisk -l
```

<img src="../../images/ncp_server_storage_add_detail_process_07.jpg" alt="네이버 클라우드 Linux 스토리지(디스크) 추가 상세 가이드" style="width:660px;align:center">

## 디스크 파티션
다음 명령어를 입력해 할당된 디스크에 파티션을 생성합니다.  
파티션 설정에는 기본인 MBR 방식과 2TB 이상의 디스크를 인식하기 위해 사용하는 GPT 방식이 있는데, 네이버 클라우드는 최대 2,000GB까지만 지원하므로 여기서는 기본방식인 MBR을 사용하겠습니다.
``` bash
~# fdisk /dev/xvdb
```
<br />
#### 파티션 생성
파티션을 생성할 때는 여러 단계의 옵션이 있습니다. 일반적으로는 아래와 같은 단계로 진행하면 됩니다.

- 파티션을 새로 생성하기 위해 ‘n’을 입력

<img src="../../images/ncp_server_storage_add_detail_process_08-01.jpg" alt="네이버 클라우드 Linux 스토리지(디스크) 추가 상세 가이드" style="width:660px;align:center">

- 생성할 파티션 타입에 따라 primary type이면 ‘p’, extended type이면 ‘e’를 입력.  
  (primary type으로 생성하는 것이 일반적이며, primary 영역의 파티션이 부족할 경우 추가로 extended type으로 생성)

<img src="../../images/ncp_server_storage_add_detail_process_08-02.jpg" alt="네이버 클라우드 Linux 스토리지(디스크) 추가 상세 가이드" style="width:660px;align:center">

- 생성할 파티션 번호와 cylinder 영역을 입력 (일반적으로 추가할 disk 전체를 mount하게 되고, 이 경우 default값을 그대로 사용하므로 Enter 입력)

<img src="../../images/ncp_server_storage_add_detail_process_08-03.jpg" alt="네이버 클라우드 Linux 스토리지(디스크) 추가 상세 가이드" style="width:660px;align:center">

- ‘w’를 입력해 해당 구성을 적용. 파티션 생성 완료.

<img src="../../images/ncp_server_storage_add_detail_process_08-04.jpg" alt="네이버 클라우드 Linux 스토리지(디스크) 추가 상세 가이드" style="width:660px;align:center">

마지막으로 fdisk -l 명령어로 생성된 파티션을 다시 확인합니다.  
디스크가 /dev/xvdb1 장치로 인식된 것을 확인할 수 있습니다. 

<img src="../../images/ncp_server_storage_add_detail_process_09.jpg" alt="네이버 클라우드 Linux 스토리지(디스크) 추가 상세 가이드" style="width:660px;align:center">

## 디스크 포맷
다음으로 파티션이 생성된 디스크를 포맷하면 되는데, OS별로 명령어가 다르므로 확인 후에 실행해야 합니다.  
여기서는 CentOS 7.x 기준으로 mkfs.xfs 명령어를 사용했습니다.
``` bash

~# mkfs.xfs /dev/xvdb1

- CentOS 5.x: mkfs.ext3 /dev/xvdb1
- CentOS 6.x: mkfs.ext4 /dev/xvdb1
- CentOS 7.x: mkfs.xfs /dev/xvdb1
- Ubuntu : mkfs.ext4 /dev/xvdb1
```

## 디스크 마운트
다음으로 디스크를 마운트할 포인트 즉, 디렉토리를 원하는 이름으로 생성하고 마운트를 합니다.  
아래에 있는 마운트 경로 (/mnt/data)는 예시입니다. 원하는 경로를 직접 설정하시면 됩니다.
``` bash

~# mkdir /mnt/data
~# mount /dev/xvdb1 /mnt/data
```
<img src="../../images/ncp_server_storage_add_detail_process_11-01.jpg" alt="네이버 클라우드 Linux 스토리지(디스크) 추가 상세 가이드" style="width:660px;align:center">

<br />
마운트된 내역을 확인합니다.
``` bash
~# df -k
```
<img src="../../images/ncp_server_storage_add_detail_process_11-02.jpg" alt="네이버 클라우드 Linux 스토리지(디스크) 추가 상세 가이드" style="width:660px;align:center">

## 마운트 정보 등록
마운트 정보는 설정에 저장하지 않으면 서버가 리부팅될 때 사라지기 때문에 fstab에 저장합니다.  
마운트 정보를 등록할 때 장치명을 사용하는 방법과 장치의 UUID를 사용하는 방법이 있는데, 경우에 따라서는 장치명이 변경될 수도 있어, 이를 대비해 가능하면 UUID로 등록합니다.

#### UUID 확인
UUID를 확인하려면 blkid 명령어를 사용합니다.  
여기서 확인한 UUID를 별도로 복사해두었다가 fstab에 입력하게 됩니다.
``` bash
~# blkid /dev/xvdb1
```
<img src="../../images/ncp_server_storage_add_detail_process_12.jpg" alt="네이버 클라우드 Linux 스토리지(디스크) 추가 상세 가이드" style="width:660px;align:center">

vi로 /etc/fstab 파일을 열면 다음과 같습니다.  
서버 생성과 함께 장착된 기본 디스크도 UUID로 입력된 것을 확인할 수 있습니다.
``` bash
~# vi /etc/fstab
```
<img src="../../images/ncp_server_storage_add_detail_process_13.jpg" alt="네이버 클라우드 Linux 스토리지(디스크) 추가 상세 가이드" style="width:770px;align:center">

앞에서 확인하고 복사해둔 추가 디스크의 UUID와 기타 정보를 입력합니다.  
입력을 완료한 후 fstab 파일을 저장하고 빠져 나옵니다.  
(fstab에 입력할때 사용하는 디스크 정보 옵션에 대한 정리는 아래에서 다시 확인할 수 있습니다.)

``` bash
### /etc/fstab

UUID=29f58417-*** 중략 ***38d0f /mnt/data xfs defaults 1 2

# 또는
/dev/xvdb1 /mnt/data ext4 defaults 1 2

```
<img src="../../images/ncp_server_storage_add_detail_process_14.jpg" alt="네이버 클라우드 Linux 스토리지(디스크) 추가 상세 가이드" style="width:770px;align:center">


## fstab 설정 상세정보

/etc/fstab은 부팅 단계에서 마운트되어야 할 볼륨 정보들이 저장되는 곳입니다.  
(OS 이미지에 따라 파일 시스템이 다르기 때문에 주의해야 합니다.)

파일의 각 항목이 의미하는 바는 아래와 같으며 각 항목은 Tab 또는 Space Bar로 구분합니다.

(장치명) (마운트 포인트) (파일시스템 종류) (옵션) (dump 설정) (fsck 설정)

+ 장치명: 장치명은 장치의 UUID를 사용하거나 /dev/xvdb1와 같은 장치이름을 사용합니다.

+ 마운트 포인트: 볼륨을 마운트하고자 하는 위치입니다. 예시에서는 /mnt/data 디렉토리에 마운트했습니다.

+ 파일시스템 종류: OS별로 기본 파일시스템이 다르므로 알맞게 입력합니다.

  - CentOS 5.x : ext3
  - CentOS 6.x : ext4
  - CentOS 7.x : xfs
  - Ubuntu Server / Desktop : ext4

+ 옵션: 예시에서는 default 옵션을 사용하였으며, 해당 옵션에는 rw, nouser, auto, exec, suid 속성이 포함됩니다.  
  각 속성의 내용은 다음과 같습니다. (필요한 옵션만 사용할 시, 각 옵션을 쉼표(,)로 구분하여 작성해주시면 됩니다.)

  - auto : 부팅 시 자동 마운트
  - rw : 읽기, 쓰기 모두 가능하도록 마운트
  - nouser : root 계정만 마운트 가능
  - exec : 파일 실행을 허용
  - suid : SetUID와 SetGID를 허용

+ dump 설정: dump 명령으로 백업을 할 것인지에 대한 설정

  - 0: dump되지 않는 파일 시스템
  - 1: dump 가능한 파일 시스템

+ fsck 설정: 부팅시에 fsck 명령으로 파일시스템에 대한 무결성 검사를 할 것인지에 대한 설정

  - 0 : 부팅 시 fsck 실행하지 않음
  - 1 : 부팅 시 root 파일 시스템을 우선 체크
  - 2 : 부팅 시 root 이외의 파일 시스템을 우선 체크

## 참고 URL
<a href="https://guide.ncloud-docs.com/docs/compute-compute-4-1-v2" target="_blank" style="word-break:break-all;">https://guide.ncloud-docs.com/docs/compute-compute-4-1-v2.html</a>


> 문서 최종 수정일 : 2021-06-04