# [NCP] Cloud Log Analytics에서 수집하는 로그 유형

## 개요
Cloud Log Analytics는 네이버 클라우드 플랫폼의 여러 서비스에서 발생하는 다양한 로그들을 한 곳에 모아 저장하고 손쉽게 분석하게 해주는 서비스입니다.

## 로그 템플릿 종류
Cloud Log Analytics에서 수집하는 각 종 서비스의 로그 템플릿 종류는 다음과 같습니다.

- Server syslog
- Apache 로그(Access log, Apache Error Log)
- MySQL 설치형 상품의 로그(Error Log, Slow Log)
- Microsoft SQL Server 설치형 상품의 Error Log
- Tomcat 로그(Catalina Log)
- Windows 서버의 Event Log
- Windows 서버의 각종 text 형식의 로그
- Cloud DB for MySQL 로그
- Cloud DB for MSSQL 로그
- Bare Metal Server 로그
- 그외 템플릿으로 제공되지 않는 로그도 Custom Log 기능으로 직접 대상 로그를 지정해서 수집할 수 있습니다.


## 로그 보관 기간
로그 데이터의 **보관 기간은 30일**로, 30일이 지난 데이터는 자동 삭제되며, 사전에 별도로 통지하지 않습니다.


## 참고 URL
<a href="https://docs.ncloud.com/ko/cla/cla-1-1.html" target="_blank">https://docs.ncloud.com/ko/cla/cla-1-1.html</a>


!!! info "문서 최종 수정일 : 2020-11-19" 