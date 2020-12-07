# ACG와 NACL 비교

## 개요
네이버 클라우드에서는 VPC의 보안을 강화하기 위해 ACG와 NACL의 두 가지 보안 정책을 제공하고 있습니다.

1. ACG : Access Control Group은 서버의 NIC별 Inbound 및 Outbound 트래픽을 제어
2. NACL : Network Access Control List는 Subnet의 Inbound 및 Outbound 트래픽을 제어

## ACG vs NACL
| 구분 | ACG | NACL |
| :----: | :----: | :-----: |
| 적용 대상 | 서버의 접근 제어 | Subnet의 접근 제어 |
| 지원 규칙 | 허용 (Allow) | {==**허용 및 거부 (Allow / Deny)**==} |
| 상태 저장 여부 | 상태 저장(Stateful)<br>(규칙에 관계없이 반환 트래픽이<br>자동으로 허용됨) | 상태 비저장(Stateless)<br>(반환 트래픽이 규칙에 의해<br>명시적으로 허용되어야 함) |
| 적용 방법 | 서버의 NIC에 ACG 정책 적용 | Subnet 단위로 적용<br>(Subnet 별 1개만 허용) |


<img src="../img/ncp_vpc_acg_nacl.png" alt="VPC ACG와 NACL 비교" style="width:800px;align:center">

## 참고 URL
<a href="https://docs.ncloud.com/ko/networking/vpc/vpc_detailedsubnet.html" target="_blank">https://docs.ncloud.com/ko/networking/vpc/vpc_detailedsubnet.html</a>


!!! info "문서 최종 수정일 : 2020-12-01"
