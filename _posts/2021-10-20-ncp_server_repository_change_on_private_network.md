---
date: 2021-10-20
last_modified_at: 2021-10-25
title: 외부 네트워크에 연결할 수 없는 환경에서 Repository를 변경해 리눅스 패키지 설치하기
categories:
  - 1.compute
description: 네이버 클라우드 Secure Zone이나 Private Network 환경에서 Repository를 변경해 리눅스 패키지 설치하는 방법입니다.
type: Document
set: server
order_number: 24
creator: franky
---

## 개요
Ncloud(네이버 클라우드) Secure Zone이나 VPC 환경의 Private Network처럼 외부와 통신이 단절된 환경에서 
리눅스 패키지를 설치해야 할 때 repository 경로를 네이버 클라우드 내부 repository로 바꾸면 문제없이 패키지 설치를 할 수 있습니다. 
여기서는 OS별로, Classic/VPC 환경별로 변경하는 방법을 정리해보겠습니다.


## CentOS 
 
CentOS는 /etc/yum.repos.d/CentOS-Base.repo 를 열어보면 아래 repository주소를 확인할 수 있습니다.

```bash
~# vi /etc/yum.repos.d/CentOS-Base.repo
```

<br />
#### Classic 환경 - CentOS
네이버 클라우드 Classic 환경 CentOS에서 repository 주소는 미러 사이트가 기본으로 설정 되어 있는 것을 확인할 수 있습니다.

<img src="../../images/ncp_server_repository_change_on_private_network_01.png" alt="네이버 클라우드 Secure Zone이나 Private Network 환경에서 Repository를 변경해 리눅스 패키지 설치하는 방법" style="width:770px;align:center">

하지만 현재 서버는 Secure Zone 등 외부와 통신이 되지 않는 상태이므로 이대로는 패키지 설치를 할 수 없습니다.
이때 네이버 클라우드 내부 repository ( **mirror.ncloud.com** )로 접속하도록 mirrorlist를 주석처리하고, baseurl을 주석해제하고, 경로를 수정해주면 됩니다.

```bash
## 원본
mirrorlist=http://mirrorlist.centos.org/?release=$releaserver&arch=$basearch....
#baseurl=http://mirror.centos.org/centos/$releaserver/extras/$basearch/

## 수정
#mirrorlist=http://mirrorlist.centos.org/?release=$releaserver&arch=$basearch....
baseurl=http://mirror.ncloud.com/centos/$releaserver/extras/$basearch/
```

<img src="../../images/ncp_server_repository_change_on_private_network_03.png" alt="네이버 클라우드 Secure Zone이나 Private Network 환경에서 Repository를 변경해 리눅스 패키지 설치하는 방법" style="width:770px;align:center">

변경 후 패키지 설치 테스트를 해봅니다.

```bash
~# yum -y install httpd
```

웹서버 설치가 문제없이 되는 것을 확인할 수 있습니다.

<img src="../../images/ncp_server_repository_change_on_private_network_04.png" alt="네이버 클라우드 Secure Zone이나 Private Network 환경에서 Repository를 변경해 리눅스 패키지 설치하는 방법" style="width:770px;align:center">

#### sed 명령어 사용
sed 명령어를 사용하면 더욱 편하게 변경할 수 있습니다.

```bash
## mirrorlist 주석처리
~# sed -i 's/mirrorlist=/#mirrorlist=/g' /etc/yum.repos.d/CentOS-Base.repo

## baseurl 주석해제, 경로수정
~# sed -i 's/#baseurl=http:\/\/mirror.centos.org\/centos/baseurl=http:\/\/mirror.ncloud.com\/centos/g' /etc/yum.repos.d/CentOS-Base.repo
```

<br />
#### VPC 환경 - CentOS
네이버 클라우드 VPC 환경은 이미 repository 경로가 네이버 클라우드 내부 repository ( **mirror.ncloud.com** )로 설정되어 있으므로 별도로 수정할 필요가 없습니다.

