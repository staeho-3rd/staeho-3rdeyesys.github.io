---
date: 2021-11-22
update: 2021-11-25
title: VPC Peering 생성 가이드
categories:
  - 2.networking
description: 네이버 클라우드 VPC 환경에서 VPC Peering 생성 가이드입니다.
type: Document
set: VPC
order_number: 10
creator: franky
---

## 개요
VPC(virtural Private Cloud)는 퍼블릭클라우드상에서 제공되는 사설 가상 네트워크 입니다. 계정당 3개의 VPC를 만들수 있으며 다른 VPC 네트워크와 논리적으로 분리되어 있어 독립적인 네트워크 환경을 구현할 수 있습니다.  
그런데, 간혹 VPC 환경에서 분리되어 있는 VPC간의 통신이 필요할때가 있는데 이때 사용할 수 있는 서비스가 [**VPC Peering**] 입니다. VPC Peering은 공인 아이피를 거치지 않고 Ncloud 내부 네트워크를 이용하여 VPN없이 VPC간 통신을 할수 있게 해주는 서비스 입니다.

## VPC 생성
우선 테스트에 사용할 VPC로 [test-vpc], [test2-vpc] 이렇게 2개를 준비했습니다.

<img src="../../images/ncloud_networking_vpc_peering_guide_01.png" alt="네이버 클라우드 VPC 환경에서 VPC Peering 생성 가이드 " style="width:770px;align:center">

## Subnet 생성 

이제 각 VPC에 서브넷을 생성합니다. [test-vpc]에는 [test-subnet(192.168.10.0/24)]으로 생성합니다.

<img src="../../images/ncloud_networking_vpc_peering_guide_07.png" alt="네이버 클라우드 VPC 환경에서 VPC Peering 생성 가이드 " style="width:700px;align:center">

[test2-vpc]에는 [test2-subnet(172.16.10.0/24)]으로 생성합니다.

<img src="../../images/ncloud_networking_vpc_peering_guide_08.png" alt="네이버 클라우드 VPC 환경에서 VPC Peering 생성 가이드 " style="width:700px;align:center">

생성된 Subnet은 다음과 같습니다.

<img src="../../images/ncloud_networking_vpc_peering_guide_09.png" alt="네이버 클라우드 VPC 환경에서 VPC Peering 생성 가이드 " style="width:770px;align:center">


## VPC Peering 생성
네이버 클라우드(Ncloud) 콘솔에서 [Networking] -> [VPC] -> [VPCPeering] 메뉴로 이동해서 [VPC peerig 생성] 버튼을 클릭합니다.

<img src="../../images/ncloud_networking_vpc_peering_guide_02.png" alt="네이버 클라우드 VPC 환경에서 VPC Peering 생성 가이드 " style="width:770px;align:center">

- ① 이름을 적고
- ② 요청 VPC는 [testVPC]를 선택
- ③ 수락 VPC는 [내계정], [test2VPC]를 선택

설정이 끝났으면 생성버튼을 클릭합니다.

>다른 계정의 VPC와 연결하는 경우는 아래쪽에서 살펴보겠습니다.

<img src="../../images/ncloud_networking_vpc_peering_guide_03.png" alt="네이버 클라우드 VPC 환경에서 VPC Peering 생성 가이드 " style="width:700px;align:center">

마지막으로 VPC Peering 요청 내용을 확인하고 [확인] 버튼을 클릭합니다.

<img src="../../images/ncloud_networking_vpc_peering_guide_17.png" alt="네이버 클라우드 VPC 환경에서 VPC Peering 생성 가이드 " style="width:500px;align:center">

[test-vpc] -> [test2-vpc]로 설정된 VPC Peering을 아래와 같이 확인할 수 있습니다.  

<img src="../../images/ncloud_networking_vpc_peering_guide_04.png" alt="네이버 클라우드 VPC 환경에서 VPC Peering 생성 가이드 " style="width:770px;align:center">

#### 역방향 설정 추가
그런데 VPC peering은 단방향 통신이기에 TCP, ICMP 등의 양방향 통신을 하는 프로토콜을 이용하려면 역방향 즉, [test2-vpc] -> [test-vpc]로 설정된 VPC Peering도 추가해야 합니다.

