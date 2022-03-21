---
date: 2021-05-27
last_modified_at: 2021-05-27
title: Global DNS 사용 가이드 - 도메인 추가
categories:
  - 2.networking
description: 네이버 클라우드 Global DNS 사용 방법 중에서 도메인과 레코드 추가에 대한 가이드입니다
type: Document
set: dns
order_number: 7
creator: franky
v2_link: /networking/ncloud_networking_global_dns_guide.html
---

## 개요
DNS 서버는 개인이 직접 운영하기 어려운데, 네이버 클라우드 Global DNS를 이용하면 도메인 설정 등을 쉽고 편하게 사용할 수 있습니다.

## 도메인 추가
네이버 클라우드는 도메인 구매/등록을 지원 하지 않습니다. 그러므로 가비아, 아이네임즈, 닷네임코리아 등 전문 도메인 등록 기관에서 도메인을 구입 하셔야 합니다.  
새로 구입한 도메인 혹은 기존 도메인을 [네이버 클라우드 콘솔] - [Networking] - [Global DNS] 메뉴에서 [도메인 추가] 버튼을 클릭해 도메인주소를 입력합니다.

<img src="/images/ncp_networking_global_dns_guide_01.jpg" alt="네이버 클라우드 Global DNS 등록 가이드 " style="width:770px;align:center">

<img src="/images/ncp_networking_global_dns_guide_02.jpg" alt="네이버 클라우드 Global DNS 등록 가이드 " style="width:496px;align:center">

다음과 같이 생성된 도메인 정보에서 네임서버의 주소를 확인합니다.

<img src="/images/ncp_networking_global_dns_guide_03.jpg" alt="네이버 클라우드 Global DNS 등록 가이드 " style="width:770px;align:center">

도메인을 구입한 등록기관 사이트에서 해당 도메인의 네임서버를 위에서 확인한 네이버 클라우드 Global DNS에서 제공하는 네임서버 정보로 등록합니다.

<img src="/images/ncp_networking_global_dns_guide_04.jpg" alt="네이버 클라우드 Global DNS 등록 가이드 " style="width:770px;align:center">


## 레코드 추가
레코드를 추가하려면 도메인 정보에서 [레코드 추가]를 클릭합니다.

<img src="/images/ncp_networking_global_dns_guide_05-1.jpg" alt="네이버 클라우드 Global DNS 등록 가이드 " style="width:770px;align:center">


추가하려고 하는 레코드 즉, 호스트명과 IP 주소를 입력합니다,

<img src="/images/ncp_networking_global_dns_guide_05.jpg" alt="네이버 클라우드 Global DNS 등록 가이드 " style="width:600px;align:center">


추가된 레코드를 도메인 정보에서 아래와 같이 확인한 후, [설정 적용] 버튼을 클릭합니다.

<img src="/images/ncp_networking_global_dns_guide_06.jpg" alt="네이버 클라우드 Global DNS 등록 가이드 " style="width:770px;align:center">


[배포] 버튼을 클릭해 변경된 정보를 배포-적용합니다.

<img src="/images/ncp_networking_global_dns_guide_07.jpg" alt="네이버 클라우드 Global DNS 등록 가이드 " style="width:600px;align:center">

네이버 클라우드 네임서버를 클라이언트의 DNS로 설정하지 않은 경우, 레코드의 추가/변경 내역이 반영되는데 전파시간이 소요될 수 있습니다.

<img src="/images/ncp_networking_global_dns_guide_08.jpg" alt="네이버 클라우드 Global DNS 등록 가이드 " style="width:469px;align:center">


## 참고 URL
<a href="https://guide.ncloud-docs.com/docs/networking-networking-12-1" target="_blank" style="word-break:break-all;">https://guide.ncloud-docs.com/docs/networking-networking-12-1</a>
