# [Security] NCP : On-Premise 비교

## 개요
기존의 IDC 등의 On-Premise 환경에서 사용하고 있는 보안 서비스를 네이버 클라우드 환경에서 어떻게 구현할 수 있는지에 대한 비교 가이드입니다.  
IDC에 있는 서버들을 네이버 클라우드로 마이그레이션 할 때 참고하시면 되겠습니다.

## On-Premise → Naver Cloud
| 구분 | On-Premise |  | Naver Cloud | 설명 |
| :---: | :---: | :---: | :--- | :--- |
| Network | DDos | <h3>→</h3> | Security Monitoring | Security Monitoring DDos 서비스를 통해 고객별 특화된 탐지 정책을 적용 |
|  | 방화벽 | <h3>→</h3> | ACG(Access Control Group) | ACG Rule 변경 기능으로 서버 접속을 허용할 트래픽 규칙을 안전하고 편리하게 관리 |
|  | IDS/IPS | <h3>→</h3> | Security Monitoring | Security Monitoring IDS/IPS 서비스를 통해 고객별 특화된 탐지/차단 정책을 적용 |
|  | 전송구간 암호화 | <h3>→</h3> | IPSec/SSL VPN, Cloud Connect | 고객의 네트워크와 네이버 클라우드에 있는 네트워크에 대한 안전한 연결을 제공 |
| DB | DB 접근 통제 | <h3>→</h3> | Naver Cloud MarketPlace | MarketPlace에서 제공하는 3rd-Party 솔루션을 VM에 설치하여 사용 |
|  | DB 암호화 | <h3>→</h3> | Naver Cloud MarketPlace | MarketPlace에서 제공하는 3rd-Party 솔루션을 VM에 설치하여 사용 |
| Server | 서버접근통제 | <h3>→</h3> | SSL VPN, Naver Cloud MarketPlace | SSL VPN을 이용해 서버 접근을 관리  MarketPlace에서 제공하는 3rd-Party 솔루션을 VM에 설치하여 사용 |
|  | 서버보안(SecureOS) | <h3>→</h3> | Naver Cloud MarketPlace | MarketPlace에서 제공하는 3rd-Party 솔루션을 VM에 설치하여 사용 |
|  | Anti Virus | <h3>→</h3> | Security Monitoring | Security Monitoring DDos 악성코드 의심 이벤트 발생 시 탐지 보고서 및 분석 정보 전달 |
| Application | 웹 방화벽 | <h3>→</h3> | Security Monitoring | Security Monitoring WAF서비스를 통해 고객별 특화된 탐지/차단 정책을 적용 |
|  | Anti-Webshell | <h3>→</h3> | Naver Cloud MarketPlace | Naver Cloud MarketPlace | MarketPlace에서 제공하는 3rd-Party 솔루션을 VM에 설치하여 사용 |
| User Access | 사용자 접근통제 | <h3>→</h3> | Sub Account | Sub Account 서비스를 이요하여 콘솔 접근에 대한 사용자 접속을 관리 |
| Audit | - | <h3>→</h3> | Cloud Activity Tracer / Resource Manager | 리소스(서버, 네트워크, DB등) 생성, 변경, 삭제에 대해 추적 기능을 제공 |
| Key Management | - | <h3>→</h3> | Key Management Service | Key에 대한 접근 제어 기능을 이용하여 데이터 암호화 키를 안전하게 보호하고 관리 |


<img src="../../img/ncp_security_ncp_onpremise_compare.png" alt="Security 서비스 NCP : On-Premise 비교" style="width:800px;align:center">

## 참고 URL
<a href="https://docs.ncloud.com/ko/security/security-1-1.html" target="_blank">https://docs.ncloud.com/ko/security/security-1-1.html</a>


!!! info "문서 최종 수정일 : 2020-12-02"
