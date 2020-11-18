# LAMP(CentOS) 기본 명령어와 환경 설정 파일 위치

## Apache 시작, 중지, 재시작

Apache 시작, 중지, 재시작 명령어는 CentOS 6에서 사용하던 것이 CentOS 7이 되면서 변경되었습니다.
CentOS 6.x 이하에서는 service {서비스명} [stop|start|restart] 순서였다면 **CentOS 7.X 에서는 systemctl [stop|start|restart] {서비스명} 의 순서**로 바뀌었습니다.
내용을 정리하면 다음과 같습니다.

### CentOS 6.x 이하
	- 중지 : service httpd stop
	- 시작 : service httpd start
	- 재시작 : service httpd restart

### CentOS 7.x
	- 중지 : systemctl stop httpd
	- 시작 : systemctl start httpd
	- 재시작 : systemctl restart httpd


## mysql 시작, 중지, 재시작
mysql도 Apache와 마찬가지 방식으로 CentOS 6 이하와 CentOS 7에서 사용하는 명령어가 변경되었습니다.

### CentOS 6.x 이하
	- 중지 : service mysqld stop
	- 시작 : service mysqld start
	- 재시작 : service mysqld restart

### CentOS 7.x
	- 중지 : systemctl stop mysqld
	- 시작 : systemctl start mysqld
	- 재시작 : systemctl restart mysqld


##  Apache 환경 설정 파일 

Apache의 환경 설정 파일은 CentOS의 버전과 관계없이 모두 동일합니다.

	- httpd.conf : /etc/httpd/conf/httpd.conf


## PHP 환경 설정 파일
PHP의 환경 설정파일인 php.ini는  PHP버전에 해당 하는 디렉토리에 위치하고 있습니다.

	- php.ini : /etc/php/7.2/apache2/php.ini

## mysql 환경 설정 파일

mysql 환경  설정파일인 my.cnf는 OS버전과 관계없이 /etc/mysql/my.cnf 에 위치하고 있으며, 실제로는 /etc/alternatives/my.cnf로 링크되어 있습니다.

	- my.cnf : /etc/mysql/my.cnf -> /etc/alternatives/my.cnf

## 참고 URL
<a href="https://docs.ncloud.com/ko/lamp/lamp-1-1.html" target="_blank">https://docs.ncloud.com/ko/lamp/lamp-1-1.html</a>

!!! info "문서 최종 수정일 : 2020-11-13"

