---
date: 2021-07-06
last_modified_at: 2021-07-06
title: 리눅스서버 스토리지(디스크) LVM 구성하기
categories:
  - 1.compute
description: 네이버 클라우드 리눅스 서버의 Block Storage를 LVM으로 구성하는 방법입니다
type: Document
set: server
order_number: 19
creator: ljh0519
v2_link: /compute/ncloud_compute_server_storage_lvm_create.html
---


## 개요
네이버 클라우드 서버는 하나의 서버당 로컬 디스크 즉, 블록 스토리지(Block Storage)를 OS가 설치되는 기본 스토리지 1개 이외에 15개를 추가할 수 있으며, 각 스토리지 용량은 10GB ~2,000GB 까지 가능합니다.  
그리고, 2,000GB 이상을 사용해야 할 경우는 NAS 장비를 사용하거나 Object Storage를 활용하는 경우가 많지만, 상황에 따라서는 2,000GB를 초과하는 용량의 블록 스토리지를 사용해야 하는 경우도 있습니다.  
이럴때 사용하는 방법이 LVM (Logical Volume Manager)를 사용하여 여러 개의 스토리지를 합쳐서 대용량으로 사용하거나 합쳐진 대용량을 다시 필요한 용량으로 나누어서 사용하는 방법입니다.  
그래서 이번에는 리눅스 환경에서 LVM으로 대용량 스토리지를 생성하는 방법을 정리해보겠습니다.

## 스토리지 생성
2000GB 이상의 용량을 필요로 할 때에 대한 내용이지만, 테스트를 위해 10GB 스토리지 2개를 생성하겠습니다.  

<img src="/images/ncp_server_storage_lvm_create_01.jpg" alt="리눅스서버 스토리지(디스크) LVM 구성하기" style="width:770px;align:center">

lvm-test-1, lvm-test-2 이름으로 생성된 스토리지 2개를 Ubuntu OS가 설치된 lvm-test-svr 서버에 연결했습니다.

<img src="/images/ncp_server_storage_lvm_create_02.jpg" alt="리눅스서버 스토리지(디스크) LVM 구성하기" style="width:770px;align:center">

## 파티션 설정
생성된 스토리지 2개에 각각 파티션을 설정합니다.  
파티션 설정에는 parted 명령어를 사용합니다.

``` bash
~# parted /dev/xvdb

# (parted)
mklabel gpt
mkpart primary 0 100%
i
set 1 lvm on   # 1번 파티션에 lvm 사용 가능하게 설정
p
quit
```

<img src="/images/ncp_server_storage_lvm_create_03.jpg" alt="리눅스서버 스토리지(디스크) LVM 구성하기" style="width:700px;align:center">

``` bash
~# parted /dev/xvdc

# (parted)
mklabel gpt
mkpart primary 0 100%
i
set 1 lvm on   # 1번 파티션에 lvm 사용 가능하게 설정
p
quit
```

<img src="/images/ncp_server_storage_lvm_create_04.jpg" alt="리눅스서버 스토리지(디스크) LVM 구성하기" style="width:700px;align:center">

설정된 파티션들을 확인합니다.
``` bash
~# fdisk -l
```

<img src="/images/ncp_server_storage_lvm_create_05.jpg" alt="리눅스서버 스토리지(디스크) LVM 구성하기" style="width:600px;align:center">

## Physical Volume 생성
각 스토리지 장치에 Physical Volume을 생성합니다.

``` bash
~# pvcreate /dev/xvdb1

~# pvcreate /dev/xvdc1
```

<img src="/images/ncp_server_storage_lvm_create_06.jpg" alt="리눅스서버 스토리지(디스크) LVM 구성하기" style="width:500px;align:center">

Physical Volume이 제대로 생성되었는지 확인합니다.
``` bash
~# pvdisplay
```

<img src="/images/ncp_server_storage_lvm_create_07.jpg" alt="리눅스서버 스토리지(디스크) LVM 구성하기" style="width:600px;align:center">

## Volume Group 생성
준비된 두개의 Volume으로 하나의 Volume Group을 생성합니다.

``` bash
~# vgcreate VG01 /dev/xvdb1 /dev/xvdc1
```

<img src="/images/ncp_server_storage_lvm_create_08.jpg" alt="리눅스서버 스토리지(디스크) LVM 구성하기" style="width:500px;align:center">

생성된 Volume Group을 확인합니다.
``` bash
~# vgdisplay
```

<img src="/images/ncp_server_storage_lvm_create_09.jpg" alt="리눅스서버 스토리지(디스크) LVM 구성하기" style="width:550px;align:center">

## Logical Volume 생성
생성된 Volume Group 전체 크기를 사용하는 Logical Volume을 생성합니다.
``` bash
~# lvcreate --extents 100%FREE -n LV01 VG01
```

<img src="/images/ncp_server_storage_lvm_create_10.jpg" alt="리눅스서버 스토리지(디스크) LVM 구성하기" style="width:550px;align:center">

생성된 Logical Volume을 확인합니다.
``` bash
~# lvdisplay
```

<img src="/images/ncp_server_storage_lvm_create_11.jpg" alt="리눅스서버 스토리지(디스크) LVM 구성하기" style="width:550px;align:center">

## 포맷
다음으로 포맷을 해야하는데, OS별로 명령어가 다르므로 확인 후에 실행해야 합니다.  
여기서는 Ubuntu 기준으로 mkfs.ext4 명령어를 사용했습니다.
``` bash

~# mkfs.ext4 /dev/VG01/LV01

- CentOS 5.x: mkfs.ext3 /dev/VG01/LV01
- CentOS 6.x: mkfs.ext4 /dev/VG01/LV01
- CentOS 7.x: mkfs.xfs /dev/VG01/LV01
- Ubuntu : mkfs.ext4 /dev/VG01/LV01
```

<img src="/images/ncp_server_storage_lvm_create_12.jpg" alt="리눅스서버 스토리지(디스크) LVM 구성하기" style="width:700px;align:center">

## 마운트
마운트를 하기 위해 우선 생성된 디스크 장치명을 확인합니다.
``` bash
~# fdisk -l
```

<img src="/images/ncp_server_storage_lvm_create_13.jpg" alt="리눅스서버 스토리지(디스크) LVM 구성하기" style="width:600px;align:center">

다음으로 디스크를 마운트할 포인트 즉, 디렉토리를 원하는 이름으로 생성하고 마운트를 합니다.  
아래에 있는 마운트 경로 (/mnt/data)는 예시입니다. 원하는 경로를 직접 설정하시면 됩니다.
``` bash

~# mkdir /mnt/data
~# mount /dev/mapper/VG01-LV01 /mnt/data
```

<img src="/images/ncp_server_storage_lvm_create_14.jpg" alt="리눅스서버 스토리지(디스크) LVM 구성하기" style="width:600px;align:center">

## fstab 설정
새로 생성된 디스크를 부팅 후에도 인식할 수 있게 blkid 명령으로 UUID를 확인하고 fstab에 등록합니다.

``` bash
~# blkid |grep /dev/mapper/VG01-LV01
```
<img src="/images/ncp_server_storage_lvm_create_15.jpg" alt="리눅스서버 스토리지(디스크) LVM 구성하기" style="width:650px;align:center">

``` bash
~# vi /etc/fstab
```
<img src="/images/ncp_server_storage_lvm_create_16.jpg" alt="리눅스서버 스토리지(디스크) LVM 구성하기" style="width:770px;align:center">


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
