---
date: 2021-11-02
last_modified_at: 2021-11-02
title: VPC 환경에서 SSL VPN 설정하고 접속하는 방법
categories:
  - 3.security
description: Ncloud(네이버 클라우드) VPC 환경에서 SSL VPN 설정하고 접속하는 방법입니다.
type: Document
set: security
order_number: 5
creator: ljh0519
v2_link: /security/ncloud_security_ssl_vpn_vpc_guide.html
---

## 개요
Ncloud(네이버 클라우드) 외부에서 내부에 구성된 네트워크로 암호화된 보안 접속 통신을 제공하는 서비스인 SSL VPN을 VPC 환경에서 설정하고, 서버에 접속하는 방법에 대해 정리해보겠습니다.

## SSL VPN이란?
- VPN은 가상 사설망(Virtual Private Network)의 약자로, 외부에서 접근할 수 없는 사설망에 내 PC나 네트워크를 연결시키는 방법을 말합니다.
- 사설망과의 연결은 가상 터널을 통해 이루어지며, 이 가상 터널을 SSL 암호화로 보호하는 것이 SSL VPN입니다.
- 가상 터널을 통해 사설망과 연결된 사용자 PC는 사설망의 라우팅 및 ACL 정책에 따라 내부 서버에 접근할 수 있습니다.


## SSL VPN 생성 요청
[SSL VPN 생성] 버튼을 클릭합니다.

<img src="../../images/ncp_security_ssl_vpn_vpc_guide_01.png" alt="Ncloud(네이버 클라우드) VPC 환경에서 SSL VPN 설정하고 접속하는 방법" style="width:770px;align:center">

우선 접근할 서버가 속해 있는 VPC를 선택하고, 등록 가능한 접속 ID 개수에 따른 상품을 선택합니다.  
VPC 환경에서는 3, 5, 10, 20, 30, 50, 100, 200, 300, 400, 500개 상품 중에서 선택 가능합니다. 

<img src="../../images/ncp_security_ssl_vpn_vpc_guide_07.png" alt="Ncloud(네이버 클라우드) VPC 환경에서 SSL VPN 설정하고 접속하는 방법" style="width:700px;align:center">

VPC 환경의 SSL VPN은 콘솔에서 자동으로 생성하는 것이 아니라 **생성 요청을 하면 Ncloud 담당자가 직접 생성하고 결과를 안내 받는 구조**로 되어 있습니다. 
생성 완료까지 걸리는 기간은 **업무일 기준 2~3일** 정도이며, 생성이 완료되면 SSL VPN 상태가 [운영중]으로 변경되고, 안내 메일이 도착합니다.

<img src="../../images/ncp_security_ssl_vpn_vpc_guide_03.png" alt="Ncloud(네이버 클라우드) VPC 환경에서 SSL VPN 설정하고 접속하는 방법" style="width:500px;align:center">

아래와 같이 생성 요청을 하고 나면 상태가 [생성중] 상태로 표시 됩니다. 2~3일 후 생성 완료 안내 메일이 도착할때까지 기다리시면 됩니다.

<img src="../../images/ncp_security_ssl_vpn_vpc_guide_04.png" alt="Ncloud(네이버 클라우드) VPC 환경에서 SSL VPN 설정하고 접속하는 방법" style="width:770px;align:center">

## SSL VPN 생성 완료
SSL VPN 생성이 완료되면 아래와 같은 메일을 받을 수 있습니다.   
메일 내용에는 [**SSL VPN IP POOL**] 정보와 접속 경로 정보가 포함되어 있으니 잘 확인해야 합니다.

{: .error.box }
특히 [**SSL VPN IP POOL**] 정보는 Ncloud 콘솔에서 확인할 수 없고, 오로지 생성 완료 메일에서만 확인 가능하니 잘 보관해야 합니다.

<img src="../../images/ncp_security_ssl_vpn_vpc_guide_05.png" alt="Ncloud(네이버 클라우드) VPC 환경에서 SSL VPN 설정하고 접속하는 방법" style="width:700px;align:center">

생성 완료 메일 확인 후에 콘솔에 접속하면 아래와 같이 [운영중] 으로 상태 메시지가 바뀐 것을 확인할 수 있습니다.

<img src="../../images/ncp_security_ssl_vpn_vpc_guide_06.png" alt="Ncloud(네이버 클라우드) VPC 환경에서 SSL VPN 설정하고 접속하는 방법" style="width:770px;align:center">


