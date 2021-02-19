---
date: 2021-01-06
title: VPC 환경에서 서버 생성
categories:
  - 1.compute
description: 네이버 클라우드 VPC 환경에서 서버 생성
type: Document
set: server
order_number: 11
---

## 개요
네이버 클라우드 VPC환경에서 서버를 생성하는 순서를 정리해보겠습니다.  
아래 순서는 굳이 기억하지 않아도 필요한 부분은 그때그때  안내가 잘 되어 있기 때문에 별 문제는 없지만 그래도 어떤 것들이 필요한 가에 대해 어느 정도 알아둘 필요는 있어 보입니다.


## VPC 생성
VPC (Virtual Private Cloud) 즉, 클라우드 상에서 논리적으로 격리된 고객 전용 네트워크 공간을 먼저 생성하고 해당 공간에 서버를 생성하게 됩니다.  
VPC는 고객의 계정마다 **최대 3개**를 생성할 수 있으며, 각 VPC는 최대 넷마스크 0.0.255.255/16 (IP 65,536개) 크기의 네트워크 주소 공간을 제공합니다.

- VPC 이름 입력
- IP 주소 범위 입력

> VPC의 IP 주소 범위는, private 대역(10.0.0.0/8,172.16.0.0/12,192.168.0.0/16) 내에서 /16~/28 범위

<img src="../../images/ncp_server_vpc_create_02.jpg" alt="VPC 환경에서 서버 생성" style="width:600px;align:center">

## Subnet 생성
- Subnet 이름 입력
- VPC 선택
- IP 주소 범위 입력
- Zone 선택
- Network ACL 선택
- Internet Gateway 전용여부 선택 (Public or Private)

<img src="../../images/ncp_server_vpc_create_03.jpg" alt="VPC 환경에서 서버 생성" style="width:600px;align:center">

## 서버 설정
- VPC 선택
- Subnet 선택
- 스토리지 종류 선택
- 서버 세대 선택
- 서버 타입 선택
- 요금제 선택
- 서버 개수 입력
- 서버 이름 입력
- Network Interface 추가
- 물리 배치 그룹 여부 선택
- 반납 보호 여부 선택

<img src="../../images/ncp_server_vpc_create_01.jpg" alt="VPC 환경에서 서버 생성" style="width:600px;align:center">

## 인증키 설정
<img src="../../images/ncp_server_vpc_create_04.jpg" alt="VPC 환경에서 서버 생성" style="width:600px;align:center">

## 네트워크 접근 설정 (ACL)
<img src="../../images/ncp_server_vpc_create_05.jpg" alt="VPC 환경에서 서버 생성" style="width:600px;align:center">


## 참고 URL
<a href="https://docs.ncloud.com/ko/compute/compute-1-1-v2.html" target="_blank" style="word-break:break-all;">https://docs.ncloud.com/ko/compute/compute-1-1-v2.html</a>


> 문서 최종 수정일 : 2021-01-12