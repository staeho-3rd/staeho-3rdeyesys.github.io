---
date: 2021-06-30
last_modified_at: 2021-12-17
title: VPC 환경에서 Application Load Balancer 생성하기
categories:
  - 2.networking
description: 네이버 클라우드 VPC 환경에서 Application Load Balancer 생성하기 가이드 입니다.
type: Document
set: Load Balancer
order_number: 8
creator: ljh0519
v2_link: /networking/ncloud_networking_load_balancer_application_lb.html
---

## 개요
네이버 클라우드 VPC 환경의 대표적인 Load Balancer인 Application Load Balancer 를 생성하는 가이드입니다.  
VPC와 Subnet을 생성하고, 테스트를 위한 서버 2대를 CentOS와 Ubuntu 각각 1대씩 준비해서 Application Load Balancer와 연결하고 접속해보는 과정까지 정리해보겠습니다.

## VPC 생성
VPC 환경에서는 먼저 VPC를 먼저 생성해야 하며, 이미 만들어진 VPC가 있다면 그대로 이용하셔도 됩니다.  
VPC의 IP 주소 범위는 private 대역 (10.0.0.0/8, 172.160.0./12, 192.168.0.0/16) 내에서 /16 ~ /28 범위여야 합니다.  
여기서는 192.168.0.0/16 범위의 VPC를 생성하겠습니다.

<img src="/images/ncp_networking_load_balancer_application_01.jpg" alt="네이버 클라우드 VPC 환경의 Application Load Balancer 생성하기 " style="width:770px;align:center">

## Subnet 생성
Load Balancer를 생성할 때 Server와는 다른 Subnet을 사용해야 정상 작동합니다.  
그래서 여기서도 Load Balancer용 Subnet과 테스트 Server용 Subnet을 각각 생성하도록 하겠습니다.

#### Load Balancer용 Subnet 생성
Load Balancer는 Private Subnet에 위치해야 하므로 192.168.1.0/24 IP 대역에 Internet Gateway 전용 여부 옵션은 N (Private)을 선택합니다.

<img src="/images/ncp_networking_load_balancer_application_02.jpg" alt="네이버 클라우드 VPC 환경의 Application Load Balancer 생성하기 " style="width:770px;align:center">


#### Server용 Subnet 생성
일반 서버용 Subnet은 192.168.2.0/24 IP 대역에 Internet Gateway 전용 여부 옵션은 Y (Public)을 선택합니다.

<img src="/images/ncp_networking_load_balancer_application_03.jpg" alt="네이버 클라우드 VPC 환경의 Application Load Balancer 생성하기 " style="width:770px;align:center">

## 테스트용 Server 생성
테스트를 위한 서버는 위에서 생성했던 192.168.2.0/24 IP 대역의 Subnet을 선택하고 Network Interface도 동일한 Subnet을 선택합니다.  
Load Balancer를 테스트 하기 위한 서버이므로 2대를 생성하면서 1대는 CentOS, 나머지 1대는 Ubunt로 생성하겠습니다.

<img src="/images/ncp_networking_load_balancer_application_04.jpg" alt="네이버 클라우드 VPC 환경의 Application Load Balancer 생성하기 " style="width:770px;align:center">

## Target Group 생성
[VPC] - [Load Balancer] - [Target Group]에서 Load Balancer가 바라보게 될 Target Group를 설정합니다.  
여기서는 HTTP 프로토콜과 80 포트를 선택하겠습니다.

<img src="/images/ncp_networking_load_balancer_application_05.jpg" alt="네이버 클라우드 VPC 환경의 Application Load Balancer 생성하기 " style="width:770px;align:center">

다음으로 Health Check 설정에서는 HTTP, 80포트, HEAD Method를 선택합니다.

<img src="/images/ncp_networking_load_balancer_application_06.jpg" alt="네이버 클라우드 VPC 환경의 Application Load Balancer 생성하기 " style="width:770px;align:center">

마지막으로 Target 즉, 대상이 되는 서버 2대 (lb-test-ubuntu, lb-test-centos)를 선택하고, 적용 Target쪽으로 이동시키는 버튼을 클릭합니다.

<img src="/images/ncp_networking_load_balancer_application_07.jpg" alt="네이버 클라우드 VPC 환경의 Application Load Balancer 생성하기 " style="width:770px;align:center">

대상 서버들이 적용 Target으로 설정된 모습입니다. 이후에는 전체 설정을 다시 확인하고 생성 완료를 하면 됩니다.

<img src="/images/ncp_networking_load_balancer_application_08.jpg" alt="네이버 클라우드 VPC 환경의 Application Load Balancer 생성하기 " style="width:770px;align:center">


## Load Balancer 생성
네이버 클라우드 VPC 환경에서 제공하는 Load Balancer는 애플리케이션 로드밸런서, 네트워크 로드밸런서, 네트워크 프록시 로드밸런서 이렇게 3가지가 있습니다.  
그 중에서 가장 많이 사용하는 HTTP, HTTPS 트래픽을 사용하는 웹 애플리케이션용 Application Load Balancer를 생성하겠습니다.

<img src="/images/ncp_networking_load_balancer_application_09.jpg" alt="네이버 클라우드 VPC 환경의 Application Load Balancer 생성하기 " style="width:770px;align:center">

Network는 Public IP, Subnet은 앞에서 생성했던 192.168.1.0/24 대역의 Subnet을 선택합니다.

<img src="/images/ncp_networking_load_balancer_application_10.jpg" alt="네이버 클라우드 VPC 환경의 Application Load Balancer 생성하기 " style="width:770px;align:center">