## 사용자 설정
SSL VPN에 접속할 사용자 정보를 설정합니다.  리스트에서 SSL VPN을 선택하고 [사용자 설정] 버튼을 클릭합니다.

<img src="../../images/ncp_security_ssl_vpn_vpc_guide_08.png" alt="Ncloud(네이버 클라우드) VPC 환경에서 SSL VPN 설정하고 접속하는 방법" style="width:770px;align:center">

사용자 정보는 ID, Passowrd, Email, SMS 등을 입력하고 [추가] 버튼을 클릭합니다.  
VPC 환경은 Classic 환경과 달리 이차인증이 필수이므로  SMS와 Email 정보를 함께 입력합니다.

<img src="../../images/ncp_security_ssl_vpn_vpc_guide_09.png" alt="Ncloud(네이버 클라우드) VPC 환경에서 SSL VPN 설정하고 접속하는 방법" style="width:770px;align:center">

사용자가 추가되면 SSL VPN 정보에 사용자 계정 수(등록된 ID 수)에 숫자가 표시되는 것을 확인할 수 있습니다.

<img src="../../images/ncp_security_ssl_vpn_vpc_guide_10.png" alt="Ncloud(네이버 클라우드) VPC 환경에서 SSL VPN 설정하고 접속하는 방법" style="width:770px;align:center">

## Route Table 설정
네트워크 설정에서는 우선 접속할 서버가 속해 있는 VPC Subnet의 Route Table에 SSL VPN으로 접근할 수 있게 [**SSL VPN IP POOL**]을 등록해야 합니다.  
아래와 같이 접속할 서버의 상세정보에서 VPC와 Subnet을 확인합니다. 여기서는 Private Subnet에 생성한 서버에 접속할 예정입니다. 

<img src="../../images/ncp_security_ssl_vpn_vpc_guide_11.png" alt="Ncloud(네이버 클라우드) VPC 환경에서 SSL VPN 설정하고 접속하는 방법" style="width:770px;align:center">

[VPC] - [Route Table]에서 해당 VPC의 Private Subnet을 선택하고 [Routes 설정] 버튼을 클릭합니다.  
Public Subnet에 생성한 서버에 접근해야 할 경우에는 public-table을 선택하고 설정하시면 됩니다.

<img src="../../images/ncp_security_ssl_vpn_vpc_guide_12.png" alt="Ncloud(네이버 클라우드) VPC 환경에서 SSL VPN 설정하고 접속하는 방법" style="width:770px;align:center">

[Destination]에 SSL VPN 생성 완료 메일에서 확인한 [**SSL VPN IP POOL**] 정보를 등록하고, [Target Type]은 SSLVPN을 선택, [Target Name]은 위에서 생성한 SSL VPN 이름을 선택하고 [생성] 버튼을 클릭합니다.

<img src="../../images/ncp_security_ssl_vpn_vpc_guide_13.png" alt="Ncloud(네이버 클라우드) VPC 환경에서 SSL VPN 설정하고 접속하는 방법" style="width:770px;align:center">

## ACG 설정
다음으로 서버에 적용된 ACG에 [**SSL VPN IP POOL**]을 등록해야 합니다.  
우선 SSL VPN을 통해서 접속할 서버를 선택하고 [ACG 수정] 버튼을 클릭해서 적용된 ACG를 확인합니다.

<img src="../../images/ncp_security_ssl_vpn_vpc_guide_14.png" alt="Ncloud(네이버 클라우드) VPC 환경에서 SSL VPN 설정하고 접속하는 방법" style="width:770px;align:center">

ACG 수정 화면에서 현재 적용된 ACG 이름을 확인할 수 있습니다.

<img src="../../images/ncp_security_ssl_vpn_vpc_guide_15.png" alt="Ncloud(네이버 클라우드) VPC 환경에서 SSL VPN 설정하고 접속하는 방법" style="width:770px;align:center">

[Server] - [ACG]에서 위에서 확인한 ACG를 선택하고 [ACG 설정]을 클릭합니다. 

<img src="../../images/ncp_security_ssl_vpn_vpc_guide_16.png" alt="Ncloud(네이버 클라우드) VPC 환경에서 SSL VPN 설정하고 접속하는 방법" style="width:770px;align:center">

