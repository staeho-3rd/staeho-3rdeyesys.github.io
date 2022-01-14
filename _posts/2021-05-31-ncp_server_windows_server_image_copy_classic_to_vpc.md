---
date: 2021-05-31
last_modified_at: 2021-05-31
title: Classic 환경 Windows 서버 이미지를 VPC 환경으로 복제하는 방법
categories:
  - 1.compute
description: 네이버 클라우드 Classic 환경 Windows 서버 이미지를 VPC 환경으로 복제하는 방법을 소개합니다.
type: Document
set: server
order_number: 17
---

## 개요
네이버 클라우드 Classic 환경의 Windows 서버 이미지를 VPC 환경으로 복제하려면 몇가지 사전 작업과 조건이 있습니다.  이 사전 작업과 조건을 차례대로 정리해보겠습니다.

## OS 조건 확인
Classic 환경의 서버 이미지를 VPC 환경으로 복제하는 것이므로 **VPC에서 지원하는 OS만 복제 가능**합니다.  
즉, 현재 네이버 클라우드 VPC 환경에서 지원하는 Windows OS 버전은  
**Windows Server 2016 (64-bit) English Edition** 하나이기 때문에 2016 R2 등의 OS를 사용 중이라면 
VPC에서 지원하지 않아 복제가 불가능하니 VPC에서 지원할 때까지 작업을 보류하셔야 합니다.

<img src="../../images/ncp_server_windows_server_image_copy_classic_to_vpc_14.jpg" alt="네이버 클라우드 Classic 환경 Windows 서버 이미지를 VPC 환경으로 복제하는 방법" style="width:770px;align:center">
**(2021년 6월 1일 현재 VPC 환경에서 지원하는 Windows OS 버전)**

## 사전 작업 확인
우선 사전 작업이 어떤 것이 있는지 확인해 보기 위해 Windows 서버 이미지를 하나 만들어서 상단에 있는 [VPC로 복제] 버튼을 클릭해 봅니다.  

안내 팝업에는 다음과 같은 작업이 필요하다고 적혀 있습니다.

1. 백신 솔루션 확인 및 필요시 백신 삭제
2. 백신 상태 확인 스크립트 실행
3. 내 서버 이미지 다시 생성 후 VPC 복제 기능 사용

<img src="../../images/ncp_server_windows_server_image_copy_classic_to_vpc_01.jpg" alt="네이버 클라우드 Classic 환경 Windows 서버 이미지를 VPC 환경으로 복제하는 방법" style="width:770px;align:center">


## 백신 삭제
Windows 서버에 설치된 백신 솔루션을 확인해서, ApexOne 또는 OfficeScan이라면 백신 삭제가 필요하며, 그 외 솔루션은 삭제하지 않아도 됩니다.

#### ApexOne 삭제방법
- 제어판 -> 프로그램 추가제거로 이동 후 **‘Trend Micro Apex One Security Agent’**를 선택하고, 삭제합니다.  
- C:\Program Files (x86) 경로의 Trend Micro\Security Agent 폴더를 삭제합니다.  
  (삭제되지 않는 파일은 그대로 두셔도 됩니다)

#### OfficeScan 삭제방법
- 제어판 -> 프로그램 추가제거로 이동 후 **‘Trend Micro officescan Security Agent’**를 선택하고, 삭제합니다. 
- C:\Program Files (x86) 경로의 Trend Micro\Security Agent 폴더를 삭제합니다.

#### 그 외 솔루션
- 백신 삭제 불필요

<img src="../../images/ncp_server_windows_server_image_copy_classic_to_vpc_02.jpg" alt="네이버 클라우드 Classic 환경 Windows 서버 이미지를 VPC 환경으로 복제하는 방법" style="width:700px;align:center">

<img src="../../images/ncp_server_windows_server_image_copy_classic_to_vpc_03.jpg" alt="네이버 클라우드 Classic 환경 Windows 서버 이미지를 VPC 환경으로 복제하는 방법" style="width:700px;align:center">

<img src="../../images/ncp_server_windows_server_image_copy_classic_to_vpc_13.jpg" alt="네이버 클라우드 Classic 환경 Windows 서버 이미지를 VPC 환경으로 복제하는 방법" style="width:770px;align:center">


## 백신상태 확인
백신이 모두 정상적으로 제거되었는지 확인하는 툴을 실행합니다.

#### 백신상태 확인 Tool 다운로드
Windows PowerShell을 열고, 다음의 명령어를 실행하면 바탕화면에 Tool이 다운로드 됩니다.

``` shell
cd c:\users\Administrator\Desktop 
Invoke-WebRequest http://init.ncloud.com/vpcporting/real/ncp_vac_chk.exe -outfile ncp_vac_chk.exe
```
<img src="../../images/ncp_server_windows_server_image_copy_classic_to_vpc_04.jpg" alt="네이버 클라우드 Classic 환경 Windows 서버 이미지를 VPC 환경으로 복제하는 방법" style="width:732px;align:center">

