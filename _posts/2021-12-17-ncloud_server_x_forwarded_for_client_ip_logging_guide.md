---
date: 2021-12-17
last_modified_at: 2021-12-17
title: X-Forwarded-For를 이용해 Proxy, Load Balancer 환경에서 Client IP 기록하기
categories:
  - 1.compute
description: 네이버 클라우드 서비스에서 X-Forwarded-For를 이용해 Proxy, Load Balancer 환경에서 Client IP를 Apache access_log에 기록하는 방법입니다
type: Document
set: server
order_number: 25
creator: franky
---

## 개요
X-Forwarded-For (XFF) 는 HTTP Header 중 하나로 Load Balancer(로드밸런서)나 Proxy Server를 통해 웹서버에 접속하는 Client의 IP 주소를 식별하는 표준 헤더입니다.  
웹서버나 WAS 앞쪽에 Load Balancer 혹은 Proxy Server 등이 위치하게 된다면 서버 접근 로그에는 Client IP가 아닌 Load Balancer 혹은 Proxy Server의 IP 주소가 기록됩니다. 
이때 웹 어플리케이션에서 X-Forwarded-For 헤더를 이용하면 Client IP를 서버 접근 로그에 남길 수 있습니다.  

여기서는 Load Balancer와 연동된 CentOS 와 Ubuntu의 Apache 웹서버 환경에서 X-Forwarded-For 를 이용하여 Apache access_log에 Clinet의 IP를 저장하는 과정을 살펴보겠습니다.


## 테스트 환경
테스트는 CentOS, Ubuntu OS가 각각 설치된 2대의 서버를 Load Balancer와 연동한 후 Cloud Log Analytics에서 Apache access_log를  수집해 IP 주소를 확인하는 방식으로 진행하겠습니다.

#### Network 환경
- VPC 대역 : 172.16.0.0/16
- Subnet 대역 (Server) : 172.16.**10**.0/24
- Sbunet 대역 (Load Balancer) : 172.16.**20**.0/24

#### Server 환경
- xxf-test-1 : CentOS 7.8
- xxf-test-2 : Ubuntu 18.04


#### 테스트 서버
위 서버 환경에서 정리한 대로 CentOS, Ubuntu 2대의 서버를 준비했습니다.

- VPC 환경에서 서버 생성하는 방법 : <a href="/1.compute/ncp_server_vpc_create/" target="_blank" style="word-break:break-all;">https://docs.3rdeyesys.com/1.compute/ncp_server_vpc_create/</a>

<img src="../../images/ncloud_server_x_forwarded_for_client_ip_logging_guide_01.png" alt="네이버 클라우드 서비스에서 X-Forwarded-For를 이용해 Proxy, Load Balancer 환경에서 Client IP를 Apache access_log에 기록하는 방법" style="width:770px;align:center">

로드밸런서도 마찬가지로 준비하고, 서버와 연동까지 완료했습니다.

- VPC 환경에서 Application Load Balancer 생성하는 방법 : <a href="/2.networking/ncp_networking_load_balancer_application_lb/" target="_blank" style="word-break:break-all;">https://docs.3rdeyesys.com/2.networking/ncp_networking_load_balancer_application_lb/</a>

<img src="../../images/ncloud_server_x_forwarded_for_client_ip_logging_guide_02.png" alt="네이버 클라우드 서비스에서 X-Forwarded-For를 이용해 Proxy, Load Balancer 환경에서 Client IP를 Apache access_log에 기록하는 방법" style="width:770px;align:center">

## 설정 전 테스트
우선, **X-Forwarded-For (XFF)** 설정을 하기 전에 어떻게 기록이 남는지 확인해보겠습니다.  
아래와 같이 Load Balancer 주소로 접속해서 2대의 서버에 각각 접근하도록 합니다.  
위에서 소개한 Application Load Balancer 생성하는 방법을 그대로 따라하면 아래와 같은 메시지를 확인할 수 있습니다.

<img src="../../images/ncloud_server_x_forwarded_for_client_ip_logging_guide_11-1.png" alt="네이버 클라우드 서비스에서 X-Forwarded-For를 이용해 Proxy, Load Balancer 환경에서 Client IP를 Apache access_log에 기록하는 방법" style="width:560px;align:center">

<img src="../../images/ncloud_server_x_forwarded_for_client_ip_logging_guide_11-2.png" alt="네이버 클라우드 서비스에서 X-Forwarded-For를 이용해 Proxy, Load Balancer 환경에서 Client IP를 Apache access_log에 기록하는 방법" style="width:560px;align:center">

#### Apache 접속 로그 확인
Apache 접속 로그 파일은 아래의 위치에 존재하지만, 저희는 네이버 클라우드 (Ncloud)의 상품 중 하나인 **Cloud Log Analytics**에서 로그를 수집해서 확인해보겠습니다. 
- CentOS : /var/log/httpd/access_log
- Ubuntu : /var/log/apache2/access.log

- Cloud Log Analytics 설정 가이드 : <a href="/7.analytics/ncloud_analytics_cloud_log_analytics_guide/" target="_blank" style="word-break:break-all;">https://docs.3rdeyesys.com/7.analytics/ncloud_analytics_cloud_log_analytics_guide/</a>

Cloud Log Analytics에서 수집한 로그를 확인해보면 위에서 설정했던 **Load Balancer의 IP 대역 (172.16.20.xx)**이 기록된 것을 확인할 수 있습니다.