ACG 규칙 설정 화면에서 위에서 확인했던 [**SSL VPN IP POOL**]을 접근 소스에 입력하고, 필요한 포트들을 등록합니다. 
기본이 되는 프로토콜과 포트는 리눅스 서버 접속용 TCP 22, 윈도우 서버 접속용 TCP 3389, Ping 확인용 ICMP 등입니다.

<img src="../../images/ncp_security_ssl_vpn_vpc_guide_17.png" alt="Ncloud(네이버 클라우드) VPC 환경에서 SSL VPN 설정하고 접속하는 방법" style="width:770px;align:center">


## Agent 다운로드
SSL VPN 접속을 위한 Agent를 다운로드 합니다.  

- <a href="https://guide.ncloud-docs.com/docs/security-5-1-windowsdownload" target="_blank" style="word-break:break-all;">윈도우 용 다운로드</a>
- <a href="https://guide.ncloud-docs.com/docs/security-5-1-macdownload" target="_blank" style="word-break:break-all;">맥 용 다운로드</a>

<img src="../../images/ncp_security_ssl_vpn_classic_guide_11.png" alt="Ncloud(네이버 클라우드) VPC 환경에서 SSL VPN 설정하고 접속하는 방법" style="width:770px;align:center">

## Agent 접속
Agent 프로그램, BIG-IP Edge Client를 설치하고 실행하면 다음과 같은 화면을 볼 수 있습니다.  
Agent에는 Classic 환경 SSL VPN 서버(Ncloud-kr-01)가 기본으로 설정되어 있는데, VPC 환경 SSL VPN 서버 주소로 바꾸기 위해 [연결끊기] 클릭해서 연결을 끊고, [서버 변경] 버튼을 클릭합니다.

<img src="../../images/ncp_security_ssl_vpn_vpc_guide_18.png" alt="Ncloud(네이버 클라우드) VPC 환경에서 SSL VPN 설정하고 접속하는 방법" style="width:557px;align:center">

서버 선택 창에 **https://sslvpn-kr-vpc-01.ncloud.com** 주소를 입력합니다.

<img src="../../images/ncp_security_ssl_vpn_vpc_guide_19.png" alt="Ncloud(네이버 클라우드) VPC 환경에서 SSL VPN 설정하고 접속하는 방법" style="width:557px;align:center">

주소 변경 후에 [연결] 버튼을 클릭하면 로그인 화면이 나타나는데 위에서 설정했던 VPN 접속용 아이디와 Password를 입력하고 [로그온] 버튼을 클릭합니다. 

<img src="../../images/ncp_security_ssl_vpn_vpc_guide_20.png" alt="Ncloud(네이버 클라우드) VPC 환경에서 SSL VPN 설정하고 접속하는 방법" style="width:650px;align:center">

그리고 도착한 OTP 인증 번호를 입력합니다. 

<img src="../../images/ncp_security_ssl_vpn_vpc_guide_21.png" alt="Ncloud(네이버 클라우드) VPC 환경에서 SSL VPN 설정하고 접속하는 방법" style="width:650px;align:center">

SSL VPN에 제대로 연결이 되었는지 확인하기 위해서 cmd 창을 띄워서 ipconfig 명령어를 입력하면 아래 화면처럼 SSL VPN 주소로 설정된 어탭터에  [**SSL VPN IP POOL**]에 해당하는 IP가 할당된 것을 확인할 수 있습니다.

<img src="../../images/ncp_security_ssl_vpn_vpc_guide_22.png" alt="Ncloud(네이버 클라우드) VPC 환경에서 SSL VPN 설정하고 접속하는 방법" style="width:770px;align:center">


## 서버 접속
이제 SSL VPN이 연결된 상태에서 서버에 접속해보겠습니다.  
PuTTY 를 실행하고 접속할 서버의 IP를 입력합니다.  
이때 서버 IP는 콘솔에 있는 서버 정보에서 **비공인 IP**에 표시되는 IP를 입력하면 됩니다.

<img src="../../images/ncp_security_ssl_vpn_vpc_guide_23.png" alt="Ncloud(네이버 클라우드) VPC 환경에서 SSL VPN 설정하고 접속하는 방법" style="width:770px;align:center">


## Agent 기타 설정
SSL VPN Agent에는 몇가지 추가 기능이 있는데 아래에서 확인해보겠습니다.

