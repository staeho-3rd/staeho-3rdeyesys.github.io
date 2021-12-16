---
date: 2021-12-10
update: 2021-12-10
title: Cloud Log Analytics 설정 가이드
categories:
  - 7.analytics
description: Ncloud(네이버 클라우드) Cloud Log Analytics를 설정하는 방법입니다.
type: Document
creator: ljh0519
---

## 개요
Cloud Log Analytics는 Ncloud(네이버 클라우드)가 제공하는 여러 서비스에서 발생하는 다양한 로그들을 한 곳에 모아 저장하고 손쉽게 분석할 수 있는 서비스로, 
검색 기능을 이용해 여러 종류의 로그를 한 곳에서 한번에 조회하고 분석할 수 있어 효과적인 로그 관리가 가능합니다. 

## 로그 템플릿 종류
Cloud Log Analytics는 텍스트 형식으로 생성되는 모든 종류의 로그 데이터 파일을 수집할 수 있는데, 사전에 제공되는 로그 템플릿 종류는 다음과 같습니다.

- Server syslog
- Apache 로그(Access log, Apache Error Log)
- MySQL 설치형 상품의 로그(Error Log, Slow Log)
- Microsoft SQL Server 설치형 상품의 Error Log
- Tomcat 로그(Catalina Log)
- Windows 서버의 Event Log
- Windows 서버의 각종 text 형식의 로그
- Cloud DB for MySQL 로그
- Cloud DB for MSSQL 로그
- Cloud DB for MongoDB 로그
- Application Server Launcher 로그
- Application Load Balancer 로그
- Search Engine Service 로그
- Cloud Data Streaming Service 로그
- Bare Metal Server 로그
- 그외 템플릿으로 제공되지 않는 로그도 Custom Log 기능으로 직접 대상 로그를 지정해서 수집할 수 있습니다

## 저장 용량
- **최대 100GB**까지 저장할 수 있습니다.
- 100GB 용량을 초과했을 경우 추가 저장 용량 확보를 위해 과거부터 전날까지의 데이터가 삭제될 수 있습니다.
- CLA로 수집되는 로그량이 하루 10GB 이상을 넘거나 천만 건 이상일 경우 저장된 로그 검색시 성능에 제한이 발생할 수 있습니다.
- 저장 용량과 저장 기간을 더 늘리길 원할 경우 고객지원으로 문의해야 합니다.
- 과거 데이터를 보관하려면 [자동 보내기] 기능을 이용하여 과거 데이터를 Object Storage로 백업할 수 있습니다.

## 로그 보관 기간
- Cloud Log Analytics 서비스는 **최대 30일** 동안 데이터가 보관되며, 검색 및 대시보드에서 확인할 수 있습니다.
- 30일이 지난 데이터는 과거 데이터부터 순차적으로 삭제됩니다.
- 30일이 지나지 않았더라도 저장된 데이터가 100GB를 초과하면 과거부터 전날까지의 데이터가 매일 삭제될 수 있습니다.

## 이용신청
Ncloud(네이버 클라우드) 콘솔 [Cloud Log Analytics] - [Subscription]에서 [이용 신청] 버튼을 클릭합니다. 
Cloud Log Analytics는 Classic, VPC 환경 공통 서비스이므로 어느쪽 환경에서 이용신청을 해도 상관없습니다.

<img src="../../images/ncloud_analytics_cloud_log_analytics_guide_01.png" alt="Ncloud(네이버 클라우드) Cloud Log Analytics를 설정하는 방법" style="width:770px;align:center">


## 설정 - Linux
먼저 Linux 서버에서 설정하는 방법을 알아보겠습니다.  
[Cloud Log Analytics] - [Management]에서 로그를 수집할 서버를 선택하고, [수집 설정] 버튼을 클릭합니다.  

<img src="../../images/ncloud_analytics_cloud_log_analytics_guide_02.png" alt="Ncloud(네이버 클라우드) Cloud Log Analytics를 설정하는 방법" style="width:770px;align:center">

