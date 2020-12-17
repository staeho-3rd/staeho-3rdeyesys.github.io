# Micro 타입 서버에서 사용할 수 없는 서비스

## 개요
네이버 클라우드에서 제공하는 서버 타입 중에서 Micro 타입의 서버는 신규 가입 후 최초 결제수단 등록월부터 1년간 무료로 제공되는 체험용 서버입니다.
계정당 1대만 이용 가능하며 1년이 지나면 유료로 전환됩니다.

## 제한되는 서비스
Micro 타입의 서버에서 사용할 수 없는 서비스는 다음과 같습니다.

- mssql
- LAMP, WordPress, LEMP 등
- Network Interface
- Private Subnet


### MSSQL 
mssql 중에서 mssql 2017 Express은 무료로 제공되는 서비스이지만, mssql이 1년 무료제공 서버인 Micro서버에서는 설치가 되지 않기 때문에, compact 이상의 유료 서버를 이용해야 합니다.
즉, mssql 2017 Express는 무료이나 서버와 그에 따른 하드 디스크 비용은 유료입니다.

### LAMP
LAMP (Linux + Apache, Mysql, PHP)의 경우 Micro 타입의 서버에 설치를 할 수 없고, Standard 이상의 서버에서만 설치할 수 있는데, 대신 네이버 클라우드에서는 LAMP 등 많이 사용되는 오픈 소스 소프트웨어가 설치된 서버를 쉽게 이용할 수 있도록 해주는 서비스인 Application Server Launcher를 제공하고 있습니다.

Application Server Launcher에서는 원하는 소프트웨어의 이미지를 선택하기만 하면 쉽게 Micro 타입의 서버를 세팅하고 이용할 수 있습니다.

!!! warning 
	다만, Application Server Launcher에서 생성한 서버도 Micro 타입의 서버이기 때문에 계정당 1개만 제공되는 Micro 타입 서버 기준에 따라 Micro 타입의 서버는 더 이상 추가할 수 없습니다.


Application Server Launcher에서 OS버전(CentOS, Ubuntu)별로 제공되는 애플리케이션은 다음과 같은데 {==**현재(2020년 11월 12일 기준)는 Ubuntu만 제공**==} 되고 있습니다.

- Drupal (CMS)
- Joomla! (CMS)
- Magento (E-Commerce)
- Shadowsocks (VPN)
- LAMP (Web Stack)
- WordPress (CMS)
- Jenkins (Dev Tools)


### Private Subnet
Private Subnet을 구성해서 서버환경을 만들려고 해도 Micro 서버 타입은 Network Interface를 추가할 수 없고, 그에 따라 Private Subnet도 적용할 수 없습니다.

## 참고 URL
<a href="https://docs.ncloud.com/ko/compute/compute-1-1-v2.html" target="_blank">https://docs.ncloud.com/ko/compute/compute-1-1-v2.html</a>
<a href="https://docs.ncloud.com/ko/asl/asl_console.html" target="_blank">https://docs.ncloud.com/ko/asl/asl_console.html</a>


!!! info "문서 최종 수정일 : 2020-11-13"