<img src="../../images/ncloud_server_x_forwarded_for_client_ip_logging_guide_03.png" alt="네이버 클라우드 서비스에서 X-Forwarded-For를 이용해 Proxy, Load Balancer 환경에서 Client IP를 Apache access_log에 기록하는 방법" style="width:770px;align:center">

## CentOS 설정
이제 실제 Client IP가 기록 되도록 설정을 변경해보겠습니다.  
우선 CentOS에서는 **httpd.conf** 파일만 수정하면 됩니다.

``` bash
~# vi /etc/httpd/conf/httpd.conf

# vi Line Number 표시하기
:set number
```

httpd.conf 파일 196번째 라인의 LogFormat 항목에서 **%h** 를 **%{X-Forwarded-For}i** 로 수정합니다.

#### httpd.conf 수정 전
``` bash   
196   LogFormat "%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\"" combined
```
<img src="../../images/ncloud_server_x_forwarded_for_client_ip_logging_guide_04.png" alt="네이버 클라우드 서비스에서 X-Forwarded-For를 이용해 Proxy, Load Balancer 환경에서 Client IP를 Apache access_log에 기록하는 방법" style="width:770px;align:center">

#### httpd.conf 수정 후  
```bash
196   LogFormat "%{X-Forwarded-For}i %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\"" combined
```
<img src="../../images/ncloud_server_x_forwarded_for_client_ip_logging_guide_05.png" alt="네이버 클라우드 서비스에서 X-Forwarded-For를 이용해 Proxy, Load Balancer 환경에서 Client IP를 Apache access_log에 기록하는 방법" style="width:770px;align:center">

#### Apache 재시작
httpd.conf 파일 수정 후에 Apache를 재시작합니다. 로그 테스트는 Ubuntu까지 설정을 마친 후에 진행하겠습니다.

``` bash
~# systemctl restart httpd
```

## Ubuntu 설정
Ubuntu에서는 apache2.conf 파일을 수정하기 전에 **remoteip 모듈**을 사용하도록 설정해줘야 합니다.

#### remoteip 설정
아래 명령어를 실행하면 remoteip 모듈이 활성화 됩니다.

``` bash
~# a2enmod remoteip
```
<img src="../../images/ncloud_server_x_forwarded_for_client_ip_logging_guide_06.png" alt="네이버 클라우드 서비스에서 X-Forwarded-For를 이용해 Proxy, Load Balancer 환경에서 Client IP를 Apache access_log에 기록하는 방법" style="width:770px;align:center">

#### remoteip.load 수정
다음으로 remoteip.load 파일을 수정해서 아래쪽에 [**RemoteIPHeader X-FORWARDED-FOR**]을 추가 합니다.

``` bash
~# vi /etc/apache2/mods-enabled/remoteip.load
```

#### remoteip.load 수정 전
``` bash
LoadModule remoteip_module /usr/lib/apache2/modules/mod_remoteip.so
```
#### remoteip.load 수정 후
``` bash
LoadModule remoteip_module /usr/lib/apache2/modules/mod_remoteip.so
RemoteIPHeader X-FORWARDED-FOR
```
<img src="../../images/ncloud_server_x_forwarded_for_client_ip_logging_guide_07.png" alt="네이버 클라우드 서비스에서 X-Forwarded-For를 이용해 Proxy, Load Balancer 환경에서 Client IP를 Apache access_log에 기록하는 방법" style="width:770px;align:center">

#### apache2.conf 수정
다음으로 **apache2.conf** 파일을 수정합니다.

``` bash
~# vi /etc/apache2/apache2.conf

# vi Line Number 표시하기
:set number
```

apache2.conf 213번째 라인의 LogFormat 부분에서 **%h** 를 **%a** 로 변경합니다. 

#### apache2.conf 수정 전
``` bash
213 LogFormat "%h %l %u %t \"%r\" %>s %O \"%{Referer}i\" \"%{User-Agent}i\"" combined  
```
<img src="../../images/ncloud_server_x_forwarded_for_client_ip_logging_guide_08.png" alt="네이버 클라우드 서비스에서 X-Forwarded-For를 이용해 Proxy, Load Balancer 환경에서 Client IP를 Apache access_log에 기록하는 방법" style="width:770px;align:center">

#### apache2.conf 수정 후
``` bash
213 LogFormat "%a %l %u %t \"%r\" %>s %O \"%{Referer}i\" \"%{User-Agent}i\"" combined  
```
<img src="../../images/ncloud_server_x_forwarded_for_client_ip_logging_guide_09.png" alt="네이버 클라우드 서비스에서 X-Forwarded-For를 이용해 Proxy, Load Balancer 환경에서 Client IP를 Apache access_log에 기록하는 방법" style="width:770px;align:center">

#### Apache 재시작
``` bash
~# systemctl restart apache2
```

## 설정 후 테스트
위와 같이  CentOS, Ubuntu 2대 서버에서 설정을 모두 마친 후에 로드 밸런서 URL로 접속합니다.  
이후에 Cloud Log Analytics에서 로그를 확인해보면 아래와 같이 로드밸런서 IP가 아닌 실제 접속한 Client의 IP가 기록된 것을 확인할 수 있습니다.

<img src="../../images/ncloud_server_x_forwarded_for_client_ip_logging_guide_10.png" alt="네이버 클라우드 서비스에서 X-Forwarded-For를 이용해 Proxy, Load Balancer 환경에서 Client IP를 Apache access_log에 기록하는 방법" style="width:770px;align:center">


## 참고 URL
1.  X-Forwarded-For 안내
	- <a href="https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/X-Forwarded-For" target="_blank" style="word-break:break-all;">https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/X-Forwarded-For</a>