#### 백신상태 확인 Tool 실행
바탕화면에 다운로드 된 **ncp_vac_chk.exe** 파일을 관리자 권한으로 실행합니다.

<img src="../../images/ncp_server_windows_server_image_copy_classic_to_vpc_05.jpg" alt="네이버 클라우드 Classic 환경 Windows 서버 이미지를 VPC 환경으로 복제하는 방법" style="width:600px;align:center">

#### 백신상태 확인 체크 결과 확인
백신상태 확인이 정상적으로 수행되면 바탕화면에 notifyFreeAntiVirusRemoved, result 파일 2개가 생성되며, 백신상태 확인이 비정상으로 수행되면 result 파일 1개만 생성이 됩니다.  

notifyFreeAntiVirusRemoved 파일이 생성되지 않거나 해당 파일의 내용이 OK가 아닌 경우 백신상태 확인이 정상적으로 완료되지 못한 경우이므로 백신솔루션에 따라 필요시 백신이 잘 제거되었는지 다시 한번 더 체크 후 해당 Tool을 실행해야 합니다.  

백신상태 확인이 정상적으로 완료되지 않고 문제가 지속되면 result 파일을 첨부하여 네이버 클라우드 고객센터에 문의하셔야 합니다.

<img src="../../images/ncp_server_windows_server_image_copy_classic_to_vpc_06.jpg" alt="네이버 클라우드 Classic 환경 Windows 서버 이미지를 VPC 환경으로 복제하는 방법" style="width:600px;align:center">

## 서버 이미지 복제

#### 서버 이미지 재 생성
위에서 백신상태 확인 체크가 정상적으로 완료되었다면, 서버 이미지를 다시 생성합니다.

#### 서버 이미지 VPC로 복제
[콘솔] - [Classic] - [Server] - [Server Image]에서 새로 생성한 이미지를 선택하고 상단의 [VPC 로 복제] 버튼을 클릭합니다.  
위에서도 설명했듯이 현재는 **Windows Server 2016 (64-bit) English Edition** 버전만 지원합니다.

<img src="../../images/ncp_server_windows_server_image_copy_classic_to_vpc_07.jpg" alt="네이버 클라우드 Classic 환경 Windows 서버 이미지를 VPC 환경으로 복제하는 방법" style="width:770px;align:center">

복제되는 신규 서버 이미지 이름을 입력하고 [생성] 버튼을 클릭합니다.

<img src="../../images/ncp_server_windows_server_image_copy_classic_to_vpc_08.jpg" alt="네이버 클라우드 Classic 환경 Windows 서버 이미지를 VPC 환경으로 복제하는 방법" style="width:740px;align:center">

실제 VPC 환경에 복제 진행 중인 이미지가 표시되기까지는 다소 시간이 걸릴 수 있으니 잠시 기다리시면 확인하실 수 있습니다.

<img src="../../images/ncp_server_windows_server_image_copy_classic_to_vpc_09.jpg" alt="네이버 클라우드 Classic 환경 Windows 서버 이미지를 VPC 환경으로 복제하는 방법" style="width:550px;align:center">

#### 복제된 서버 이미지 확인
복제가 완료된 서버 이미지의 상세 정보를 살펴보면 **Classic 복제 여부** 항목이 **Y**로 표시되는 것을 확인할 수 있습니다.

<img src="../../images/ncp_server_windows_server_image_copy_classic_to_vpc_10.jpg" alt="네이버 클라우드 Classic 환경 Windows 서버 이미지를 VPC 환경으로 복제하는 방법" style="width:770px;align:center">

## 서버 생성
Classic 환경에서 VPC 환경으로 복제된 서버 이미지로 새로운 서버가 생성되는지 확인해보겠습니다.  
이미지를 선택하고 [서버 생성] 버튼을 클릭해서 서버 생성화면에서 정보를 입력하고 서버를 생성합니다.

<img src="../../images/ncp_server_windows_server_image_copy_classic_to_vpc_11.jpg" alt="네이버 클라우드 Classic 환경 Windows 서버 이미지를 VPC 환경으로 복제하는 방법" style="width:770px;align:center">

정상적으로 생성된 서버를 확인할 수 있습니다.

<img src="../../images/ncp_server_windows_server_image_copy_classic_to_vpc_12.jpg" alt="네이버 클라우드 Classic 환경 Windows 서버 이미지를 VPC 환경으로 복제하는 방법" style="width:770px;align:center">



## 참고 URL
<a href="https://guide.ncloud-docs.com/docs/compute-compute-5-1-v2" target="_blank" style="word-break:break-all;">https://guide.ncloud-docs.com/docs/compute-compute-5-1-v2</a>
