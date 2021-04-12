---
date: 2020-11-19
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

fdisk -l

fdisk /dev/xvdb

mkfs.ext4 /dev/xvdb1

	- CentOS 5.x: mkfs.ext3 /dev/xvdb1
	- CentOS 6.x: mkfs.ext4 /dev/xvdb1
	- CentOS 7.x: mkfs.xfs /dev/xvdb1
	- Ubuntu : mkfs.ext4 /dev/xvdb1

mkdir /mnt/data

mount /dev/xvdb1 /mnt/data

df

vi /etc/fstab
...
/dev/xvdb1 /mnt/data ext4 defaults 1 2
...

```

## 스토리지 생성, 할당
우선은 네이버 클라우드 콘솔에서 해당 서버의 상세정보에서 스토리지 생성 메뉴를 선택해서 원하는 타입과 용량을 선택하고 생성하면 서버에 스토리지가 할당됩니다.

## 스토리지 할당 확인
네이버 클라우드 콘솔에서 할당한 스토리지를 확인하기 위해 putty를 실행하여 서버에 접속합니다.  
이후 과정은 모두 서버에 접속한 상태에서 진행하게 됩니다.
``` bash
fdisk -l
```

## 스토리지(디스크) 파티션
할당된 스토리지에 파티션을 생성합니다.
``` bash
fdisk /dev/xvdb
```
<br />
#### fdisk /dev/xvdb
파티션을 생성할 때는 여러 단계의 옵션이 있습니다. 일반적으로는 아래와 같은 단계로 진행하면 됩니다.

1. 파티션을 새로 생성하기 위해 ‘n’을 입력
2. 생성할 파티션 타입에 따라 primary type이면 ‘p’, extended type이면 ‘e’를 입력. (primary type으로 생성하는 것이 일반적이며, primary 영역의 파티션이 부족할 경우 추가로 extended type으로 생성)
3. 생성할 파티션 번호와 cylinder 영역을 입력 (일반적으로 추가할 disk 전체를 mount하게 되고, 이 경우 default값을 그대로 사용하므로 Enter 입력)
4. ‘w’를 눌러서 해당 구성을 적용. 파티션 생성 완료.

## 스토리지(디스크) 포맷
다음으로 파티션이 생성된 디스크를 포맷하면 되는데, OS별로 명령어가 다르므로 확인 후에 실행하면 됩니다.
``` bash

- CentOS 5.x: mkfs.ext3 /dev/xvdb1
- CentOS 6.x: mkfs.ext4 /dev/xvdb1
- CentOS 7.x: mkfs.xfs /dev/xvdb1
- Ubuntu : mkfs.ext4 /dev/xvdb1
```

## 스토리지(디스크) 마운트
다음으로 스토리지를 마운트할 포인트 즉, 디렉토리를 원하는 이름으로 생성하고 마운트를 합니다.  
아래에 있는 마운트 경로 (/mnt/data)는 예시입니다. 원하는 경로를 직접 설정하시면 됩니다.
``` bash

mkdir /mnt/data
mount /dev/xvdb1 /mnt/data
```
<br />
마운트된 내역을 확인합니다.
``` bash
df
```

## 마운트 정보 등록
마운트 정보는 설정에 저장하지 않으면 서버가 리부팅될 때 사라지기 때문에 fstab에 저장합니다.
``` bash

vi /etc/fstab
...
/dev/xvdb1 /mnt/data ext4 defaults 1 2
...
```


## 참고 URL
<a href="https://guide.ncloud-docs.com/docs/compute-compute-4-1-v2" target="_blank" style="word-break:break-all;">https://guide.ncloud-docs.com/docs/compute-compute-4-1-v2.html</a>


> 문서 최종 수정일 : 2021-01-13