---
date: 2021-07-09
last_modified_at: 2021-07-09
title: VPC 환경에서 NAT Gateway 설정하기
categories:
  - 2.networking
description: 네이버 클라우드 VPC 환경에서 NAT Gateway 설정하기 가이드입니다.
type: Document
set: Load Balancer
order_number: 9
creator: ljh0519
v2_link: /networking/ncloud_networking_natgw_routetb.html
---

## 개요
NAT Gateway는 비공인 IP를 가진 다수의 서버들이 대표 공인 IP를 이용해 외부와 통신을 할 수 있도록 도와주는 네트워킹 서비스입니다.  
그리고, 일반적으로 서버에 직접 공인 IP를 부여하는 것과 달리 외부에서 서버로의 직접 접근은 허용하지 않기 때문에 높은 수준의 보안을 유지할 수 있는 것이 특징입니다.  
여기서는 VPC 환경에서 NAT Gateway를 어떻게 구성하고, NAT Gateway를 적용하기 전과 후의 외부와 통신 상태에 대해 확인해보도록 하겠습니다.

## 생성개수 제한
우선 VPC는 리전별로 계정당 3개씩 생성 가능하고, **NAT Gateway는 각 VPC별로 Zone당 1개씩 생성 가능**합니다.  
현재 한국 리전에는 KR-1, KR-2 이렇게 2개의 Zone이 있으므로 한국 리전 기준으로 VPC당 최대 2개, **계정당 최대 6개의 NAT Gateway를 생성**할 수 있습니다.

## VPC 생성
먼저 VPC를 생성합니다. IP주소 범위는 10.0.0.0/16으로 정하겠습니다.

<img src="../../images/ncp_networking_natgw_routetb_01.jpg" alt="네이버 클라우드 VPC 환경에서 VPC 환경에서 NAT Gateway 설정하기 가이드 " style="width:700px;align:center">

## Subnet 생성
NAT Gateway 동작 테스트를 위해 Public, Private 이렇게 2가지 Subnet을 생성하겠습니다.

#### Public Subnet 생성
Public Subnet의 IP 범위는 **10.0.0.0/24**로 설정하겠습니다.

<img src="../../images/ncp_networking_natgw_routetb_02.jpg" alt="네이버 클라우드 VPC 환경에서 VPC 환경에서 NAT Gateway 설정하기 가이드 " style="width:700px;align:center">

#### Private Subnet 생성
Private Subnet의 IP 범위는 **10.0.0.1/24**로 설정하겠습니다.

<img src="../../images/ncp_networking_natgw_routetb_03.jpg" alt="네이버 클라우드 VPC 환경에서 VPC 환경에서 NAT Gateway 설정하기 가이드 " style="width:700px;align:center">

## ACG 설정
테스트를 위한 ACG (Access Control Group)를 생성하고 설정합니다.

<img src="../../images/ncp_networking_natgw_routetb_04.jpg" alt="네이버 클라우드 VPC 환경에서 VPC 환경에서 NAT Gateway 설정하기 가이드 " style="width:700px;align:center">

#### Inbound 설정
Inbound 규칙에는 Public, Private Subnet과 SSH 접속을 위한 로컬PC IP를 허용하고, Ping 테스트를 위한 ICMP도 허용합니다.

<img src="../../images/ncp_networking_natgw_routetb_05.jpg" alt="네이버 클라우드 VPC 환경에서 VPC 환경에서 NAT Gateway 설정하기 가이드 " style="width:770px;align:center">

#### Outbound 설정
Outbound 규칙은 TCP, UDP, ICMP 모두 전체 허용으로 추가합니다.

<img src="../../images/ncp_networking_natgw_routetb_06.jpg" alt="네이버 클라우드 VPC 환경에서 VPC 환경에서 NAT Gateway 설정하기 가이드 " style="width:770px;align:center">

## 서버 생성
테스트를 위해 Public, Private 각각의 Subnet에 1대씩 서버를 설정하고 Public 서버에는 공인 IP도 할당합니다.

<img src="../../images/ncp_networking_natgw_routetb_07.jpg" alt="네이버 클라우드 VPC 환경에서 VPC 환경에서 NAT Gateway 설정하기 가이드 " style="width:770px;align:center">

#### Public -> Private 서버 접속 확인
Public Subnet에 있는 서버에서 Private Subnet에 있는 서버로 통신이 가능한 것을 확인할 수 있습니다.  
다음 단계에서 Private 서버로 SSH로 접속하기 위한 사전 확인 단계입니다.

<img src="../../images/ncp_networking_natgw_routetb_08.jpg" alt="네이버 클라우드 VPC 환경에서 VPC 환경에서 NAT Gateway 설정하기 가이드 " style="width:670px;align:center">

#### Private 서버 외부 접속 여부 확인
Public 서버에서 Private 서버로 SSH로 접속한 후에 Private 서버에서 외부로 통신이 되는 것을 확인해보면 불가능한 것을 확인할 수 있습니다.  
이후 단계에서 NAT Gateway를 설정하고 Private 서버에서 외부와 통신이 가능한지 확인해보겠습니다.

