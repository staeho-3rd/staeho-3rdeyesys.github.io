---
date: 2020-11-26
update: 2021-09-01
title: ACG(Access Control Group) 가이드
categories:
  - 3.security
description: 네이버 클라우드 ACG(Access Control Group) 가이드
type: Document
set: server
order_number: 6
---

## 개요
ACG(Access Control Group)는 서버 간 네트워크 접근 제어 및 관리를 할 수 있는 IP/Port 기반 필터링 방화벽 서비스로 AWS에서는 비슷하게 Security Group이라는 것이 있습니다.

## 제한 사항

### VPC
- VPC당 최대 **500개**까지 ACG 생성 가능
- `NIC당 3개의 ACG`를 허용
- Inbound / Outbound 각각 **50개의 규칙** 생성 가능

### Classic
- 계정당 최대 **100개**까지 ACG를 생성 가능
- 각 ACG에는 최대 **100개**까지의 규칙을 설정할 수 있음
- 서버는 **최대 5개**의 ACG에 중복 포함될 수 있음
- 서버가 생성될 시 선택한 ACG는 변경이 불가하며, 반납 전까지 해당 ACG 규칙을 적용 받게 됨

> Classic 환경에서는 서버 자체에 할당되는 개념이었으나 VPC에는 NIC 즉, 네트워크 카드에 할당되는 개념이어서 VPC 환경에서는 **NIC 당 최대 3개**까지 ACG를 적용할 수 있다.

## 기본 규칙

### Default ACG
기본적으로 추가되는 ACG

- 모든 들어오는 연결(inbound traffic)을 차단함
- 모든 나가는 연결(outbound traffic)을 허용함
- Default ACG 내 속한 서버들끼리의 네트워크 양방향 통신(TCP, UDP, ICMP)이 허용됨
- 원격 접속 기본 포트 (**Linux - 22, Windows - 3389**)에 대한 TCP 허용됨

#### VPC 화면

##### Inbound
<img src="../../images/ncp_server_acg_vpc_inbound.png" alt="VPC Default ACG Inbound " style="width:770px;align:center">

> 기본으로 생성된 ACG에는 위처럼 22, 3389 포트에 대해 0.0.0.0/0 전체 IP에 대해 허용되어 있는데 보안을 위해 이 항목을 삭제하고 지정된 IP에서만 접속하도록 수정하는 것이 좋습니다.

##### Outbound
<img src="../../images/ncp_server_acg_vpc_outbound.png" alt="VPC Default ACG Outbound " style="width:770px;align:center">

#### Classic 화면
<img src="../../images/ncp_server_acg_classic.png" alt="Classic Default ACG " style="width:770px;align:center">

> 기본으로 생성된 ACG에는 위처럼 22, 3389 포트에 대해 0.0.0.0/0 전체 IP에 대해 허용되어 있는데 보안을 위해 이 항목을 삭제하고 지정된 IP에서만 접속하도록 수정하는 것이 좋습니다.

### Custom ACG
Default ACG 이외에 사용자가 추가하는 ACG

- 모든 inbound traffic을 차단함(규칙으로 명시되어 있지 않음)
- 모든 outbound traffic을 허용함(규칙으로 명시되어 있지 않음)


## 참고 URL
<a href="https://guide.ncloud-docs.com/docs/compute-compute-2-3" target="_blank" style="word-break:break-all;">https://guide.ncloud-docs.com/docs/compute-compute-2-3.html</a>


> 문서 최종 수정일 : 2021-09-01