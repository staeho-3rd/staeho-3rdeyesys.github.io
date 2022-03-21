---
date: 2021-10-27
last_modified_at: 2021-10-27
title: Classic 환경에서 SSL VPN 설정하고 접속하는 방법
categories:
  - 3.security
description: Ncloud(네이버 클라우드) Classic 환경에서 SSL VPN 설정하고 접속하는 방법입니다.
type: Document
set: security
order_number: 4
creator: ljh0519
v2_link: /security/ncloud_security_ssl_vpn_classic_guide.html
---

## 개요
Ncloud(네이버 클라우드) 외부에서 내부에 구성된 네트워크로 암호화된 보안 접속 통신을 제공하는 서비스인 SSL VPN을 Classic 환경에서 설정하고, 서버에 접속하는 방법에 대해 정리해보겠습니다.

## SSL VPN이란?
- VPN은 가상 사설망(Virtual Private Network)의 약자로, 외부에서 접근할 수 없는 사설망에 내 PC나 네트워크를 연결시키는 방법을 말합니다.
- 사설망과의 연결은 가상 터널을 통해 이루어지며, 이 가상 터널을 SSL 암호화로 보호하는 것이 SSL VPN입니다.
- 가상 터널을 통해 사설망과 연결된 사용자 PC는 사설망의 라우팅 및 ACL 정책에 따라 내부 서버에 접근할 수 있습니다.


## SSL VPN 생성
[SSL VPN 생성] 버튼을 클릭합니다.

<img src="/images/ncp_security_ssl_vpn_classic_guide_01.png" alt="Ncloud(네이버 클라우드) Classic 환경에서 SSL VPN 설정하고 접속하는 방법" style="width:770px;align:center">

우선 등록 가능한 접속 ID 개수에 따른 상품을 선택합니다. Classic 환경에서는 3개, 5개, 10개 상품만 선택 가능합니다. 
다음으로 인증 방식은 ID/PW 만으로 접속하는 일차인증과 OTP까지 사용하는 이차 인증 중에서 원하는 방식을 선택합니다. 
그리고 이차인증을 선택했을 경우에는 SMS와 Email 어떤 것을 이용할 것인지도 선택하게 됩니다.

{: .error }
인증방식과 OTP 전송 방식은 SSL VPN 생성 시에 한번 선택하면 변경할 수 없으니 주의해야 합니다. 만약 변경을 하고 싶을 경우에는 SSL VPN을 새로 생성해야 합니다. 
Classic 환경에서는 이차인증을 선택하면 별도의 이용요금이 부과됩니다.

<img src="/images/ncp_security_ssl_vpn_classic_guide_02.png" alt="Ncloud(네이버 클라우드) Classic 환경에서 SSL VPN 설정하고 접속하는 방법" style="width:770px;align:center">

다음으로 개인정보 수집 및 이용에 동의해야 합니다.

<img src="/images/ncp_security_ssl_vpn_classic_guide_03.png" alt="Ncloud(네이버 클라우드) Classic 환경에서 SSL VPN 설정하고 접속하는 방법" style="width:700px;align:center">

SSL VPN이 생성되면 다음과 같이 리스트에 서 확인 가능하며 여기서 [**SSL VPN IP POOL**]은 뒤쪽에서 네트워크 접근 제한을 설정할 때 필요한 중요한 정보입니다.

<img src="/images/ncp_security_ssl_vpn_classic_guide_04.png" alt="Ncloud(네이버 클라우드) Classic 환경에서 SSL VPN 설정하고 접속하는 방법" style="width:770px;align:center">


## 사용자 설정
SSL VPN에 접속할 사용자 정보를 설정합니다.  리스트에서 SSL VPN을 선택하고 [사용자 설정] 버튼을 클릭합니다.

<img src="/images/ncp_security_ssl_vpn_classic_guide_05.png" alt="Ncloud(네이버 클라우드) Classic 환경에서 SSL VPN 설정하고 접속하는 방법" style="width:770px;align:center">

사용자 정보는 ID, Passowrd, Email, SMS 등을 입력하고 [추가] 버튼을 클릭합니다.

<img src="/images/ncp_security_ssl_vpn_classic_guide_06.png" alt="Ncloud(네이버 클라우드) Classic 환경에서 SSL VPN 설정하고 접속하는 방법" style="width:770px;align:center">