리스너 설정에서 프로토콜은 HTTP, 포트는 80을 선택하고 [추가] 버튼을 클릭합니다.

<img src="/images/ncp_networking_load_balancer_application_11.jpg" alt="네이버 클라우드 VPC 환경의 Application Load Balancer 생성하기 " style="width:770px;align:center">

Target Group은 앞에서 생성했던 test-tg을 선택하면, 아래에 해당 Target Group의 설정이 표시됩니다.  
다음으로 전체 설정을 다시 확인하고 최종 생성하기 버튼을 클릭하면 Load Balancer가 생성됩니다.

<img src="/images/ncp_networking_load_balancer_application_12.jpg" alt="네이버 클라우드 VPC 환경의 Application Load Balancer 생성하기 " style="width:770px;align:center">

## Network ACL 설정
Load Balancer가 정상 작동하기 위해서는 2가지 설정이 추가로 필요한데, 우선 Network ACL 설정에서 Load Balancer가 속한 Subnet 대역을 허용해 주어야 합니다.  
[VPC] - [Network ACL] - [ACL Rule]에서 [Rule 설정]에 있는 [Inbound 규칙]에 앞에서 생성했던 Load Balancer용 Subnet 대역인 192.168.1.0/24 대역의 80 포트를 허용으로 설정해 줍니다.

<img src="/images/ncp_networking_load_balancer_application_13.jpg" alt="네이버 클라우드 VPC 환경의 Application Load Balancer 생성하기 " style="width:770px;align:center">

## ACG 설정
다음으로 [Server] - [ACG]에서 [Inbound 규칙]에 마찬가지로 Load Balancer용 Subnet 대역인 192.168.1.0/24 대역의 80 포트를 허용포트로 설정해 줍니다.

<img src="/images/ncp_networking_load_balancer_application_14.jpg" alt="네이버 클라우드 VPC 환경의 Application Load Balancer 생성하기 " style="width:770px;align:center">

## Server 웹서버 설정
Application Load Balancer를 테스트 하기 위해서는 테스트 Server에 웹서버가 설정되어 있어야 하는데, 네이버 클라우드 VPC 환경에서는 LAMP, LEMP 등의 웹서버가 설정된 이미지를 제공하지 않습니다.  
그래서 기본 OS만 설치된 서버에 Apache 웹서버와 php를 설치하도록 하겠습니다. 설치 작업은 아래와 같이 Ubuntu와 CentOS 각각 스크립트를 만들어서 한번에 설치하는 방법을 사용했는데, 필요에 따라서는 하나씩 별도로 설치하셔도 됩니다.

#### Ubuntu에 Apache, PHP 설치하기 스크립트
Apache와 PHP를 설치하고 기본문서 index.html에 서버의 호스트명을 출력하는 스크립트를 적용합니다.

사용한 OS는 Ubuntu 16.04 입니다.

``` bash
#!/bin/bash

apt-get update
apt-get install apache2 
apt-get install php
apt-get install libapache2-mod-php

systemctl start apache2

cd /var/www/html
echo "<?php" > index.html
echo "echo \"Your server name is \".(gethostname()).\"<br>\";" >> index.html
echo "?>" >> index.html

echo AddType application/x-httpd-php .php .php3 .php4 .php5 .html .htm .inc >> phpadd
cat phpadd >> /etc/apache2/apache2.conf

systemctl restart apache2
systemctl enable apache2
systemctl status apache2
```
<br />

#### CentOS에 Apache, PHP 설치하기 스크립트
사용한 OS는 CentOS 7.3 입니다.

``` bash
#!/bin/bash

yum install -y httpd php

systemctl start httpd

cd /var/www/html
echo "<?php" > index.html
echo "echo \"Your server name is \".(gethostname()).\"<br>\";" >> index.html
echo "?>" >> index.html

echo AddType application/x-httpd-php .php .php3 .php4 .php5 .html .htm .inc >> phpadd
cat phpadd >> /etc/httpd/conf/httpd.conf

systemctl restart httpd
systemctl enable httpd
systemctl status httpd
```

## 접속 테스트
앞에서 생성했던 Load Balancer 정보에서 접속 정보용 주소를 확인하고 복사합니다.

<img src="/images/ncp_networking_load_balancer_application_16.jpg" alt="네이버 클라우드 VPC 환경의 Application Load Balancer 생성하기 " style="width:770px;align:center">

복사한 Load Balancer 주소를 웹브라우저에 입력하고 계속 새로 고침을 해보면 아래와 같이 CentOS 서버와 Ubuntu에 접속될 때 마다 해당 서버의 호스트명이 출력되면서 Load Balancer가 정상 작동하는 것을 확인할 수 있습니다.

<img src="/images/ncp_networking_load_balancer_application_15-1.jpg" alt="네이버 클라우드 VPC 환경의 Application Load Balancer 생성하기 " style="width:560px;align:center">

<img src="/images/ncp_networking_load_balancer_application_15-2.jpg" alt="네이버 클라우드 VPC 환경의 Application Load Balancer 생성하기 " style="width:560px;align:center">


## 참고 URL
1. Application Load Balancer 가이드
	- <a href="https://guide.ncloud-docs.com/docs/networking-loadbalancer-applicationlbconsole" target="_blank" style="word-break:break-all;">https://guide.ncloud-docs.com/docs/networking-loadbalancer-applicationlbconsole</a>

2. Target Group 가이드
	- <a href="https://guide.ncloud-docs.com/docs/networking-loadbalancer-targetgroupconsole" target="_blank" style="word-break:break-all;">https://guide.ncloud-docs.com/docs/networking-loadbalancer-targetgroupconsole</a>