<img src="../../images/ncp_networking_natgw_routetb_09.jpg" alt="네이버 클라우드 VPC 환경에서 VPC 환경에서 NAT Gateway 설정하기 가이드 " style="width:670px;align:center">

## Route Table 설정 확인
NAT Gateway를 위한 Route Table 설정을 추가하기 전에 현재 상태의 Route Table 설정을 확인해보겠습니다.

#### Public Subnet의 Route Table 설정 확인
위에서 Public Subnet의 서버와 Private Subnet의 서버가 통신이 가능했던 것은 VPC와 Subnet이 생성될때 Route Table이 생성되면서 VPC 내부 통신을 위한 규칙 (10.0.0.0/16 LOCAL)이 기본으로 설정되기 때문입니다.  
그리고 Public의 경우 외부 통신을 위한 INTERNET GATEWAY가 추가되어 있는 것을 확인할 수 있습니다.

<img src="../../images/ncp_networking_natgw_routetb_10.jpg" alt="네이버 클라우드 VPC 환경에서 VPC 환경에서 NAT Gateway 설정하기 가이드 " style="width:770px;align:center">

#### Private Subnet의 Route Table 설정 확인
반면에 Private의 경우는 VPC 내부 통신을 위한 LOCAL만 설정된 것을 확인 할 수 있습니다.

<img src="../../images/ncp_networking_natgw_routetb_11.jpg" alt="네이버 클라우드 VPC 환경에서 VPC 환경에서 NAT Gateway 설정하기 가이드 " style="width:770px;align:center">

Route Table 설정 화면에 들어가도 Target Type에 LOCAL만 선택할 수 있는 것을 확인할 수 있습니다.
<img src="../../images/ncp_networking_natgw_routetb_14.jpg" alt="네이버 클라우드 VPC 환경에서 VPC 환경에서 NAT Gateway 설정하기 가이드 " style="width:770px;align:center">

## NAT Gateway 생성
NAT Gateway 생성 버튼을 클릭합니다.

<img src="../../images/ncp_networking_natgw_routetb_15.jpg" alt="네이버 클라우드 VPC 환경에서 VPC 환경에서 NAT Gateway 설정하기 가이드 " style="width:770px;align:center">

이름을 입력하고 VPC와 Zone을 선택하고 NAT Gateway를 생성합니다.

<img src="../../images/ncp_networking_natgw_routetb_16.jpg" alt="네이버 클라우드 VPC 환경에서 VPC 환경에서 NAT Gateway 설정하기 가이드 " style="width:770px;align:center">

#### Route Table 설정
앞에서 LOCAL 항목만 존재했던 Private Subnet의 Route Table 설정 화면에 가보면 Target Type에 NATGW가 추가된 것을 확인할 수 있습니다.

<img src="../../images/ncp_networking_natgw_routetb_18.jpg" alt="네이버 클라우드 VPC 환경에서 VPC 환경에서 NAT Gateway 설정하기 가이드 " style="width:770px;align:center">

목적지(Destination)에 0.0.0.0/0을 입력하고, Target Type을 NATGW를 선택하고 생성 버튼을 클릭합니다.

<img src="../../images/ncp_networking_natgw_routetb_19.jpg" alt="네이버 클라우드 VPC 환경에서 VPC 환경에서 NAT Gateway 설정하기 가이드 " style="width:770px;align:center">

## 외부 접속 테스트
마지막으로 다시 한번 Private 서버에서 외부 통신이 가능한지 ping 테스트를 해보면, 아래 화면과 같이 정상적으로 통신이 되는 것을 확인할 수 있습니다.

<img src="../../images/ncp_networking_natgw_routetb_20.jpg" alt="네이버 클라우드 VPC 환경에서 VPC 환경에서 NAT Gateway 설정하기 가이드 " style="width:670px;align:center">


## 고려사항
NAT Gateway는 사용하지 않고 생성만 해두어도 요금이 부과됩니다. (데이터 처리요금 별도)  
생성 후 보유 시간에 따라 요금이 부과되는데 1개당 56원/시간 입니다. 그러므로 1달간 보유하고만 있다고 가정할 때 비용을 계산해보면  
**24시간 * 30일 * 56원 = 40,320원** 그러므로 사용하지 않는 NAT Gatway는 반드시 삭제하기를 추천드립니다.



## 참고 URL
1. NAT Gateway 가이드
	- <a href="https://guide.ncloud-docs.com/docs/networking-vpc-vpcdetailenatgw" target="_blank" style="word-break:break-all;">https://guide.ncloud-docs.com/docs/networking-vpc-vpcdetailenatgw</a>

2. NAT Gateway 특징과 비교
	- <a href="https://m.blog.naver.com/n_cloudplatform/221173116642" target="_blank" style="word-break:break-all;">https://m.blog.naver.com/n_cloudplatform/221173116642</a>
