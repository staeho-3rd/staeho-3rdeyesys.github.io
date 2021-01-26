---
date: 2021-01-26
title: CentOS6에서 pip - Python 설치하기
categories:
  - 1.compute
description: 네이버 클라우드 CentOS6.x에서 pip - Python 설치하기
type: Document
set: database
order_number: 5
---

## 개요
2020년 12월 01일부로 CentOS6의 기술지원이 공식 종료되었습니다.  
yum을 이용한 패키지 설치, 업데이트 등을 할 수 없는 상황이기에 pip, Python등의 설치가 원활하지 않습니다.
그래서 pip설치에 필요한 CentOS6.x의 yum 명령을 수행하려면 
먼저 아래의 가이드대로 Repository mirror를 변경하는 Script를 다운받아 설치해야 그 다음 단계를 진행할 수 있습니다.

## 목적
지원이 종료된 CentOS6에서 굳이 pip를 설치하려고 하는 이유는 DB 백업 파일을 aws cli를 이용해서 Object Storage에 저장하기 위해서 입니다.  
매 일정 시간에 DB나 소스 파일을 백업하고 백업된 파일을 Object Storage에 백업-동기화 하는 작업을 aws cli를 이용해서 처리하게 됩니다.

## Repository 변경
이 Repository 변경 스트립트는 네이버 클라우드에서 공식 제공하는 스크립트입니다.
``` bash
~# wget http://repo.ncloud.com/etc/patch/Add-CentOS-Vault-Repo.sh
~# bash Add-CentOS-Vault-Repo.sh
~# yum install zsh
```

## 필요 패키지들 설치
``` bash
~# yum -y groupinstall 'Development Tools'
~# yum -y install openssl-devel* ncurses-devel* zlib*.x86_64
~# yum update curl nss
```

## Python 설치
CentOS6에는 Python 2.6이 설치되어 있습니다.  
하지만, 위에서 설명한 대로 CentOS 6에 대한 지원이 종료되면서 일반적인 방법으로는 Python과 pip를 설치할 수 없습니다.  
몇가지 상황을 테스트 해본 결과 Python 2.7과 Python 3.5는 정상적으로 설치하는 방법을 찾았고, 그래서 여기서는 Python 3.5.10을 설치하겠습니다.

``` bash
# Python 3.5 설치 압축파일 다운로드
~# wget https://www.python.org/ftp/python/3.5.10/Python-3.5.10.tgz

# Python 설치파일 압축해제 후 설치
~# tar vxzf Python-3.5.10.tgz
~# cd Python-3.5.10
~# ./configure
~# make
~# make install

# Python 설치 확인
~# which python3

# Python 버전 확인
~# python3 -V
```

## PIP 설치
pip설치 파일도 위에서 설치한 Python 버전에 맞는 설치파일을 다운받아서 설치해야 문제없이 설치됩니다.  
현재까지 문제 없는 것으로 확인된 버전은 2.7과 3.5 입니다.
``` bash
~# curl -O https://bootstrap.pypa.io/3.5/get-pip.py
~# python3 get-pip.py
```


## 설치 오류 상황과 해결 방법
pip를 설치하는 과정에 발생하는 몇가지 오류 상황과 그에 따른 해결방법을 정리해보겠습니다.

1. curl: (35) SSL connect error  
pip 설치파일을 다운 받기 위해 curl을 실행할 때 발생하는 오류 메시지로 위쪽에서 필요 패키지들 설치할 때 있었던 다음 명령을 실행하면 해결됩니다.
``` bash
~# yum update curl nss
```

2. SyntaxError: invalid syntax  
pip를 설치할 때 Python 버전이 일치하지 않는 등의 이유로 발생하는 문제입니다.  
위에서 설명했던 대로 Python 버전과 일치하는 pip 설치 파일을 다운 받으면 됩니다.  

``` bash
# 오류가 발생하는 경로
~# curl -O https://bootstrap.pypa.io/get-pip.py

# 해결방법 : 경로 중간에 Python 버전을 추가
~# curl -O https://bootstrap.pypa.io/{Python 버전}/get-pip.py

# 오류 없는 버전 예시
~# curl -O https://bootstrap.pypa.io/2.7/get-pip.py
~# curl -O https://bootstrap.pypa.io/3.5/get-pip.py
```

> 문서 최종 수정일 : 2021-01-26