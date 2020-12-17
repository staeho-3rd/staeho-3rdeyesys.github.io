# Subnet 과 NAT GW

## 개요
네이버 클라우드에서는 VPC의 보안을 강화하기 위해 두 가지 서브넷을 제공하고 있습니다.

1. Public Subnet : 인터넷과 자유로운 통신이 필요할 때 사용할 수 있는 서브넷으로 Interget GW를 통해 외부와 통신
2. Private Subnet : 보안상 외부에서 서버에 접근하는 것을 막아야 할 때 사용할 수 있는 서브넷으로 NAT GW를 통해 외부와 통신

## Public vs Private Subnet
| 구분 | Public Subnet | Private Subnet |
| :----: | :----: | :-----: |
| 용도 | 인터넷 연결이 필요할 때 | 외부 접속을 최소화 해야 할 때 |
| 지원 리소스 | 서버 | 서버, 로드밸런서 |
| 인터넷 연결 시<br>필요한 리소스 | Internet Gateway (Default) | NAT Gateway |


<img src="../img/ncp_vpc_subnet_natgw.png" alt="VPC Subnet 과 NAT GW" style="width:800px;align:center">

## 참고 URL
<a href="https://docs.ncloud.com/ko/networking/networking-10-1.html" target="_blank">https://docs.ncloud.com/ko/networking/networking-10-1.html</a>


!!! info "문서 최종 수정일 : 2020-12-01"
