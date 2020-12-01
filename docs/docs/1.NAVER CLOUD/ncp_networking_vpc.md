# VPC 구성요소

## 개요
이 문서는 VPC를 구성하는 요소들에 대한 설명으로 네이버 클라우드 파트너 테크데이에서 발표된 내용을 정리한 것입니다.

## VPC
VPC(Virtual Private Cloud)는 퍼블릭 클라우드 상에 논리적으로 완전하게 분리된 고객전용 네트워크를 제공하는 서비스.
최대 /16의 IP 네트워크 공간을 제공 (IP 대역: RFC 1918).

!!! tip "RFC 1978 IP대역"
	10.0.0.0/8 (10.0.0.0 - 10.255.255.255)  
	172.16.0.0/12 (172.16.0.0 - 172.31.255.255)  
	192.168.0.0/16 (192.168.0.0 - 192.168.255.255)


## Subnet (Internet Gateway)
할당된 VPC를 용도에 맞게 네트워크 공간을 세분화 하여 사용.  
/16 ~ /28의 네트워크 주소 할당이 가능.  
Public Subet 생성 시 Internet Gateway가 연결됨.

## NAT Gateway
Network Address Translation의 약자로, 폐쇄된 네트워크에서 외부와의 인터넷 동신 시 사용하는 게이트웨이.

## Route Table
네트워크 경로를 설정할 수 있는 기능을 제공. VPC 내부 통신을 위한 Local은 기본적으로 설정.

## ACG
서버에서 인바운드/아웃바운드의 네트워크 접근제어를 지원하며 Stateful 기반으로 동작.

## NACL
Network Access Control List의 약자로, Subnet에서 인바운드/아웃바운드의 네트워크 접근제어를 지원하며 Stateless 기반으로 동작.

## Virtual Private Gateway
Cloud Connect와 IPSec VPN에 연결되는 네이버 클라우드의 VPC측 연결 접점으로서 Cloud Connect와 IPSec VPN 연결을 지원.

## VPC Peering
VPC간 사설연결을 보장하는 기능으로, 일반적인 VPC <-> VPC 간의 통신은 인터넷을 통하게 되고, 이는 과다한 요금 발생 및 성능 저하를 일으킬 수 있음.  
VPC Peering을 이용하면 보다 안전한 사설 IP기반의 통신이 가능함.  
VPC Peering은 단방향 통신을 제공하기 때문에 양방향 통신을 원하면 Src -> Dest 별로 각각 1개씩, 두개의 정책을 모두 적용해야 함.


<img src="../../img/ncp_vpc_element.png" alt="VPC 구성요소" style="width:800px;align:center">


## 참고 URL
<a href="https://docs.ncloud.com/ko/networking/vpc/vpc_overview.html" target="_blank">https://docs.ncloud.com/ko/networking/vpc/vpc_overview.html</a>


!!! info "문서 최종 수정일 : 2020-11-30" 