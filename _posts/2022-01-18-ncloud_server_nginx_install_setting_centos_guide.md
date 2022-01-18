---
date: 2022-01-18
last_modified_at: 2022-01-18
title: CentOS에서 NginX 설치, 설정하는 방법
categories:
  - 1.compute
description: 네이버 클라우드 CentOS에서 NginX 설치, 설정하는 방법입니다
type: Document
set: server
order_number: 27
---

## 개요
네이버 클라우드 CentOS 서버에 NginX를 Package로 설치하고 기본 설정을 하는 방법에 대한 내용을 정리해보겠습니다.  
사용한 서버는 CentOS 7.8 입니다.

## yum 유틸리티 설치
yum으로 NginX를 설치하기 전에 yum-utils를 먼저 설치합니다. 이미 설치 되어 있을 경우에는 아래와 같이 설치되어 있다는 메시지가 출력됩니다.

``` bash
~# yum install yum-utils
```
<img src="../../images/ncloud_server_nginx_install_setting_guide_centos_01.png" alt="네이버 클라우드 CentOS에서 NginX 설치, 설정하는 방법" style="width:770px;align:center">


## Repository 설정
NginX package를 다운 받아 설치하기 위해서는 Repository를 설정해야 합니다. 
Repository 디렉토리에 nginx.repo 파일을 만들고 아래와 같은 내용을 입력합니다.

``` bash
~# vi /etc/yum.repos.d/nginx.repo
```

``` bash
[nginx-stable]
name=nginx stable repo
baseurl=http://nginx.org/packages/centos/$releasever/$basearch/
gpgcheck=1
enabled=1
gpgkey=https://nginx.org/keys/nginx_signing.key
module_hotfixes=true

[nginx-mainline]
name=nginx mainline repo
baseurl=http://nginx.org/packages/mainline/centos/$releasever/$basearch/
gpgcheck=1
enabled=0
gpgkey=https://nginx.org/keys/nginx_signing.key
module_hotfixes=true
```

{: .info }
NginX는 stable, mainline 두가지 버전이 있습니다. NginX의 공식 설명에 따르면 버그 수정이나 보안 패치 등은 항상 mainline 버전에 먼저 적용되기 때문에 mainline을 사용하는 것을 추천한다고 합니다. 
stable 버전을 사용하는 주된 경우는 third-party 모듈을 사용하고 있어서 신규 버전에서 호환성 문제가 발생할 가능성이 걱정될 때라고 합니다.

<img src="../../images/ncloud_server_nginx_install_setting_guide_centos_02.png" alt="네이버 클라우드 CentOS에서 NginX 설치, 설정하는 방법" style="width:770px;align:center">


## 버전 선택
stable, mainline 두가지 버전 중에서 기본은 stable 버전입니다.  
stable 버전을 설치할 경우에는 다음 명령어는 건너띄어도 되고, mainline 버전을 설치하기 위해서는 아래 명령어로 설정을 변경해주어야 합니다.

``` bash
~# yum-config-manager --enable nginx-mainline
```

<img src="../../images/ncloud_server_nginx_install_setting_guide_centos_03.png" alt="네이버 클라우드 CentOS에서 NginX 설치, 설정하는 방법" style="width:770px;align:center">

## NginX 설치
설정을 마쳤으면 yum으로 NginX를 설치합니다.
``` bash
~# yum -y install nginx
```

<img src="../../images/ncloud_server_nginx_install_setting_guide_centos_04.png" alt="네이버 클라우드 CentOS에서 NginX 설치, 설정하는 방법" style="width:770px;align:center">


## 디렉토리 설정
다음으로 홈으로 사용할 디렉토리를 생성하고, 해당 디렉토리의 소유권을 설정하겠습니다.  
그리고, NginX가 정상 작동하는지 확인해보기 위해 설치시에 포함된 index.html을 홈 디렉토리로 복사합니다.

``` bash
~# mkdir -p /ncp/data/www
~# chown -R nginx:nginx /ncp/data/www
~# cp /usr/share/nginx/html/index.html /ncp/data/www/index.html
~# ls -al /ncp/data/www
```
<img src="../../images/ncloud_server_nginx_install_setting_guide_centos_05.png" alt="네이버 클라우드 CentOS에서 NginX 설치, 설정하는 방법" style="width:770px;align:center">


