---
date: 2021-10-07
last_modified_at: 2021-12-29
title: 서버 모니터링 서비스 Monitoring 설정 가이드
categories:
  - 81.manage
description: 네이버 클라우드 서버 모니터링 서비스 Monitoring 설정 가이드입니다
type: Document
set: server
order_number: 22
creator: ljh0519
v2_link: /management/ncloud_management_classic_monitoring_guide.html
---

## 개요
네이버 클라우드 Classic 환경에서 서버 모니터링을 설정하는 가이드입니다.  
Classic에서는 Monitoring 서비스와 Cloud Insite(Monitoring) 서비스 이렇게 2가지 서비스가 있는데 여기서는 Monitoring 서비스를 설정하는 방법에 대해 정리해보겠습니다.


#### 기본 모니터링 vs 상세 모니터링
- 기본 모니터링 : 네이버 클라우드에서 서버를 생성하면 별다른 설정을 하지 않아도 CPU, Memory 사용 등의 기본적인 항목들의 데이터를 확인할 수 있는데 이를 기본 모니터링이라고 하며 5분 단위의 데이터를 확인할 수 있습니다.
  확인 가능한 데이터는 [CPU Used], [Memory Used (except cache/buffer)], [Disk Used], [Swap Used], [Network In (bps)], [Network Out (bps)], [Disk Read (bytes)], [Disk Write (bytes)] 입니다.

- 상세 모니터링 : 기본 모니터링보다 훨씬 많고 상세한 데이터를 확인하고, 상태 감시와 통보 알람 설정을 통해 지정한 수치 이상의 상태가 되면 문자나 메일로 통보 받을 수 있는 모니터링 서비스이며, 1분 단위의 데이터를 확인할 수 있습니다. 
  네이버 클라우드 [Console] - [Monitoring] 서비스가 바로 [상세 모니터링]입니다. 또한 [Monitoring] 서비스에서는 Auto Scaling 그룹에 대한 이벤트 설정도 할 수 있습니다.


## 기본 모니터링
아래와 같이 서버 리스트에서 모니터링할 서버를 선택하고 위에 있는 [모니터링] 버튼을 클릭합니다.

<img src="../../images/ncp_management_monitoring_guide_01.jpg" alt="네이버 클라우드 서버 모니터링 서비스 Monitoring 설정 가이드" style="width:770px;align:center">

기본 모니터링에서는 5분 주기의 데이터를 확인할 수 있고, 날짜와 기간을 선택해서 데이터를 확인할 수 있습니다.

<img src="../../images/ncp_management_monitoring_guide_02.jpg" alt="네이버 클라우드 서버 모니터링 서비스 Monitoring 설정 가이드" style="width:770px;align:center">


## 상세 모니터링
[Monitoring] 서비스 즉, 상세 모니터링 서비스는 별도로 설정이 필요한데 설정하는 방법은 아래와 같이 기본 모니터링 화면에서 [상세 모니터링 설정] 링크를 클릭하거나, 
서버 리스트에서 [서버 관리 및 설정 변경] 메뉴 - [상세 모니터링 설정 변경] 메뉴를 클릭하면 됩니다.

<img src="../../images/ncp_management_monitoring_guide_03.jpg" alt="네이버 클라우드 서버 모니터링 서비스 Monitoring 설정 가이드" style="width:770px;align:center">

<img src="../../images/ncp_management_monitoring_guide_04.jpg" alt="네이버 클라우드 서버 모니터링 서비스 Monitoring 설정 가이드" style="width:770px;align:center">

[상세 모니터링] 신청화면입니다. 특별한 설정이 필요한 것은 아니므로 신청 화면에서 [예] 버튼을 클릭합니다.

<img src="../../images/ncp_management_monitoring_guide_05.jpg" alt="네이버 클라우드 서버 모니터링 서비스 Monitoring 설정 가이드" style="width:770px;align:center">

[상세 모니터링] 신청이 끝나면 바로 [Monitoring] - [Notification Recipient] 즉, 이벤트 통보 대상자 화면으로 넘어갑니다.  여기서는 모니터링 대상인 서버에서 이벤트가 발생했을 때 연락이 가도록 대상자를 등록하게 됩니다.  
기본으로 계정 사용자 정보가 등록되어 있는데, 혹시 등록되지 않았을 경우에는 [대상자 추가] 버튼을 클릭해서 통보 대상자를 등록합니다.

<img src="../../images/ncp_management_monitoring_guide_06.jpg" alt="네이버 클라우드 서버 모니터링 서비스 Monitoring 설정 가이드" style="width:770px;align:center">

## 대시보드

#### 통합 대시보드
[Monitoring] - [Integrated Dashboard] 즉, 통합 대시보드 메뉴로 이동합니다.  
여기서는 일별 이벤트 발생 횟수와 히스토리, 그리고, 각 모니터링 항목별로 상위 5개의 서버 정보를 통합해서 표시합니다. 

<img src="../../images/ncp_management_monitoring_guide_07.jpg" alt="네이버 클라우드 서버 모니터링 서비스 Monitoring 설정 가이드" style="width:770px;align:center">

#### 서버 대시보드
[Monitoring] - [Dashboard] - [Server Dashboard]에서는 전체 서버들의 상세한 모니터링 데이터를 리스트로 확인할 수 있습니다.

<img src="../../images/ncp_management_monitoring_guide_08.jpg" alt="네이버 클라우드 서버 모니터링 서비스 Monitoring 설정 가이드" style="width:770px;align:center">