사용자가 추가되면 SSL VPN 정보에 사용자 계정 수(등록된 ID 수)에 숫자가 표시되는 것을 확인할 수 있습니다.

<img src="/images/ncp_security_ssl_vpn_classic_guide_07.png" alt="Ncloud(네이버 클라우드) Classic 환경에서 SSL VPN 설정하고 접속하는 방법" style="width:770px;align:center">

## ACG 설정
SSL VPN을 사용하려면 서버에 적용된 ACG에 [**SSL VPN IP POOL**]을 등록해야 합니다.  
우선 SSL VPN을 통해서 접속할 서버를 선택하고 적용된 ACG를 확인합니다.

<img src="/images/ncp_security_ssl_vpn_classic_guide_08.png" alt="Ncloud(네이버 클라우드) Classic 환경에서 SSL VPN 설정하고 접속하는 방법" style="width:770px;align:center">

[Server] - [ACG]에서 위에서 확인한 ACG를 선택하고 [ACG 설정]을 클릭합니다. 

<img src="/images/ncp_security_ssl_vpn_classic_guide_09.png" alt="Ncloud(네이버 클라우드) Classic 환경에서 SSL VPN 설정하고 접속하는 방법" style="width:770px;align:center">

ACG 규칙 설정 화면에서 위에서 확인했던 [**SSL VPN IP POOL**]을 접근 소스에 입력하고, 필요한 포트들을 등록합니다. 
기본이 되는 프로토콜과 포트는 리눅스 서버 접속용 TCP 22, 윈도우 서버 접속용 TCP 3389, Ping 확인용 ICMP 등입니다.

<img src="/images/ncp_security_ssl_vpn_classic_guide_10.png" alt="Ncloud(네이버 클라우드) Classic 환경에서 SSL VPN 설정하고 접속하는 방법" style="width:770px;align:center">


## Agent 다운로드
SSL VPN 접속을 위한 Agent를 다운로드 합니다.  

- <a href="https://guide.ncloud-docs.com/docs/security-5-1-windowsdownload" target="_blank" style="word-break:break-all;">윈도우 용 다운로드</a>
- <a href="https://guide.ncloud-docs.com/docs/security-5-1-macdownload" target="_blank" style="word-break:break-all;">맥 용 다운로드</a>

<img src="/images/ncp_security_ssl_vpn_classic_guide_11.png" alt="Ncloud(네이버 클라우드) Classic 환경에서 SSL VPN 설정하고 접속하는 방법" style="width:770px;align:center">

## Agent 접속
Agent 프로그램, BIG-IP Edge Client를 설치하고 실행하면 다음과 같은 화면을 볼 수 있습니다.  
Agent에는 Classic 환경 SSL VPN 서버(Ncloud-kr-01)가 기본으로 설정되어 있는데, 혹시 빠져 있다면 [서버 변경] 버튼을 클릭해서 **https://sslvpn-kr-01.ncloud.com**을 입력하고 연결 버튼을 클릭합니다.

<img src="/images/ncp_security_ssl_vpn_classic_guide_12.png" alt="Ncloud(네이버 클라우드) Classic 환경에서 SSL VPN 설정하고 접속하는 방법" style="width:557px;align:center">

위에서 설정했던 VPN 접속용 아이디와 Password를 입력하고 [로그온] 버튼을 클릭합니다. 

<img src="/images/ncp_security_ssl_vpn_classic_guide_13.png" alt="Ncloud(네이버 클라우드) Classic 환경에서 SSL VPN 설정하고 접속하는 방법" style="width:650px;align:center">

OTP 접속도 설정했다면 도착한 인증 번호를 입력합니다. 

<img src="/images/ncp_security_ssl_vpn_classic_guide_14.png" alt="Ncloud(네이버 클라우드) Classic 환경에서 SSL VPN 설정하고 접속하는 방법" style="width:650px;align:center">

SSL VPN에 제대로 연결이 되었는지 확인하기 위해서 cmd 창을 띄워서 ipconfig 명령어를 입력하면 아래 화면처럼 SSL VPN 주소로 설정된 어탭터에  [**SSL VPN IP POOL**]에 해당하는 IP가 할당된 것을 확인할 수 있습니다.

