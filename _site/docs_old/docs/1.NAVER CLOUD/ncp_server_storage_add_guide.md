# 스토리지 추가 생성 가이드

## 개요
네이버 클라우드에서 서버 생성 후에 스토리지를 추가 생성하는 경우가 있는데 이때 사용되는 스토리지는 Block Storage라고 해서 AWS의 EBS(Elastic Block Store)와 유사합니다.

## 스토리지 추가 제약 사항

- 부팅 스토리지가 SSD인 경우, 추가 스토리지로 HDD, SSD 모두 추가할 수 있습니다.
- 부팅 스토리지가 HDD인 경우, HDD, SSD 모두 추가할 수 있으나 SSD 스토리지는 최신 서버에만 추가가 가능합니다. 서버 상세정보의 'SSD 스토리지 추가 여부'가 적용 가능인지 확인해야 합니다.
- {==**Micro 타입의 서버, Bare Metal 서버는 스토리지를 추가할 수 없습니다.**==}


## 추가 가능한 최대 사이즈와 개수
스토리지는 **최대 2,000GB**를 지원하며, 서버 1대당 **최대 16개**의 스토리지를 이용할 수 있습니다. (단, Local Disk 서버 타입 및 2017년 1월 23일 이전에 생성된 서버에 대해서는 1,000GB까지 지원됩니다.)


## 리눅스 OS 서버 이미지별 포맷 명령어
리눅스는 OS 즉, 네이버 클라우드에서 제공하는 서버 이미지별로 추가된 스토리지를 포맷하는 명령어가 다릅니다.

- CentOS 5.x: mkfs.ext3 /dev/xvdb1
- CentOS 6.x: mkfs.ext4 /dev/xvdb1
- CentOS 7.x: mkfs.xfs /dev/xvdb1
- Ubuntu Server / Desktop: mkfs.ext4 /dev/xvdb1


## 참고 URL
<a href="https://docs.ncloud.com/ko/compute/compute-4-1-v2.html" target="_blank">https://docs.ncloud.com/ko/compute/compute-4-1-v2.html</a>


!!! info "문서 최종 수정일 : 2020-11-19" 