좀 더 자세한 정보를 확인하고 싶으면 서버 리스트에서 원하는 서버를 선택합니다.

<img src="../../images/ncp_management_monitoring_guide_09.jpg" alt="네이버 클라우드 서버 모니터링 서비스 Monitoring 설정 가이드" style="width:770px;align:center">

서버를 선택하면 아래와 같이 모니터링 데이터를 차트로 확인할 수 있습니다.


<img src="../../images/ncp_management_monitoring_guide_10.jpg" alt="네이버 클라우드 서버 모니터링 서비스 Monitoring 설정 가이드" style="width:770px;align:center">

그리고 서버 Process와 File System 사용 정보도 상세하게 확인할 수 있습니다.

<img src="../../images/ncp_management_monitoring_guide_11.jpg" alt="네이버 클라우드 서버 모니터링 서비스 Monitoring 설정 가이드" style="width:770px;align:center">

## 감시-통보 설정
Monitoring 시스템에서 수집하는 성능 Item 중 사용자가 지정한 Item이 임계치 값을 벗어난 경우에 Event가 발생합니다. 
Event를 발생 하게 하는 조건을 감시설정이라 하며 해당 Event를 Mail or SMS로 수신 받는 설정을 통보설정이라고 합니다.

[Monitoring] - [Configuration] - [New Observation] 메뉴에서 우선 감시설정에 등록하고자 하는 서버를 먼저 선택하고 하단의 감시설정 버튼을 클릭합니다.

<img src="../../images/ncp_management_monitoring_guide_12.jpg" alt="네이버 클라우드 서버 모니터링 서비스 Monitoring 설정 가이드" style="width:770px;align:center">


다음으로 감시할 항목을 설정합니다. CPU, Memory 등 원하는 분류와 항목을 선택하고 임계치 등의 수치를 입력 후 추가 버튼을 클릭합니다.  
또한, 미리 Template을 등록해 두면 이후에 여러 서버들에 설정을 쉽게 적용할 수 있습니다.

#### 분류 항목별 주의 사항
- 프로세스: 상세 입력칸에 프로세스명을 입력할 때는 정규 표현식으로 입력해야 합니다.
- 파일 시스템:  상세 입력칸에 경로 입력 시 Linux의 경우 '/' 경로로 입력하고, Windows의 경우 'C:, D:' 등의 경로로 반드시 대문자로 입력합니다.

<img src="../../images/ncp_management_monitoring_guide_13.jpg" alt="네이버 클라우드 서버 모니터링 서비스 Monitoring 설정 가이드" style="width:770px;align:center">

{: .warning }
감시 통보설정에서 중요한 것은 지속시간입니다. 위 설정화면을 예로 설명하자면 CPU 사용율이 90% 이상인 상태가 5분 이상 지속되면 통보되도록 설정된 상태입니다. 
즉, 중간에 한번이라도 90% 미만으로 내려가면, 예를 들어 4분 50초 동안 90% 이상을 유지하다 85%로 내려간 경우라면 통보가 되지 않습니다. 
혹시 통보 알람을 테스트 하고 싶을 경우에는 지정한 상태가 5분 이상 지속되는 상황을 연출해서 테스트 해보셔야 합니다.

그 다음은 통보 받을 대상자와 통보 방법을 선택하고 [추가] 버튼을 클릭합니다. 통보 대상자가 리스트에 없을 경우 위쪽에 있는 [통보대상관리] 버튼을 클릭해서 대상자를 등록한 후에 다시 추가합니다.

<img src="../../images/ncp_management_monitoring_guide_14.jpg" alt="네이버 클라우드 서버 모니터링 서비스 Monitoring 설정 가이드" style="width:770px;align:center">


마지막으로 지금까지 설정한 내용들을 다시 한번 확인 한 후에 [최종 확인] 버튼을 클릭합니다.

<img src="../../images/ncp_management_monitoring_guide_15.jpg" alt="네이버 클라우드 서버 모니터링 서비스 Monitoring 설정 가이드" style="width:770px;align:center">


## 통보 알람 중지
서버 점검 등을 진행할 때는 통보 알람 기능을 중지 시켜두면 불필요한 알람을 받을 필요가 없어서 편리합니다.
[Monitoring] - [Configuration] - [Notification Stop] 메뉴에서 서버를 선택하고 [알람중지] 버튼을 클릭합니다.

<img src="../../images/ncp_management_monitoring_guide_16.jpg" alt="네이버 클라우드 서버 모니터링 서비스 Monitoring 설정 가이드" style="width:770px;align:center">

알람 중지 팝업 화면에서 목적과 기간 등을 선택해서 적용합니다.

<img src="../../images/ncp_management_monitoring_guide_17.jpg" alt="네이버 클라우드 서버 모니터링 서비스 Monitoring 설정 가이드" style="width:700px;align:center">


## 정보 수집 오류
간혹 서버 모니터링 성능 정보 수집에 오류가 발생하는 경우가 있습니다.  
그 중에서 Classic 환경의 Windows 서버에서 오류가 발생했을 때의 해결 방법을 아래 문서에 따로 정리해두었습니다.

- <a href="/81.manage/ncloud_management_monitoring_win_perf_data_error_troubleshoot/" target="_blank" style="word-break:break-all;">Windows 서버 모니터링 성능 정보 수집 오류 해결 방법</a>


## 참고 URL
1.  상세 모니터링 사용 가이드
	- <a href="https://guide.ncloud-docs.com/docs/ko/management-management-1-1" target="_blank" style="word-break:break-all;">https://guide.ncloud-docs.com/docs/ko/management-management-1-1</a>
