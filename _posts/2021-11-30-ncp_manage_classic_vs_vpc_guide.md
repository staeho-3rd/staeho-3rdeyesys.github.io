---
date: 2021-11-30
last_modified_at: 2021-12-03
title: 네이버 클라우드 Classic 환경 vs VPC 환경 비교
categories:
  - 81.manage
description: 네이버 클라우드 Classic 환경 vs VPC 환경 비교 가이드입니다
type: Document
set: server
order_number: 24
v2_link: /management/ncloud_management_classic_vs_vpc_guide.html
---

## 개요
네이버 클라우드 (nCloud)에는 Classic과 VPC 이렇게 2가지의 환경이 있습니다.
각각의 특징을 장점을 중심으로 비교해보도록 하겠습니다.

## Classic 환경 장점 요약
- 서로 다른 계정의 서버들 간에 사설 통신 가능
- 리전간 서버들의 사설 통신 가능 (한국, 미국, 싱가포르, 홍콩, 일본, 독일)
- 다양한 설치형 서버 이미지 이용 가능


## VPC 환경 장점 요약
- 논리적으로 분리된 Network
- 사용자가 직접 Network 설계 가능
- 기존 고객의 데이터센터 네트워크와 유사하게 구현 가능
- 좀 더 상세하고, 높은 수준의 보안 설정 가능

<br />
## Classic 환경 장점 상세
위에서 요약한 장점들을 좀 더 상세하게 살펴보겠습니다.

#### 서로 다른 계정의 서버들 간에 사설 통신 가능
사용자가 2개 이상의 계정을 보유하고 있을 경우 각 계정에 생성된 서버들간에 사설 통신을 할 수 있습니다.

#### 리전간 서버들의 사설 통신 가능
현재 네이버 클라우드는 한국, 미국, 싱가포르, 홍콩, 일본, 독일 이렇게 6개의 리전으로 서비스가 구분되어 있는데, 각 리전에 생성된 서버들끼리 사설 통신을 할 수 있습니다.

#### 다양한 설치형 서버 이미지 이용 가능
Classic 환경에서는 LAMP, Wordpress 등 사용자들이 편하게 각 종 애플리케이션과 DB가 미리 설치된 서버를 쉽게 생성할 수 있도록 설치형 서버 이미지를 제공하고 있습니다. 
지원하는 주요 이미지는 다음과 같습니다.

- Jenkins, Tensorflow, RabbitMQ, Pinpoint, LAMP, WordPress, Magento, Drupal, Joomla!, Shadowsocks, LEMP, Hugo, Gitlab CE, Node.js, Superset, Tomcat, JEUS, WebtoB, Gradle
- MySQL, MSSQL, Cubrid, PostgreSQL, MariaDB, Redis, Tibero


## VPC 환경 장점 상세
위에서 요약한 장점들을 좀 더 상세하게 살펴보겠습니다.

#### 논리적으로 분리된 Network
- 논리적으로 분리된 Network 체계를 제공하기 때문에 다른 이용자와의 간섭 없이 더 안전하고, 투명한 환경을 구현할 수 있습니다.

#### 사용자가 직접 Network 설계 가능
- 네트워크 서브넷(Subnet) 기능을 통해 용도에 따라 네트워크를 세분화하여 서비스 맞춤형 네트워크를 사용자가 직접 구성하실 수 있습니다
- Load Balancer가 Application Load Balancer / Network Load Balancer / Network Proxy Load Balancer 등 3가지로 구분되어 있어, 고객이 각자의 서비스 환경에 최적화된 Load Balancer를 선택할 수 있습니다.
- 원하는 사설 IP 대역을 직접 할당할 수 있습니다. 
  그러므로 사용해야 하는 사설 IP대역이 정해져 있어 변경할 수 없거나, 서버의 사설 IP를 변경하기 어려운 경우에도 VPC 환경에서는 원하는 대역의 원하는 IP를 직접 부여할 수 있습니다.

