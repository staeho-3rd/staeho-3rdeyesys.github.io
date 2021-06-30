---
date: 2021-05-27
update: 2021-05-27
title: 리눅스서버 SSH 접속 보안 설정하기
categories:
  - 1.compute
description: 네이버 클라우드 리눅스 서버에 SSH로 접속할 때 필요한 보안 설정입니다.
type: Document
set: server
order_number: 16
creator: ljh0519
---


## 개요
리눅스 서버들은 서버에 접속할 때 SSH를 이용하게 되는데, 이때 root 계정에 대한 무작위 패스워드 입력 등의 해킹시도가 있을 수 있습니다.  
여기서는 이러한 해킹시도를 차단하기 위한 보안설정 중에서 root 계정과 관련한 보안설정 2가지를 정리해보겠습니다.

## 설정 파일 위치
root 계정에 대한 보안 설정은 **/etc/ssh/sshd_config** 파일에 있습니다.

``` bash
~# vi /etc/ssh/sshd_config
```
<br />
#### CentOS
<img src="../../images/ncp_server_ssh_security_setting_01.jpg" alt="리눅스서버 SSH 접속 보안 설정하기" style="width:660px;align:center">

#### Ubuntu
<img src="../../images/ncp_server_ssh_security_setting_06.jpg" alt="리눅스서버 SSH 접속 보안 설정하기" style="width:660px;align:center">

## root 로그인 차단
로그인 차단은 위 설정에서 PermitRootLogin 항목을 바꾸시면 됩니다.  
CentOS는 주석처리 되어 있으므로 주석을 해제하고 설정을 변경하시면 되고, Ubuntu는 주석이 해제된 상태이므로 설정값만 변경하시면 됩니다.

> root 로그인을 차단하기 전에 다른 관리자 계정을 생성한 후에 차단 설정을 적용해야 합니다.

``` bash
# 기존 - CentOS
#PermitRootLogin yes

# 기존 - Ubuntu
PermitRootLogin yes

# 변경
PermitRootLogin no
```
<img src="../../images/ncp_server_ssh_security_setting_02.jpg" alt="리눅스서버 SSH 접속 보안 설정하기" style="width:660px;align:center">

#### 기타 옵션
``` bash
# 패스워드 로그인은 차단하고 Key 파일을 이용한 로그인만 허용
PermitRootLogin prohibit-password
```

<br />
## 로그인 시도 횟수 제한
이 옵션을 지정하게 되면 지정한 횟수 이상으로 로그인을 실패했을 때 접속이 강제 종료되는데, 기본값은 6회이니 적절하게 수정하시면 됩니다.

``` bash
# 기존
#MaxAuthTries 6

# 변경
MaxAuthTries 3
```
<br >
#### 데몬 재시작
설정을 수정하고 파일을 저장한 후에 sshd 데몬을 재시작합니다.

``` bash
~# systemctl restart sshd
```
<br />
데몬 재시작 후 로그인을 시도해보면 로그인이 실패하는 것을 확인하실 수 있습니다.

<img src="../../images/ncp_server_ssh_security_setting_03.jpg" alt="리눅스서버 SSH 접속 보안 설정하기" style="width:660px;align:center">

## SSH 접속 로그 확인
SSH로 접속을 하면 성공, 실패에 대한 로그가 모두 남게 되는데, 이 로그를 주기적으로 확인하는 것이 좋습니다.

#### 접속 실패 로그
``` bash
~# last -f /var/log/btmp

# 또는
~# lastb
```
<img src="../../images/ncp_server_ssh_security_setting_04.jpg" alt="리눅스서버 SSH 접속 보안 설정하기" style="width:660px;align:center">

#### 접속 성공 로그
``` bash
~# last -f /var/log/wtmp
```
<img src="../../images/ncp_server_ssh_security_setting_05.jpg" alt="리눅스서버 SSH 접속 보안 설정하기" style="width:660px;align:center">



## 참고 URL
1. 네이버 클라우드 리눅스서버 접속 가이드
	- <a href="https://guide.ncloud-docs.com/docs/compute-compute-3-1-v2" target="_blank" style="word-break:break-all;">https://guide.ncloud-docs.com/docs/compute-compute-3-1-v2.html</a>

2. 네이버 클라우드 플랫폼을 활용한 보안 강화
	- <a href="https://m.blog.naver.com/n_cloudplatform/221117956958" target="_blank" style="word-break:break-all;">https://m.blog.naver.com/n_cloudplatform/221117956958</a>


> 문서 최종 수정일 : 2021-05-28