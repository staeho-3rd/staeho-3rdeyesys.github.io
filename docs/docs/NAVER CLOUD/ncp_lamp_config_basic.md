# NCP LAMP 기본 환경 설정 정보

## LAMP 기본설정

NCP(네이버 클라우드 플랫폼, 이하 네이버 클라우드)에서 제공하는 Application중에서 가장 대표적인 LAMP(Linux + Apache, Mysql, PHP)의 기본설정입니다.

### LAMP 홈디렉토리
네이버 클라우드에서 Linux 서버를 세팅하게 되면 기본적으로 root 계정으로 접속이 됩니다.
그래서 LAMP 서비스의 홈디렉토리도 /root/lamp로 설정됩니다. 

	- LAMP 서비스 홈디렉토리 : /root/lamp

### LAMP 서비스 전체 재시작
네이버 클라우드에서는 LAMP 전체 서비스를 빠르게 재시작할 수 있는 기능을 제공하고 있습니다.

	-  LAMP 서비스 전체 재시작 명령 : /root/lamp/lamp_restart.sh

위 명령을 실행하면 Apache 와 mysql이 순서대로 재시작됩니다.


### LAMP 서비스 설치 상태 확인
네이버 클라우드에서는 LAMP 서비스들의 설치 정보를 확인할 수 있는 기능도 제공하고 있습니다.

	- LAMP 설치 상태와 정보 확인 명령 : /root/lamp/lamp_info.sh

위 명령을 실행하면 LAMP의 기본 웹사이트 경로, 웹사이트 기본 디렉토리, mysql 초기 비번 안내, Apache 버전, mysql 버전, PHP와 Zend 버전 정보 등을 확인할 수 있습니다.



### LAMP 웹사이트 기본 디렉토리 
네이버 클라우드에서 제공하는 LAMP의 웹사이트 기본 디렉토리 위치는 다음과 같습니다.

	- 웹사이트 기본 디렉토리 : /ncp/data/www


## 참고 URL
<a href="https://docs.ncloud.com/ko/lamp/lamp-1-1.html" target="_blank">https://docs.ncloud.com/ko/lamp/lamp-1-1.html</a>

!!! info "문서 최종 수정일 : 2020-11-13"