Log 수집 설정 화면에서 수집할 로그 템플릿을 선택하거나, 직접 [Custom Log]를 선택해서 로그 형태를 설정한 후에 [적용] 버튼을 클릭합니다.

<img src="../../images/ncloud_analytics_cloud_log_analytics_guide_03.png" alt="Ncloud(네이버 클라우드) Cloud Log Analytics를 설정하는 방법" style="width:770px;align:center">

#### 로그 수집 Agent 설치
Log 수집 설정을 마치면 [로그 수집 Agent] 설치 안내가 나옵니다.  
로그 수집 Agent 설치 명령어에는 URL 뒤쪽에 설치 하려는 서버에 해당하는 **설치키(Install Key)**가 포함되어 있습니다. 그러므로 URL을 수정해서도 안되고 다른 서버에 사용할 수도 없습니다.

``` bash
# VPC 환경
~# curl -s http://cm.vcla.ncloud.com/setUpClaVPC/{설치키(Install Key)} | sudo sh

# Classic 환경
~# curl -s http://cm.cla.ncloud.com/setUpCla/{설치키(Install Key)} | sudo sh
```

<img src="../../images/ncloud_analytics_cloud_log_analytics_guide_04.png" alt="Ncloud(네이버 클라우드) Cloud Log Analytics를 설정하는 방법" style="width:770px;align:center">

서버에 실제로 설치해보면 아래와 같이 설치 과정이 진행되고,  
설치가 완료되면 마지막에 **Finish Installation**이라는 메시지가 출력됩니다.

<img src="../../images/ncloud_analytics_cloud_log_analytics_guide_06.png" alt="Ncloud(네이버 클라우드) Cloud Log Analytics를 설정하는 방법" style="width:770px;align:center">

설치된 Agent가 제대로 작동하고 있는지 확인해보면 아래와 같이 active (running) 상태인 것을 확인할 수 있습니다.
``` bash
~# systemctl status filebeat
```
<img src="../../images/ncloud_analytics_cloud_log_analytics_guide_07.png" alt="Ncloud(네이버 클라우드) Cloud Log Analytics를 설정하는 방법" style="width:770px;align:center">

## 설정 - Windows
다음으로 Windows 서버에서 설정하는 방법을 살펴보겠습니다.  
마찬가지로 서버를 선택하고 [수집 설정] 버튼을 클릭합니다.

<img src="../../images/ncloud_analytics_cloud_log_analytics_guide_11.png" alt="Ncloud(네이버 클라우드) Cloud Log Analytics를 설정하는 방법" style="width:770px;align:center">

로그 수집 설정에서 Log Template은 [EventLog]를 선택합니다.

<img src="../../images/ncloud_analytics_cloud_log_analytics_guide_12.png" alt="Ncloud(네이버 클라우드) Cloud Log Analytics를 설정하는 방법" style="width:770px;align:center">

설정을 마치면 Agent 설치 가이드를 확인할 수 있습니다.  
서버에서 [Windows PowerShell]을 열고, 아래 명령어를 실행합니다. 마찬가지로 마지막에는 설치 서버에 해당하는 **설치키**가 포함되어 있습니다.

``` bash
# VPC 환경
Invoke-Expression $((New-Object System.Net.WebClient).DownloadString("http://cm.vcla.ncloud.com/setUpwinClaVPC/{설치키(Install Key)}"))

# Classic 환경
Invoke-Expression $((New-Object System.Net.WebClient).DownloadString("http://cm.cla.ncloud.com/setUpwinClaVPC/{설치키(Install Key)}"))
```
<img src="../../images/ncloud_analytics_cloud_log_analytics_guide_13.png" alt="Ncloud(네이버 클라우드) Cloud Log Analytics를 설정하는 방법" style="width:770px;align:center">

설치가 완료되면 마지막에 **Finish Installation**이라는 메시지가 출력됩니다.