<img src="../../images/ncp_server_repository_change_on_private_network_10.png" alt="네이버 클라우드 Secure Zone이나 Private Network 환경에서 Repository를 변경해 리눅스 패키지 설치하는 방법" style="width:770px;align:center">


## Ubuntu

Ubuntu는 /etc/apt/sources.list 에서 repository list 를  확인할 수 있습니다.  
다만, 기본 미러사이트 주소가 Classic, VPC 두 환경이 다르게 설정되어 있습니다.
- Classic :  **kr.archive.ubuntu.com**
- VPC : **archive.ubuntu.com**

```bash
~# vi /etc/apt/sources.list
```

#### Classic 환경 - Ubuntu
<img src="../../images/ncp_server_repository_change_on_private_network_05.png" alt="네이버 클라우드 Secure Zone이나 Private Network 환경에서 Repository를 변경해 리눅스 패키지 설치하는 방법" style="width:770px;align:center">

#### VPC 환경 - Ubuntu
<img src="../../images/ncp_server_repository_change_on_private_network_11.png" alt="네이버 클라우드 Secure Zone이나 Private Network 환경에서 Repository를 변경해 리눅스 패키지 설치하는 방법" style="width:770px;align:center">

현재 서버는 Secure Zone 또는 Private Network  등 외부와 통신이 되지 않는 상태이므로 이대로는 패키지 설치를 할 수 없습니다.
이때 네이버 클라우드 내부 repository ( **mirror.ncloud.com** )로 접속하도록 경로를 수정해주면 됩니다.

```bash
## Classic 환경 - Ubuntu
kr.archive.ubuntu.com --> mirror.ncloud.com (변경)
security.ubuntu.com --> mirror.ncloud.com (변경)

## VPC 환경 - Ubuntu
archive.ubuntu.conm --> mirror.ncloud.com (변경)
```
<img src="../../images/ncp_server_repository_change_on_private_network_12.png" alt="네이버 클라우드 Secure Zone이나 Private Network 환경에서 Repository를 변경해 리눅스 패키지 설치하는 방법" style="width:770px;align:center">


/etc/apt/sources.list 에서 위와 같이 변경-저장한 후에 apt update 를 해주면 변경해준 Ncloud 내부 repository에서 패키지 리스트를 가져와 설치를 합니다.

<img src="../../images/ncp_server_repository_change_on_private_network_08.png" alt="네이버 클라우드 Secure Zone이나 Private Network 환경에서 Repository를 변경해 리눅스 패키지 설치하는 방법" style="width:770px;align:center">

설치 된 repository를 테스트 하기 위해 apt install 을 사용하여 패키지 다운로드를 해보면 Ncloud 내부 repository에서 패키지 설치가 진행되는 것을 확인 할 수 있습니다.

```bash
~# apt -y install apache2
```

<img src="../../images/ncp_server_repository_change_on_private_network_09.png" alt="네이버 클라우드Secure Zone에서 OS 패키지를 설치하기 위해 Repository를 변경하는 방법" style="width:770px;align:center">


Ubuntu 또한 sed 명령어로 간단하게 변경할 수 있습니다.

```bash
## Classic 환경 - Ubuntu
sed -i 's/kr.archive.ubuntu.com/mirror.ncloud.com/g' /etc/apt/sources.list
sed -i 's/security.ubuntu.com/mirror.ncloud.com/g' /etc/apt/sources.list

## VPC 환경 - Ubuntu
sed -i 's/archive.ubuntu.com/mirror.ncloud.com/g' /etc/apt/sources.list
```


## 참고 URL
1.  CentOS mirror 사이트 안내
	- <a href="https://www.centos.org/download/mirrors/" target="_blank" style="word-break:break-all;">https://www.centos.org/download/mirrors/</a>

2.  Ubuntu mirror 사이트 안내
	- <a href="https://launchpad.net/ubuntu/+archivemirrors" target="_blank" style="word-break:break-all;">https://launchpad.net/ubuntu/+archivemirrors</a>