- ① 이름을 적고
- ② 요청 VPC에는 test2VPC를 선택
- ③ 수락 VPC에는 testVPC를 선택

<img src="../../images/ncloud_networking_vpc_peering_guide_05.png" alt="네이버 클라우드 VPC 환경에서 VPC Peering 생성 가이드 " style="width:700px;align:center">

VPC Peering 요청 내용을 확인하고 [확인] 버튼을 클릭합니다. 

<img src="../../images/ncloud_networking_vpc_peering_guide_18.png" alt="네이버 클라우드 VPC 환경에서 VPC Peering 생성 가이드 " style="width:500px;align:center">

아래와 같이 [test-vpc] -> [test2-vpc] , [test2-vpc] -> [test-vpc] 2가지 VPC Peering을 모두 생성했으므로 양방향 통신이 가능하게 되었습니다.

<img src="../../images/ncloud_networking_vpc_peering_guide_06.png" alt="네이버 클라우드 VPC 환경에서 VPC Peering 생성 가이드 " style="width:770px;align:center">


## Route Table 설정

이제 통신할 서브넷 혹은 서버의 아이피를 Route Table 설정에 추가 합니다. 여기서는 서브넷을 추가하도록 하겠습니다.

우선 [VPC] - [Route Table]에서, [test-vpc-default-public-table]의 [Routes 설정]을 클릭합니다.

<img src="../../images/ncloud_networking_vpc_peering_guide_11.png" alt="네이버 클라우드 VPC 환경에서 VPC Peering 생성 가이드 " style="width:770px;align:center">

- Destination에는 [test2-subnet]의 아이피 대역을 입력 (서버의 아이피로 입력해도 됨)  
- Target Type은 [VPCPEERING]을 선택
- Target Name은 [test-vpc-peering]을 선택

<img src="../../images/ncloud_networking_vpc_peering_guide_12.png" alt="네이버 클라우드 VPC 환경에서 VPC Peering 생성 가이드 " style="width:770px;align:center">

다음으로 [test2-vpc-default-public-table]의 [Routes 설정]을 클릭합니다.

<img src="../../images/ncloud_networking_vpc_peering_guide_13.png" alt="네이버 클라우드 VPC 환경에서 VPC Peering 생성 가이드 " style="width:770px;align:center">

- Destination에는 [test-subnet]의 아이피 대역을 입력 (서버의 아이피로 입력해도 됨)  
- Target Type은 [VPCPEERING]을 선택
- Target Name은 [test2-vpc-peering]을 선택

<img src="../../images/ncloud_networking_vpc_peering_guide_14.png" alt="네이버 클라우드 VPC 환경에서 VPC Peering 생성 가이드 " style="width:770px;align:center">

## 서버 준비
아래와 같이 테스트로 사용할 서버 2대를 준비했습니다.  
테스트는 [test-vpc]에 위치한 [test-vpc-peering-svr]서버 -> [test2-vpc]에 위치한 [test2-vpc-peering-svr]로 접속 시도를 해보겠습니다.

<img src="../../images/ncloud_networking_vpc_peering_guide_15.png" alt="네이버 클라우드 VPC 환경에서 VPC Peering 생성 가이드 " style="width:770px;align:center">

## ACG 설정
ACG를 설정하지 않고 [test-vpc-peering-svr] -> [test2-vpc-peering-svr]로 접속 시도를 하면 접속이 되지 않는 것을 확인할 수 있습니다.

<img src="../../images/ncloud_networking_vpc_peering_guide_19.png" alt="네이버 클라우드 VPC 환경에서 VPC Peering 생성 가이드 " style="width:770px;align:center">

[test2-vpc-peering-svr]로 접속할 것이므로 해당 서버에 설정된 acg인 [test2-vpc-default-acg]를 선택하고 [ACG 설정] 버튼을 클릭합니다. 

<img src="../../images/ncloud_networking_vpc_peering_guide_20.png" alt="네이버 클라우드 VPC 환경에서 VPC Peering 생성 가이드 " style="width:770px;align:center">

접근소스에는 [test-vpc-peering-svr] 서버의 subnet 대역인 [192.168.10.0/24]를 입력하고, 포트는 22번 포트를 입력하고 추가해서 적용합니다.