<img src="../../images/ncloud_analytics_cloud_log_analytics_guide_14.png" alt="Ncloud(네이버 클라우드) Cloud Log Analytics를 설정하는 방법" style="width:770px;align:center">

#### Windows Server 2019 지원 여부
현재 Windows Server 2019는 지원하지 않고, 2022년 상반기 지원 예정입니다.  
Windows Server 2019는 아래와 같이 선택을 해도 [수집 설정] 버튼을 활성화되지 않습니다.

<img src="../../images/ncloud_analytics_cloud_log_analytics_guide_15.png" alt="Ncloud(네이버 클라우드) Cloud Log Analytics를 설정하는 방법" style="width:770px;align:center">

## 로그 확인
Agent 설치 후 [Dashboard]를 확인해보면 로그가 수집되고 있을 것을 알 수 있습니다.

<img src="../../images/ncloud_analytics_cloud_log_analytics_guide_08.png" alt="Ncloud(네이버 클라우드) Cloud Log Analytics를 설정하는 방법" style="width:770px;align:center">

[Search] 메뉴에서는 로그 내용을 자세히 검색, 확인할 수 있고, 굳이 서버에 접속하지 않더라도 필요한 로그를 콘솔 화면에서 직접 확인할 수 있습니다.

<img src="../../images/ncloud_analytics_cloud_log_analytics_guide_09.png" alt="Ncloud(네이버 클라우드) Cloud Log Analytics를 설정하는 방법" style="width:770px;align:center">

## 로그 백업
Cloud Log Analytics는 수집된 로그를 Object Storage로 내보내기하거나 Excel 파일로 다운로드 해서 백업할 수 있는 기능을 지원합니다.

#### 수동 백업
[Search] 메뉴에 [Object Storage로 내보내기]와 [X 다운로드] 버튼이 있습니다.

<img src="../../images/ncloud_analytics_cloud_log_analytics_guide_24.png" alt="Ncloud(네이버 클라우드) Cloud Log Analytics를 설정하는 방법" style="width:770px;align:center">

[Object Storage로 내보내기] 버튼을 클릭하면 내보내기 할 버킷을 선택할 수 있습니다.

<img src="../../images/ncloud_analytics_cloud_log_analytics_guide_25.png" alt="Ncloud(네이버 클라우드) Cloud Log Analytics를 설정하는 방법" style="width:600px;align:center">

#### 자동 백업
[Export Log] 메뉴에서 [자동 내보내기 설정]을 클릭합니다.

<img src="../../images/ncloud_analytics_cloud_log_analytics_guide_16.png" alt="Ncloud(네이버 클라우드) Cloud Log Analytics를 설정하는 방법" style="width:770px;align:center">

설정 화면에서 내보내기를 할 Object Storage의 버킷을 선택합니다. 혹시 버킷이 생성되지 않았다면 Object Storage로 가서 먼저 버킷을 생성하고 와야 합니다.

<img src="../../images/ncloud_analytics_cloud_log_analytics_guide_17.png" alt="Ncloud(네이버 클라우드) Cloud Log Analytics를 설정하는 방법" style="width:600px;align:center">

내보내기는 하루에 한번 진행되므로 설정 후 다음 날 Object Storage에서 아래와 같이 파일이 저장되어 있는 것을 확인할 수 있습니다.

<img src="../../images/ncloud_analytics_cloud_log_analytics_guide_20.png" alt="Ncloud(네이버 클라우드) Cloud Log Analytics를 설정하는 방법" style="width:770px;align:center">


## 로그 수집 해제
더 이상 로그를 수집할 필요가 없어지면, 로그 수집 설정을 해제하면 됩니다.  

#### Linux 서버 로그 수집 해제
서버를 선택하고 [수집 해제] 버튼을 클릭합니다.

<img src="../../images/ncloud_analytics_cloud_log_analytics_guide_22.png" alt="Ncloud(네이버 클라우드) Cloud Log Analytics를 설정하는 방법" style="width:770px;align:center">

