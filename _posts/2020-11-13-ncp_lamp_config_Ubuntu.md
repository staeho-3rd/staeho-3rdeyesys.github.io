---
date: 2020-11-13
last_modified_at: 2020-11-13
title: LAMP(Ubuntu) 기본 명령어와 환경 설정 파일 위치
categories:
  - 1.compute
description: 네이버 클라우드 LAMP(Ubuntu) 기본 명령어와 환경 설정 파일 위치
type: Document
set: lamp
order_number: 3
v2_link: /compute/ncloud_compute_lamp_config_ubuntu.html
---

## Apache 시작, 중지, 재시작
Apache 시작, 중지, 재시작 명령어는 OS별로 조금씩 다른데 Ubuntu의 경우에는 systemctl [stop|start|restart] {서비스명} 의 순서로 되어 있습니다.

### Ubuntu
``` bash
- 중지 : systemctl stop apache2
- 시작 : systemctl start apache2
- 재시작 : systemctl restart apache2
```


## mysql 시작, 중지, 재시작
mysql 시작, 중지, 재시작 명령어도 Apache와 마찬가지로 systemctl [stop|start|restart] {서비스명} 의 순서로 되어 있습니다.

## Ubuntu
``` bash
- 중지 : systemctl stop mysql
- 시작 : systemctl start mysql
- 재시작 : systemctl restart mysql
```
	

## Apache 환경 설정 파일 
Apache의 기본 환경설정 파일은 CentOS의 경우 httpd.conf라는 파일로 간단하게 구성되어 있는데 Ubuntu의 경우에는 포트, 가상호스트, 로그 등 각각의 항목별로 파일이 나뉘어져 있습니다.

### Ubuntu
``` bash
- 기본 설정 : /etc/apache2/apache2.conf
- 포트 설정 : /etc/apache2/ports.conf
- 가상호스트 설정: /etc/apache2/sites-enabled/000-default.conf -> /etc/apache2/sites-available/000-default.conf
- 로그 : /var/log/apache2
- 기타 옵션 설정 :
  /etc/apache2/mods-enabled/*.load -> /etc/apache2/mods-available/*.load
  /etc/apache2/mods-enabled/*.conf -> /etc/apache2/mods-available/*.conf
 ```


## PHP 환경 설정 파일
PHP의 환경 설정파일인 php.ini는  PHP버전에 해당 하는 디렉토리에 위치하고 있습니다.
``` bash
php.ini : /etc/php/7.2/apache2/php.ini
```

## mysql 환경 설정 파일

mysql 환경  설정파일인 my.cnf는 OS버전과 관계없이 /etc/mysql/my.cnf 에 위치하고 있으며, 실제로는 /etc/alternatives/my.cnf로 링크되어 있습니다.
``` bash
my.cnf : /etc/mysql/my.cnf -> /etc/alternatives/my.cnf
```

## 참고 URL
<a href="https://guide.ncloud-docs.com/docs/lamp-lamp-1-1" target="_blank" style="word-break:break-all;">https://guide.ncloud-docs.com/docs/lamp-lamp-1-1.html</a>
