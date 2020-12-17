# [CentOS] http 접속 시에 https로 강제 리다이렉트 시키는 방법 - Apache

## 개요
웹사이트 SSL 인증서를 설치하고 https 접속을 유도할 때 http로 접속하면 https로 강제로 리다이렉트 시키는 방법을 사용하는 경우가 많습니다.  
웹페이지 소스에서 http 접속 여부를 판단해서 redirect 시키는 방법 등 여러가지 있을 수 있는데 여기서는 Apache 설정으로 쉽게 할 수 있는 방법을 소개합니다.  

SSL 인증서가 설치되어 있다는 가정하에 우선 Linux CentOS에서 설정하는 방법을 확인해보겠습니다.

## Apache conf 파일 수정

**/etc/httpd/conf/httpd.conf** 의 Vritual host에 다음 코드를 추가하고 Apache를 재시작하면 됩니다.
``` bash
RewriteEngine On
RewriteCond %{HTTPS} !on
RewriteRule ^(.*)$ https://%{HTTP_HOST}$1 [R,L]
```

httpd.conf 파일에 실제로 적용하면 다음과 비슷한 모습이 되겠습니다.
``` bash
<VirtualHost *:80>
	DocumentRoot "/ncp/data/www/"
	ServerName www.test.com

	RewriteEngine On
	RewriteCond %{HTTPS} !on
	RewriteRule ^(.*)$ https://%{HTTP_HOST}$1 [R,L]
</VirtualHost>
```

## Apache 재시작
``` bash
root@test-lamp-2:~# systemctl restart  httpd.service
```
이렇게 재시작하고 http로 접속을 해보면 https로 전환되는 것을 확인할 수 있습니다.


## Apache 재시작 오류
위 내용처럼 아파치 재시작 명령어를 입력하면 끝입니다만, 재시작이 되지 않고 오류 메시지가 뜨는 경우가 있습니다.
``` bash
root@test-lamp-2:~# systemctl restart  httpd.service
Job for httpd.service failed because the control process exited with error code.  
See "systemctl status httpd.service" and "journalctl -xe" for details.
```

오류 메시지에 나온 것 처럼 상세 내용을 확인해봅니다.
``` bash
root@test-lamp-2:~# systemctl status httpd.service
```
상세 오류 메시지 중에서 핵심 내용만 살펴보면 다음과 같습니다.
``` bash
Dec 04 17:24:48 test-lamp-2 httpd[3217]: httpd: Syntax error on line 552 of /etc/httpd/conf/httpd.conf: Could not open configuration ...irectory
```

위 오류 메시지에 나온 /etc/httpd/conf/httpd.conf 파일을 찾아가서 해당 라인을 확인해보면 다음과 같습니다.
``` bash
#<IfModule ssl_module>
# Secure (SSL/TLS) connections
Include conf/extra/httpd-ssl.conf
#</IfModule>
```

위에 나온 Include conf/extra/httpd-ssl.conf 라인을 주석처리 하면 해결이 됩니다.

``` bash
#<IfModule ssl_module>
# Secure (SSL/TLS) connections
#Include conf/extra/httpd-ssl.conf
#</IfModule>
```

그리고 나서 다시 Apache를 재시작 하면 됩니다.
``` bash
root@test-lamp-2:~# systemctl restart  httpd.service
```

이제 http로 접속하셔서 https로 전환되는지 확인해보시기 바랍니다.



## SSL 모듈 설치
혹시 Apache에 mod_ssl 가 설치되어 있지 않다면 설치하셔야 합니다.

``` bash
[root@test-lamp-2 ~]# yum install mod_ssl
```

설치 도중에 아래와 같은 확인 화면이 나타납니다.
``` bash
========================================================================
 Package   Arch       Version                 Repository   Size
========================================================================
Installing:
 mod_ssl    x86_64   1:2.4.6-97.el7.centos   updates    114 k

Transaction Summary
========================================================================
Install  1 Package

Total download size: 114 k
Installed size: 224 k
Is this ok [y/d/N]:
```

별 문제 없으면 y 를 입력하면 됩니다.
``` bash
Is this ok [y/d/N]: y

Downloading packages:
mod_ssl-2.4.6-97.el7.centos.x86_64.rpm                    | 114 kB  00:00:00
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : 1:mod_ssl-2.4.6-97.el7.centos.x86_64        1/1
  Verifying  : 1:mod_ssl-2.4.6-97.el7.centos.x86_64        1/1

Installed:
  mod_ssl.x86_64 1:2.4.6-97.el7.centos

Complete!
```


!!! info "문서 최종 수정일 : 2020-12-07"