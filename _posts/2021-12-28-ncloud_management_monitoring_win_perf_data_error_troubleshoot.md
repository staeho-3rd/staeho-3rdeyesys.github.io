---
date: 2021-12-28
last_modified_at: 2021-12-28
title: Windows 서버 모니터링 성능 정보 수집 오류 해결 방법
categories:
  - 81.manage
description: Ncloud(네이버 클라우드) Classic 환경에서 Windows 서버의 모니터링 성능 정보를 수집할 때 발생하는 오류를 해결하는 방법입니다.
type: Document
creator: ljh0519
v2_link: /management/ncloud_management_monitoring_win_perf_data_error_troubleshoot.html
---

## 개요
Ncloud(네이버 클라우드) Monitoring 서비스를 이용해 서버 시스템 자원들(CPU, Memory 등)의 성능 정보를 수집, 모니터링할 때 간혹 성능 정보가 수집되지 않는 경우가 발생하기도 합니다. 
특히 Windows 서버의 경우 Windows 자체 성능 테이블을 참조하여 데이터를 수집하기 때문에 Windows 자체 성능 테이블에 문제가 발생할 경우 성능 정보를 수집하지 못하게 됩니다.  
여기서는 Ncloud(네이버 클라우드) Classic 환경에서 Windows 서버의 모니터링 성능 정보가 수집되지 않을 때 해결 방법을 정리해보겠습니다.

## 서버 환경
- Classic 환경
- Windows Server 2016 64bit R2 en

## 오류 상황
성능 정보가 수집되지 않을 경우 아래와 같이 Monitoring 정보에서 그래프가 특정 시점 이후로 끊겨서 표시되게 됩니다.

<img src="/images/ncloud_management_monitoring_win_perf_data_error_troubleshoot_01.png" alt="Ncloud(네이버 클라우드) Classic 환경에서 Windows 서버의 모니터링 성능 정보를 수집할 때 발생하는 오류를 해결하는 방법입니다" style="width:770px;align:center">

## 서버 상태 확인

#### 서버 성능 정보 상태 확인
우선 서버에 접속해서 서버의 작업 관리자 (Task Manager)를 실행, 성능(Performance)탭을 열고 CPU, Memory, Disk, Ethernet 등의 성능 정보가 제대로 표시되는지 확인합니다.

<img src="/images/ncloud_management_monitoring_win_perf_data_error_troubleshoot_04.png" alt="Ncloud(네이버 클라우드) Classic 환경에서 Windows 서버의 모니터링 성능 정보를 수집할 때 발생하는 오류를 해결하는 방법입니다" style="width:770px;align:center">

#### 모니터링 Agent 상태 확인
Ncloud(네이버 클라우드) 서버들에는 모니터링 정보를 수집하기 위한 Agent가 설치되는데 이 모니터링 Agent가 작동하고 있는지 확인합니다.

아래와 같이 Windows PowerShell에서 서비스를 검색합니다. 
Agent가 표시되는지 Status가 Running 상태인지 확인합니다.

``` bash
> Get-Service -name *nsight*
```

<img src="/images/ncloud_management_monitoring_win_perf_data_error_troubleshoot_03.png" alt="Ncloud(네이버 클라우드) Classic 환경에서 Windows 서버의 모니터링 성능 정보를 수집할 때 발생하는 오류를 해결하는 방법입니다" style="width:770px;align:center">


## 문제 해결

#### 서버 성능 정보에 문제가 있는 경우
혹시 서버 성능 정보가 제대로 표시 되지 않을 경우에는 cmd 창에서 아래 명령어로 성능 정보 테이블을 복구합니다.

``` bash
> lodctr /R
```
<img src="/images/ncloud_management_monitoring_win_perf_data_error_troubleshoot_05.png" alt="Ncloud(네이버 클라우드) Classic 환경에서 Windows 서버의 모니터링 성능 정보를 수집할 때 발생하는 오류를 해결하는 방법입니다" style="width:770px;align:center">

#### 서버 성능 정보가 정상인 경우
서버의 성능 정보가 제대로 표시되는데도 Monitoring 데이터가 제대로 수집되지 않는다면 cmd 창에서 모니터링 Agent를 다시 실행합니다.

우선 Agent를 중지 시킵니다.
``` bash
> sc stop noms_nsight
```
<img src="/images/ncloud_management_monitoring_win_perf_data_error_troubleshoot_07.png" alt="Ncloud(네이버 클라우드) Classic 환경에서 Windows 서버의 모니터링 성능 정보를 수집할 때 발생하는 오류를 해결하는 방법입니다" style="width:770px;align:center">

그리고 Agent를 다시 실행 시킵니다.
``` bash
> sc start noms_nsight
```

<img src="/images/ncloud_management_monitoring_win_perf_data_error_troubleshoot_06.png" alt="Ncloud(네이버 클라우드) Classic 환경에서 Windows 서버의 모니터링 성능 정보를 수집할 때 발생하는 오류를 해결하는 방법입니다" style="width:770px;align:center">

#### Monitoring 정보 수집 복구
문제가 해결되었다면 아래와 같이 Monitoring 정보가 제대로 수집되면서 그래프가 나타나기 시작합니다.

<img src="/images/ncloud_management_monitoring_win_perf_data_error_troubleshoot_02.png" alt="Ncloud(네이버 클라우드) Classic 환경에서 Windows 서버의 모니터링 성능 정보를 수집할 때 발생하는 오류를 해결하는 방법입니다" style="width:770px;align:center">

## 복구 불가 상황
위 방법대로 했는데에도 복구가 되지 않을 경우에는 다음 2가지 데이터를 모아서 Ncloud(네이버 클라우드) 고객센터로 문의하셔야 합니다.

- 모니터링 Agent 상태 정보
- 모니터링 Agent 로그

#### 모니터링 Agent 상태 정보 확인
모니터링 Agent가 설치된 경로에서 아래의 상태 정보 확인 명령어를 실행합니다. 

``` bash
C:\Program Files (x86)\NBP\NSight>noms_nsight.exe -status
```
<img src="/images/ncloud_management_monitoring_win_perf_data_error_troubleshoot_08.png" alt="Ncloud(네이버 클라우드) Classic 환경에서 Windows 서버의 모니터링 성능 정보를 수집할 때 발생하는 오류를 해결하는 방법입니다" style="width:770px;align:center">

#### 모니터링 Agent 로그
모니터링 Agent가 설치된 경로에 있는 파일과 폴더를 모두 압축합니다.

``` bash
C:\Program Files (x86)\NBP\NSight\
```

## 참고 URL
1.  성능 수집 오류 해결 FAQ
	- <a href="https://www.ncloud.com/support/faq/all/1292" target="_blank" style="word-break:break-all;">https://www.ncloud.com/support/faq/all/1292</a>
