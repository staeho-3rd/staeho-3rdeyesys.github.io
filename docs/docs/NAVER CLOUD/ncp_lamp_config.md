# NCP LAMP 환경 설정 파일 위치

## LAMP 기본설정
- 홈디렉토리 : /root/lamp
- LAMP 서비스 전체 재시작 : $LAMP_HOME/lamp_restart.sh
- LAMP 서비스 설치 상태 확인하기 : $LAMP_HOME/lamp_info.sh

## 아파치 시작, 중지, 재시작

### CentOS 6.x 이하
	- 중지 : service httpd stop
	- 시작 : service httpd start
	- 재시작 : service httpd restart

### CentOS 7.x
	- 중지 : systemctl stop httpd
	- 시작 : systemctl start httpd
	- 재시작 : systemctl restart httpd

### Ubuntu
	- 중지 : systemctl stop apache2
	- 시작 : systemctl start apache2
	- 재시작 : systemctl restart apache2

## mysql 시작, 중지, 재시작

### CentOS 6.x 이하
	- 중지 : service mysqld stop
	- 시작 : service mysqld start
	- 재시작 : service mysqld restart

### CentOS 7.x
	- 중지 : systemctl stop mysqld
	- 시작 : systemctl start mysqld
	- 재시작 : systemctl restart mysqld

## Ubuntu
	- 중지 : systemctl stop mysql
	- 시작 : systemctl start mysql
	- 재시작 : systemctl restart mysql
	

## 웹 기본 디렉토리 
- /ncp/data/www

##  아파치 환경 설정 파일 

### CentOS

	- /etc/httpd/conf/httpd.conf

### Ubuntu

	- 기본 설정 : /etc/apache2/apache2.conf
	- 포트 설정 : /etc/apache2/ports.conf
	- 가상호스트 설정: /etc/apache2/sites-enabled/000-default.conf -> /etc/apache2/sites-available/000-default.conf
	- 로그 : /var/log/apache2
	- 기타 옵션 설정 :
	 /etc/apache2/mods-enabled/*.load -> /etc/apache2/mods-available/*.load
	 /etc/apache2/mods-enabled/*.conf -> /etc/apache2/mods-available/*.conf


## PHP 환경 설정 파일
	- php.ini : /etc/php/7.2/apache2/php.ini

## mysql 환경 설정 파일
	- my.cnf : etc/mysql/my.cnf -> /etc/alternatives/my.cnf

## 참고 URL
<a href="https://docs.ncloud.com/ko/lamp/lamp-1-1.html" target="_blank">https://docs.ncloud.com/ko/lamp/lamp-1-1.html</a>