<img src="/images/ncp_security_ssl_vpn_classic_guide_15.png" alt="Ncloud(네이버 클라우드) Classic 환경에서 SSL VPN 설정하고 접속하는 방법" style="width:770px;align:center">


## 서버 접속
이제 SSL VPN이 연결된 상태에서 서버에 접속해보겠습니다.  
PuTTY 를 실행하고 접속할 서버의 IP를 입력합니다.  
이때 서버 IP는 콘솔에 있는 서버 정보에서 **비공인 IP**에 표시되는 IP를 입력하면 됩니다.

{: .error }
이때 혹시나 포트 포워딩이나 공인 IP를 설정하고 그 IP를 입력하는 일이 없도록 주의해야 합니다.

<img src="/images/ncp_security_ssl_vpn_classic_guide_16.png" alt="Ncloud(네이버 클라우드) Classic 환경에서 SSL VPN 설정하고 접속하는 방법" style="width:770px;align:center">


## Agent 기타 설정
SSL VPN Agent에는 몇가지 추가 기능이 있는데 아래에서 확인해보겠습니다.

#### 트래픽 그래프
Agent 창에서 [그래프 보기] 버튼을 클릭합니다. 

<img src="/images/ncp_security_ssl_vpn_classic_guide_17.png" alt="Ncloud(네이버 클라우드) Classic 환경에서 SSL VPN 설정하고 접속하는 방법" style="width:557px;align:center">

여기서는 SSL VPN이 연결된 상태의 트래픽을 그래프로 확인할 수 있습니다.

<img src="/images/ncp_security_ssl_vpn_classic_guide_18.png" alt="Ncloud(네이버 클라우드) Classic 환경에서 SSL VPN 설정하고 접속하는 방법" style="width:560px;align:center">

다음으로 [상세 보기] 버튼을 클릭하면 접속 세부 정보, 로그, 통계, 알림, 라우팅 테이블, IP설정 등의 정보를 확인할 수 있습니다.

<img src="/images/ncp_security_ssl_vpn_classic_guide_19.png" alt="Ncloud(네이버 클라우드) Classic 환경에서 SSL VPN 설정하고 접속하는 방법" style="width:770px;align:center">


## 스펙 변경
처음에 설정한 접속 가능 ID 개수를 변경하고 싶을 경우 SSL VPN을 선택하고 [스펙 변경] 버튼을 클릭합니다. 

<img src="/images/ncp_security_ssl_vpn_classic_guide_20.png" alt="Ncloud(네이버 클라우드) Classic 환경에서 SSL VPN 설정하고 접속하는 방법" style="width:770px;align:center">

3개, 5개, 10개 상품 중에서 원하는 개수로 변경할 수 있습니다.

<img src="/images/ncp_security_ssl_vpn_classic_guide_21.png" alt="Ncloud(네이버 클라우드) Classic 환경에서 SSL VPN 설정하고 접속하는 방법" style="width:770px;align:center">


## 주의사항
{: .error.box }
**SSL VPN이 연결된 상태에서 새로운 서버를 생성하고 접속을 시도하면 아래와 같은 오류 메시지가 뜨면서 서버 접속이 되지 않습니다.  이때는 SSL VPN의 연결을 끊었다가 다시 연결해야 새로 생성한 서버에 접속할 수 있습니다.**

<img src="/images/ncp_security_ssl_vpn_classic_guide_23.png" alt="Ncloud(네이버 클라우드) Classic 환경에서 SSL VPN 설정하고 접속하는 방법" style="width:700px;align:center">


## 삭제
삭제를 원할 경우 SSL VPN을 선택하고 상단의 [삭제] 버튼을 클릭합니다. 

<img src="/images/ncp_security_ssl_vpn_classic_guide_22.png" alt="Ncloud(네이버 클라우드) Classic 환경에서 SSL VPN 설정하고 접속하는 방법" style="width:770px;align:center">

## 참고 URL
1.  SSL VPN 사용 가이드(Classic)
	- <a href="https://guide.ncloud-docs.com/docs/security-security-5-1" target="_blank" style="word-break:break-all;">https://guide.ncloud-docs.com/docs/security-security-5-1</a>

2. SSL VPN 요금
	- <a href="https://www.ncloud.com/product/security/sslVpn" target="_blank" style="word-break:break-all;">https://www.ncloud.com/product/security/sslVpn</a>