## 환경 설정
주로 변경할 환경 설정 파일은 /etc/nginx/conf.d/default.conf 입니다.

``` bash
~# vi /etc/nginx/conf.d/default.conf
```
<br />
#### Port와 Server Name 설정
80이 아닌 다른 Port를 사용할 경우나 도메인을 설정하게 될 경우 2, 3 라인에 있는 아래 항목들을 수정하면 됩니다.

``` bash
server_name  localhost;
server_name  127.0.0.1;
```

<img src="../../images/ncloud_server_nginx_install_setting_guide_centos_06.png" alt="네이버 클라우드 CentOS에서 NginX 설치, 설정하는 방법" style="width:770px;align:center">

#### 홈 디렉토리, 기본 문서 설정
앞에서 만들었던 홈 디렉토리 경로를 설정하고 기본 문서를 지정하는 곳입니다. 

``` bash
# 변경 전
    location / {
        root   /usr/share/nginx/html;
        index  index.html index.htm;
    }

# 변경 후
    root   /ncp/data/www;
    index  index.html index.htm;

    location / {
        try_files $uri $uri/ = 404;
    }
```
<img src="../../images/ncloud_server_nginx_install_setting_guide_centos_07.png" alt="네이버 클라우드 CentOS에서 NginX 설치, 설정하는 방법" style="width:770px;align:center">

<img src="../../images/ncloud_server_nginx_install_setting_guide_centos_08.png" alt="네이버 클라우드 CentOS에서 NginX 설치, 설정하는 방법" style="width:770px;align:center">


#### error 페이지 설정
404, 500 등의 error 페이지를 설정할 경우 아래 내용들을 수정하면 됩니다.

``` bash
# 변경 전
    #error_page  404              /404.html;

    location = /50x.html {
        root   /usr/share/nginx/html;
    }

# 변경 후
    error_page  404              /404.html;

    location = /50x.html {
        root   /ncp/data/www;
    }
```

<img src="../../images/ncloud_server_nginx_install_setting_guide_centos_09.png" alt="네이버 클라우드 CentOS에서 NginX 설치, 설정하는 방법" style="width:770px;align:center">

<img src="../../images/ncloud_server_nginx_install_setting_guide_centos_10.png" alt="네이버 클라우드 CentOS에서 NginX 설치, 설정하는 방법" style="width:770px;align:center">


#### .htaccess 파일 접근 금지 설정
.htaccess 파일에 대한 접근 금지를 설정할 경우 아래 내용을 주석 해제하면 됩니다.

``` bash
# 변경 전
    #location ~ /\.ht {
    #    deny  all;
    #}

# 변경 후
    location ~ /\.ht {
        deny  all;
    }
```

<img src="../../images/ncloud_server_nginx_install_setting_guide_centos_11.png" alt="네이버 클라우드 CentOS에서 NginX 설치, 설정하는 방법" style="width:770px;align:center">

<img src="../../images/ncloud_server_nginx_install_setting_guide_centos_12.png" alt="네이버 클라우드 CentOS에서 NginX 설치, 설정하는 방법" style="width:770px;align:center">


## NginX 실행
설정을 모두 마쳤으면 NginX를 시작하고 상태를 확인합니다.

``` bash
~# systemctl start nginx
~# systemctl status nginx
```
<img src="../../images/ncloud_server_nginx_install_setting_guide_centos_13.png" alt="네이버 클라우드 CentOS에서 NginX 설치, 설정하는 방법" style="width:770px;align:center">


#### 사이트 접속
NginX가 정상 작동하면 아래와 같이 서버 접속 화면을 확인할 수 있습니다.

<img src="../../images/ncloud_server_nginx_install_setting_guide_centos_14.png" alt="네이버 클라우드 CentOS에서 NginX 설치, 설정하는 방법" style="width:600px;align:center">


## 참고 URL
1.  NginX Linux packages 설치 가이드
	- <a href="http://nginx.org/en/linux_packages.html" target="_blank" style="word-break:break-all;">http://nginx.org/en/linux_packages.html</a>