#### 기존 고객의 데이터센터 네트워크와 유사하게 구현 가능
- 기존 고객 데이터센터 네트워크와 유사하게 구현 가능합니다.
  즉, 기존에 AWS를 사용하고 있었던 고객은 AWS는 VPC 환경만 제공하기 때문에 거의 비슷하게 구현 가능합니다.
  또한 온프레미스 환경의 IDC 센터를 이용했던 고객이 클라우드 환경으로 마이그레이션해야 할 때 VPC 환경은 기존의 구조를 거의 그대로 구현해서 마이그레이션할 수 있습니다.

#### 좀 더 상세하고, 높은 수준의 보안 설정 가능
- 서버 측면에서 접근 제어를 위한 ACG(Access Control Group) 외에도,  네트워크 서브넷 측면에서 접근 제어를 할 수 있도록 NACL(Network Access Control List)을 제공합니다. 
  이처럼 여러가지 접근제어 기능을 이용해 클라우드 상에서 발생할 수 있는 다양한 공격에 대한 대비가 가능합니다.
- ACG, NACL등의 네트워크 접근제어 설정에서 Inbound, Outbound 각각에 대한 제어 설정 등 상세한 보안 설정을 직접 할 수 있습니다.


#### VPC 환경 활용사례
- 외부와 인터넷 연결이 필요한 서브넷과 이와 별개로 중요 데이터를 저장하기 위해 외부 접속을 최소화하기 위한 서브넷을 구성하는 경우  
  하나의 서브넷에 인터넷 게이트웨이를 연결하여 프런트-엔드(Front-end) 전용 서브넷을 구성하고, 다른 하나의 서브넷에는 NAT 게이트웨이를 연결하여 백-엔드(Back-end)용 서브넷으로 활용

- Cloud Connect만을 이용하여 접근할 수 있는 서브넷을 구성하는 경우  
   Cloud Connect를 이용하여 On-Premise에 있는 고객의 전산망을 클라우드로 확장한 하이브리드 클라우드 구성

- 외부와 인터넷 연결이 필요한 서브넷과 이와 별개로 외부 인터넷 연결을 차단하고 VPN을 통해 On-Premise에서만 접근할 수 있는 서브넷을 구성하는 경우  
  하나의 서브넷은 외부와 연결되는 일반 구성으로 하고, 다른 하나의 서브넷에 IPsec VPN을 연결하여 VPN 전용 서브넷(VPN Only Subnet)을 구성하는 방식

- 다른 두개의 서비스를 각각 다른 사설 IP대역에서 서비스하려는 경우  
  VPC를 2개 생성해서 각각 다른 사설 IP 대역을 할당해서 구성 가능


## Classic vs VPC 선택

#### Classic 선택
- 구축하려는 서비스 규모가 작으며 네트워크 설정은 신경 쓰고 싶지 않은 경우는 Classic 환경
- 리전간 서버끼리 사설통신이 필요하다면 (ex. DB 한국리전, 서비스서버 일본리전) Classic 환경
- 다양한 설치형 서버 이미지들이 필요하다면 Classic 환경

#### VPC 선택
- 네트워크 세분화및  서비스에 별도 사설 대역이 필요한 경우는 VPC 환경
- 서버, 네트워크 통신의 inbound, outbound를 직접 통제하고 싶은 경우는 VPC 환경
- 좀 더 다양한 기능을 지원하는 상품을 이용하고자 할때는 VPC환경


## 참고 URL
1.  VPC 제품 설명
	- <a href="https://www.ncloud.com/product/networking/vpc" target="_blank" style="word-break:break-all;">https://www.ncloud.com/product/networking/vpc</a>

2.  VPC 사용 가이드
	- <a href="https://guide.ncloud-docs.com/docs/ko/networking-vpc-vpcoverview" target="_blank" style="word-break:break-all;">https://guide.ncloud-docs.com/docs/ko/networking-vpc-vpcoverview</a>