<img src="../../images/ncloud_networking_vpc_peering_guide_21.png" alt="네이버 클라우드 VPC 환경에서 VPC Peering 생성 가이드 " style="width:770px;align:center">

## 접속 테스트 
ACG 설정까지 완료하고 나서 다시 접속 테스트를 해보면 아래와 같이 접속이 잘 되는 것을 확인할 수 있습니다.

<img src="../../images/ncloud_networking_vpc_peering_guide_22.png" alt="네이버 클라우드 VPC 환경에서 VPC Peering 생성 가이드 " style="width:770px;align:center">

## 다른 계정 VPC 연결
위에서는 동일한 내 계정에 생성된 VPC들 간의 Peering을 살펴보았는데, 아래에서는 다른 계정에 생성된 VPC와 연결할 때의 화면을 살펴보겠습니다.

VPC Peering 생성 화면에서 [다른 계정]을 선택하면 아래와 같이 [로그인 ID (이메일)], [VPC ID], [VPC 이름]을 입력하게 됩니다.

<img src="../../images/ncloud_networking_vpc_peering_guide_23.png" alt="네이버 클라우드 VPC 환경에서 VPC Peering 생성 가이드 " style="width:700px;align:center">

마찬가지로 VPC Peering 요청 내용을 확인하고 [확인] 버튼을 클릭합니다.

<img src="../../images/ncloud_networking_vpc_peering_guide_24.png" alt="네이버 클라우드 VPC 환경에서 VPC Peering 생성 가이드 " style="width:500px;align:center">

다음으로 수락을 요청받은 다른 계정의 VPC Peering 화면에 가면 요청 내용을 확인할 수 있고, 요청 응답에서 [수락] 버튼을 클릭합니다. 

<img src="../../images/ncloud_networking_vpc_peering_guide_25.png" alt="네이버 클라우드 VPC 환경에서 VPC Peering 생성 가이드 " style="width:770px;align:center">

한번 더 VPC Peering 연결 요청을 수락할 것인지 확인하는 창이 나타납니다. 

<img src="../../images/ncloud_networking_vpc_peering_guide_26.png" alt="네이버 클라우드 VPC 환경에서 VPC Peering 생성 가이드 " style="width:500px;align:center">

수락하고 나면 역방향의 VPC Peering 연결을 생성해야 한다는 안내와 함께 역방향 VPC Peering 생성 화면이 나타납니다.  
역방향은 위의 설정과 반대로 진행하면 되고, 그 이후에는 내 계정에서 설정했던 것과 마찬가지로 설정하시면 완료됩니다.

<img src="../../images/ncloud_networking_vpc_peering_guide_27.png" alt="네이버 클라우드 VPC 환경에서 VPC Peering 생성 가이드 " style="width:500px;align:center">

## 제한사항
- VPC Peering은 연결하려는 VPC들의 IP주소 대역이 달라야 합니다. 일치되거나 중첩되는 대역이 있으면 설정되지 않습니다.
- VPC Peering은 단방향입니다. TCP등 양방향 통신을 해야 하는 경우에는 요청 / 수락 VPC를 맞바꾸어 역방향 Peering도 추가 생성해야 합니다.
- VPC Peering은 전이적 연결 관계를 지원하지 않습니다. 즉, Peering된 VPC를 통하여 다른 VPC 혹은 외부로 통신하는 것은 불가능 합니다.
- VPC Peering은 동일한 리전 내 VPC 끼리만 연결할 수 있습니다.

## 참고 URL
1. VPC Peering 가이드
	- <a href="https://guide.ncloud-docs.com/docs/networking-vpc-vpcdetailedpeering" target="_blank" style="word-break:break-all;">https://guide.ncloud-docs.com/docs/networking-vpc-vpcdetailedpeering</a>

2. VPC Peering 설정 시나리오
	- <a href="https://guide.ncloud-docs.com/docs/networking-vpc-vpcuserscenario4" target="_blank" style="word-break:break-all;">https://guide.ncloud-docs.com/docs/networking-vpc-vpcuserscenario4</a>


> 문서 최종 수정일 : 2021-11-25
