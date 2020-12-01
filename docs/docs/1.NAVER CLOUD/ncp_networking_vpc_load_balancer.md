# Load Balancer 상품군의 변화

## 개요
Load Balancer는 수신 트래픽을 다수의 서버로 분산시키는 서비스로서, 수신 트래픽을 등록된 멤버 서버로 분산시켜 가용성을 높이고 시스템 가동률을 조절하는 역할을 수행합니다.  
VPC 플랫폼에서는 Network Load Balancer / Application Load Balancer / Network Proxy Load Balancer 가 제공되어 서비스에 적합한 로드밸런서를 선택할 수 있습니다.

## 종류

1. Application Load Balancer  
HTTP 및 HTTPS 트래픽을 사용하는 웹 애플리케이션을 위한 유연한 기능을 제공

2. Network Load Balancer  
DSR(Direct Server Return) 구조의 고성능, 대규모 네트워크 연결에 적합한 로드밸런서로 **고정 IP**를 제공

3. Network Proxy Load Balancer  
TCP 세련 유지에 최적화 되어 있으며, Network Load Balancer와 다르게 DSR를 지원하지 않으며, Load Balander가 세션을 관리.

!!! tip "KR존/서브넷 별 LB 생성지역 지정 가능"
	VPC 환경에서는 {==**내가 원하는 KR존의 특정 서브넷에 LB생성 가능, KR-1/2 존에 각각 생성하여 고가용성을 확보**==}할 수 있다.

## LB 선택 기준 및 기능 비교
| 기능 | Network LB | Network Proxy LB | Application |
| :----: | :----: | :-----: | :-----: |
| 프로토콜 | TCP | TCP, TLS | HTTP/HTTPS|
| 상태확인 | O | O | O |
| 로깅 | X | O | O |
| DSR | O | X | X |
| 동일 인스턴스의<br>여러 포트로<br>로드밸런싱 | X | X | O |
| HTTP 2.0 | N/A | N/A | O |
| 경로기반 라우팅 | N/A | N/A | O (출시 예정) |
| SSL Offload | X | O | O |
| 고정 세션 | X | O | O |



<img src="../../img/ncp_vpc_load_balancer_01.png" alt="Load Balancer 상품군의 변화" style="width:800px;align:center">
<img src="../../img/ncp_vpc_load_balancer_02.png" alt="Load Balancer 상품군의 변화" style="width:800px;align:center">

## 참고 URL
<a href="https://docs.ncloud.com/ko/networking/loadbalancer/loadbalancer_overview.html" target="_blank">https://docs.ncloud.com/ko/networking/loadbalancer/loadbalancer_overview.html</a>


!!! info "문서 최종 수정일 : 2020-12-01"