#### 트래픽 그래프
Agent 창에서 [그래프 보기] 버튼을 클릭합니다. 

<img src="../../images/ncp_security_ssl_vpn_vpc_guide_24.png" alt="Ncloud(네이버 클라우드) VPC 환경에서 SSL VPN 설정하고 접속하는 방법" style="width:557px;align:center">

여기서는 SSL VPN이 연결된 상태의 트래픽을 그래프로 확인할 수 있습니다.

<img src="../../images/ncp_security_ssl_vpn_vpc_guide_25.png" alt="Ncloud(네이버 클라우드) VPC 환경에서 SSL VPN 설정하고 접속하는 방법" style="width:557px;align:center">

다음으로 [상세 보기] 버튼을 클릭하면 접속 세부 정보, 로그, 통계, 알림, 라우팅 테이블, IP설정 등의 정보를 확인할 수 있습니다.

<img src="../../images/ncp_security_ssl_vpn_vpc_guide_26.png" alt="Ncloud(네이버 클라우드) VPC 환경에서 SSL VPN 설정하고 접속하는 방법" style="width:770px;align:center">


## 스펙 변경
처음에 설정한 접속 가능 ID 개수를 변경하고 싶을 경우 SSL VPN을 선택하고 [스펙 변경] 버튼을 클릭합니다. 

<img src="../../images/ncp_security_ssl_vpn_vpc_guide_27.png" alt="Ncloud(네이버 클라우드) VPC 환경에서 SSL VPN 설정하고 접속하는 방법" style="width:770px;align:center">

3, 5, 10, 20, 30, 50, 100, 200, 300, 400, 500개 상품 중에서 원하는 개수로 변경할 수 있습니다.

<img src="../../images/ncp_security_ssl_vpn_vpc_guide_28.png" alt="Ncloud(네이버 클라우드) VPC 환경에서 SSL VPN 설정하고 접속하는 방법" style="width:770px;align:center">


## 주의사항
{: .error.box }
**SSL VPN이 연결된 상태에서 새로운 서버를 생성하고 접속을 시도하면 아래와 같은 오류 메시지가 뜨면서 서버 접속이 되지 않습니다.  이때는 SSL VPN의 연결을 끊었다가 다시 연결해야 새로 생성한 서버에 접속할 수 있습니다.**

<img src="../../images/ncp_security_ssl_vpn_classic_guide_23.png" alt="Ncloud(네이버 클라우드) Classic 환경에서 SSL VPN 설정하고 접속하는 방법" style="width:700px;align:center">


## 삭제
삭제를 원할 경우 SSL VPN을 선택하고 상단의 [삭제] 버튼을 클릭합니다. 

<img src="../../images/ncp_security_ssl_vpn_vpc_guide_29.png" alt="Ncloud(네이버 클라우드) VPC 환경에서 SSL VPN 설정하고 접속하는 방법" style="width:770px;align:center">

하지만, 바로 삭제가 되지 않고 다음과 같이 "Route Table에서 Target으로 지정된 Route 정보를 모두 삭제해야 삭제가 가능합니다"라는 메시지가 뜹니다. 

<img src="../../images/ncp_security_ssl_vpn_vpc_guide_30.png" alt="Ncloud(네이버 클라우드) VPC 환경에서 SSL VPN 설정하고 접속하는 방법" style="width:500px;align:center">

[VPC] - [Route Table]에서 해당 VPC의 Private Subnet을 선택하고 [Routes 설정] 버튼을 클릭해서 등록된 설정을 확인하고 [X] 버튼을 클릭해서 설정을 삭제합니다. 
Route Table 정보를 삭제한 후에 SSL VPN을 삭제하시면 됩니다.

<img src="../../images/ncp_security_ssl_vpn_vpc_guide_31.png" alt="Ncloud(네이버 클라우드) VPC 환경에서 SSL VPN 설정하고 접속하는 방법" style="width:770px;align:center">

## 참고 URL
1.  SSL VPN 사용 가이드(VPC)
	- <a href="https://guide.ncloud-docs.com/docs/security-security-5-2" target="_blank" style="word-break:break-all;">https://guide.ncloud-docs.com/docs/security-security-5-2</a>

2. SSL VPN 요금
	- <a href="https://www.ncloud.com/product/security/sslVpn" target="_blank" style="word-break:break-all;">https://www.ncloud.com/product/security/sslVpn</a>