로그 수집 해제를 위한 가이드에서 로그 수집 Agent 삭제 명령어를 복사합니다.

``` bash
# VPC 환경
~# curl -s http://cm.vcla.ncloud.com/removeCla | sudo sh

# Classic 환경
~# curl -s http://cm.cla.ncloud.com/removeCla | sudo sh
```
<img src="../../images/ncloud_analytics_cloud_log_analytics_guide_05.png" alt="Ncloud(네이버 클라우드) Cloud Log Analytics를 설정하는 방법" style="width:770px;align:center">

Agent 삭제 명령어를 실행하면 아래와 같이 **Success Remove Agent** 메시지가 출력됩니다.

<img src="../../images/ncloud_analytics_cloud_log_analytics_guide_10.png" alt="Ncloud(네이버 클라우드) Cloud Log Analytics를 설정하는 방법" style="width:770px;align:center">


#### Windows 서버 로그 수집 해제
마찬가지로 서버를 선택하고 [수집 해제] 버튼을 클릭합니다.

<img src="../../images/ncloud_analytics_cloud_log_analytics_guide_22.png" alt="Ncloud(네이버 클라우드) Cloud Log Analytics를 설정하는 방법" style="width:770px;align:center">

로그 수집 해제를 위한 가이드에서 로그 수집 Agent 삭제 명령어를 복사합니다.

``` bash
# VPC 환경
Invoke-Expression $((New-Object System.Net.WebClient).DownloadString("http://cm.vcla.ncloud.com/removewinCla"))

# Classic 환경
Invoke-Expression $((New-Object System.Net.WebClient).DownloadString("http://cm.cla.ncloud.com/removewinCla"))
```
<img src="../../images/ncloud_analytics_cloud_log_analytics_guide_18.png" alt="Ncloud(네이버 클라우드) Cloud Log Analytics를 설정하는 방법" style="width:770px;align:center">

Agent 삭제 명령어를 실행하면 아래와 같이 **Remove Agent** 메시지가 출력됩니다.

<img src="../../images/ncloud_analytics_cloud_log_analytics_guide_19.png" alt="Ncloud(네이버 클라우드) Cloud Log Analytics를 설정하는 방법" style="width:770px;align:center">


#### Windows 서버 Agent 삭제 오류 상황
Windows 서버에서 Agent 삭제를 시도할 때 아래와 같이 오류 메시지가 발생하는 경우가 있습니다.  
이때는 당황하지 마시고, Agent 삭제 명령어를 다시 한번 실행하면 됩니다.

오류의 원인 : 로그 수집 설정에서 EventLog만 선택했을 경우 발생합니다.  
로그 수집 Agent는 윈도 이벤트 로그 수집을 위한 winlogbeat와 그 외 로그를 수집하기 위한 filebeat 두가지가 설치되는데, EventLog만 수집하도록 설정할 경우 filebeat는 실행되지 않습니다. 
그 상태에서 Agent를 삭제하려고 하면 실행중이 아닌 filebeat를 실행 중지 시키려고 시도하게 되고, 결국 오류가 발생합니다.  
그러므로 심각한 오류는 아니고 만약을 위해 Agent 삭제 명령어를 한번 더 실행시키는 것으로 문제는 해결됩니다.

``` bash
Stop-Service : Cannot find any service with service name 'filebeat'.
```

<img src="../../images/ncloud_analytics_cloud_log_analytics_guide_21.png" alt="Ncloud(네이버 클라우드) Cloud Log Analytics를 설정하는 방법" style="width:770px;align:center">

## 참고 URL
1.  Cloud Log Analytics 사용 가이드
	- <a href="https://guide.ncloud-docs.com/docs/cla-cla-1-1" target="_blank" style="word-break:break-all;">https://guide.ncloud-docs.com/docs/cla-cla-1-1</a>

> 문서 최종 수정일 : 2021-12-14