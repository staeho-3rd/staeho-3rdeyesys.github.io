---
date: 2021-10-15
last_modified_at: 2021-10-18
title: 모니터링 서비스 Cloud Insight 설정 가이드
categories:
  - 81.manage
description: 네이버 클라우드 모니터링 서비스 Cloud Insight 설정 가이드입니다
type: Document
set: server
order_number: 23
v2_link: /management/ncloud_management_cloud_insight_guide.html
---

## 개요
네이버 클라우드와 사용자 애플리케이션의 성능/운영 지표를 통합 관리하고, 장애나 이벤트가 발생했을 때 SMS 및 Email로 알람 통보를 해주는 서비스인 Cloud Insight 서비스를 설정하는 방법에 대해 정리해보겠습니다.


#### Monitoring 서비스 vs Cloud Insight 서비스
Classic 환경에 있는 Monitoring 서비스와 Cloud Insight 서비스의 차이점을 정리하면 다음과 같습니다.

- Monitoring 서비스는 Classic 환경에서만 사용할 수 있다.
- Cloud Insight 서비스는 Classic, VPC 양쪽에서 모두 사용할 수 있고, 두가지 환경을 통합해서 보여준다.
- Monitoring 서비스는 Server 제품 (AutoScaling 포함)만 모니터링 한다.
- Cloud Insight 서비스는 Server 뿐만 아니라 Object Storage, Load Balancer 등 10여개의 서비스를 모니터링 한다.

## 적용 서비스 리스트
위 비교에서도 설명했듯이 Cloud Insight는 Server 뿐만 아니라 네이버 클라우드의 다양한 서비스의 모니터링 정보를 확인할 수 있는데 해당 리스트는 다음과 같습니다.

#### Classic 
- Server
- Load Balancer

#### VPC
- Server(VPC)
- Load Balancer(VPC)
- Cloud DB for MySQL(VPC)
- Cloud DB for MSSQL(VPC)
- Cloud DB for Redis(VPC)
- Cloud DB for MongoDB(VPC)
- Cloud Hadoop(VPC)
- Auto Scaling Group(VPC)
- Kubernetes Service(VPC)
- Search Engine Service(VPC)
- Cloud Data Streaming Service(VPC)

#### 통합
- Cloud Search
- Object Storage


## 이용 신청
Cloud Insight 서비스는 이용 신청을 해야 사용할 수 있습니다. Classic, VPC 어떤 환경에서든 [Cloud Insight(Monitoring)] - [Subscription]에서 [상품 이용 신청] 버튼을 클릭해서 이용 신청을 합니다.

<img src="../../images/ncp_management_cloud_insight_guide_01.jpg" alt="네이버 클라우드 모니터링 서비스 Cloud Insight 설정 가이드" style="width:770px;align:center">

## 대시보드
이용 신청을 한 후에 [Dashboard] 메뉴에 가면 아래와 같이 현재 사용 중인 서비스 중에서 모니터링 가능한 서비스 리스트를 확인할 수 있습니다.  
또한 기본으로 제공되는 대시보드에서 확인할 수 있는 모니터링 항목을 각 서비스별로 고정되어 있습니다. 
추가적인 항목을 확인하려면 별도로 대시보드를 생성해야 하는데 이에 대해서는 아래쪽에서 확인해보겠습니다.

{: .success }
일부 서비스의 경우 리스트에 나타날 때까지 시간이 걸릴 수도 있습니다. 여유있게 1시간 정도 후에 확인해보시면 됩니다.

<img src="../../images/ncp_management_cloud_insight_guide_02.jpg" alt="네이버 클라우드 모니터링 서비스 Cloud Insight 설정 가이드" style="width:770px;align:center">

#### Load Balancer 모니터링
<img src="../../images/ncp_management_cloud_insight_guide_08.jpg" alt="네이버 클라우드 모니터링 서비스 Cloud Insight 설정 가이드" style="width:770px;align:center">

#### Object Storage 모니터링
<img src="../../images/ncp_management_cloud_insight_guide_09.jpg" alt="네이버 클라우드 모니터링 서비스 Cloud Insight 설정 가이드" style="width:770px;align:center">

#### Server 모니터링
위에서 설명했듯이 Server 제품도 GPU 관련 항목을 제외하면 5가지 정도의 기본 모니터링 항목을 확인할 수 있습니다. 

<img src="../../images/ncp_management_cloud_insight_guide_03.jpg" alt="네이버 클라우드 모니터링 서비스 Cloud Insight 설정 가이드" style="width:770px;align:center">

## 커스텀 대시보드
좀 더 상세한 모니터링 데이터를 확인하려면 커스텀 대시보드가 필요하고 그전에 [상세 모니터링]을 설정해야 합니다. 서버 설정에서 [상세 모니터링 설정 변경] 메뉴를 클릭합니다.

<img src="../../images/ncp_management_cloud_insight_guide_04.jpg" alt="네이버 클라우드 모니터링 서비스 Cloud Insight 설정 가이드" style="width:770px;align:center">

[상세 모니터링 신청] 팝업에서 [예] 버튼을 클릭합니다.

<img src="../../images/ncp_management_cloud_insight_guide_05.jpg" alt="네이버 클라우드 모니터링 서비스 Cloud Insight 설정 가이드" style="width:500px;align:center">

#### 대시보드 생성
[Dashboard] 화면에서 [대시보드 생성] 버튼을 클릭합니다.

<img src="../../images/ncp_management_cloud_insight_guide_06.jpg" alt="네이버 클라우드 모니터링 서비스 Cloud Insight 설정 가이드" style="width:770px;align:center">

생성 팝업에서 대시보드 이름과 설명을 입력합니다.
<img src="../../images/ncp_management_cloud_insight_guide_07.jpg" alt="네이버 클라우드 모니터링 서비스 Cloud Insight 설정 가이드" style="width:500px;align:center">


생성된 대시보드에서 [위젯 추가] 버튼을 클릭해서 위젯을 추가합니다.
<img src="../../images/ncp_management_cloud_insight_guide_10.jpg" alt="네이버 클라우드 모니터링 서비스 Cloud Insight 설정 가이드" style="width:770px;align:center">

위젯 이름을 입력하고, 종류는 [Time Series], [Pie Chart], [Table], [Index], [Markdown] 중에서 하나를 선택합니다.  여기서는 [CPU Usage]와 [Time Series]를 선택했습니다.

<img src="../../images/ncp_management_cloud_insight_guide_11.jpg" alt="네이버 클라우드 모니터링 서비스 Cloud Insight 설정 가이드" style="width:770px;align:center">

다음으로 데이터 설정에서 CPU 사용율에 해당하는 CPU/used_rto 등 필요한 항목을 선택하고 [선택 항목 추가] 버튼을 클릭합니다.

<img src="../../images/ncp_management_cloud_insight_guide_12.jpg" alt="네이버 클라우드 모니터링 서비스 Cloud Insight 설정 가이드" style="width:770px;align:center">

항목을 추가하면 아래쪽 화면에 다음과 같이 리스트를 확인할 수 있습니다. 설정이 완료되었으면 [다음] 버튼을 클릭해 마지막 확인을 하고 생성을 완료합니다.

<img src="../../images/ncp_management_cloud_insight_guide_13.jpg" alt="네이버 클라우드 모니터링 서비스 Cloud Insight 설정 가이드" style="width:770px;align:center">

위에서 추가한 [CPU Usage] 위젯과 함께 추가로 [Server - Load Average], [File System], [Memory Usage], [Net Work (Max In/Out bps)] 데이터를 확인할 수 있는 위젯을 추가하면 다음과 같은 대시보드를 확인할 수 있습니다.

<img src="../../images/ncp_management_cloud_insight_guide_14.jpg" alt="네이버 클라우드 모니터링 서비스 Cloud Insight 설정 가이드" style="width:770px;align:center">

## 통보 대상자 등록
이제 이벤트를 등록하고 알람을 통보 받을 대상자를 등록해보겠습니다.  
먼저 통보 대상자 그룹을 생성합니다.  [Cloud Insight(Monitoring)] - [Notification Recipient] 메뉴에서 [전체 대상자] 옆에 있는 [ + ] 버튼을 클릭하고 아래 입력칸에 그룹명을 입력합니다.

<img src="../../images/ncp_management_cloud_insight_guide_15.jpg" alt="네이버 클라우드 모니터링 서비스 Cloud Insight 설정 가이드" style="width:770px;align:center">

네이버 클라우드 계정 생성을 할 때 기본으로 1명의 대상자가 등록됩니다. 해당 대상자를 위에서 생성한 그룹에 할당하기 위해 선택하고 [할당] 버튼을 클릭합니다.

<img src="../../images/ncp_management_cloud_insight_guide_20.jpg" alt="네이버 클라우드 모니터링 서비스 Cloud Insight 설정 가이드" style="width:770px;align:center">

그룹 할당 팝업에서 [할당] 버튼을 클릭합니다.
<img src="../../images/ncp_management_cloud_insight_guide_21.jpg" alt="네이버 클라우드 모니터링 서비스 Cloud Insight 설정 가이드" style="width:500px;align:center">

대상자 리스트에서 해당 대상자에 그룹이 할당된 것을 확인할 수 있습니다.

<img src="../../images/ncp_management_cloud_insight_guide_22.jpg" alt="네이버 클라우드 모니터링 서비스 Cloud Insight 설정 가이드" style="width:770px;align:center">

## 이벤트 규칙 등록
이제 이벤트를 등록해보겠습니다. [Cloud Insight(Monitoring)] - [Configuration] - [Event Rue]에서 [Event Rule 생성] 버튼을 클릭합니다.

<img src="../../images/ncp_management_cloud_insight_guide_16.jpg" alt="네이버 클라우드 모니터링 서비스 Cloud Insight 설정 가이드" style="width:770px;align:center">

이벤트 규칙을 생성해서 감시가 필요한 상품을 선택합니다. 여기서는 [Sever(VPC)]를 선택하겠습니다.

<img src="../../images/ncp_management_cloud_insight_guide_17.jpg" alt="네이버 클라우드 모니터링 서비스 Cloud Insight 설정 가이드" style="width:770px;align:center">

[감시 대상 설정]에서 [전체 보기]를 선택하고, 감시 대상에 체크한 후 [다음] 버튼을 클릭합니다.

<img src="../../images/ncp_management_cloud_insight_guide_18.jpg" alt="네이버 클라우드 모니터링 서비스 Cloud Insight 설정 가이드" style="width:770px;align:center">

감시 항목 및 조건 설정에서 [전체 보기]를 선택하고, CPU, Memory 등의 항목 중에서 원하는 항목을 선택합니다.  
여기서는 [CPU]에서 CPU 사용율에 해당하는 [CPU/used_rto]를 선택하고, 90% 이상인 상태가 5분 이상 지속되면 경고 알림을 보내도록 설정했습니다. 

<img src="../../images/ncp_management_cloud_insight_guide_19.jpg" alt="네이버 클라우드 모니터링 서비스 Cloud Insight 설정 가이드" style="width:770px;align:center">

다음으로 감시 대상에서 설정한 이벤트가 발생했을 때 어떤 액션을 취할 것인가를 설정해보겠습니다.  
설정 가능한 액션은 [알림 메시지 발송], [Integration], [Cloud Functions], [Auto Scaling 정책] 중에서 선택할 수 있는데, 여기서는 [알림 메시지 발송]을 선택하겠습니다.  
통보 대상자 그룹을 선택하고, [Email]과 [SMS]중에서 원하는 것을 선택하고, [다음] 버튼을 클릭합니다.

<img src="../../images/ncp_management_cloud_insight_guide_23.jpg" alt="네이버 클라우드 모니터링 서비스 Cloud Insight 설정 가이드" style="width:770px;align:center">

마지막으로 이벤트 규칙 이름을 입력하고, 앞에서 설정한 내용들을 확인한 후에 [생성] 버튼을 클릭합니다.
<img src="../../images/ncp_management_cloud_insight_guide_24.jpg" alt="네이버 클라우드 모니터링 서비스 Cloud Insight 설정 가이드" style="width:770px;align:center">

## 유지보수 계획 설정
앞에서 설정한 이벤트 규칙이 업데이트나 점검 등의 유지보수가 진행되는 동안에도 작동되면 유지보수 시간 동안 쉼없이 통보 알람이 울리게 됩니다.  
이런 불편함이 없도록 Cloud Insight에서는 유지보수 계획 일정을 등록해두면 등록된 기간 동안에는 이벤트 규칙에 따른 통보알람이 울리지 않습니다.

유지보수 일정은 아래와 같이 달력 행태나 리스트 형태로 확인 가능하며 [유지보수 계획 설정하기] 버튼으로 일정을 등록할 수 있습니다.

<img src="../../images/ncp_management_cloud_insight_guide_25.jpg" alt="네이버 클라우드 모니터링 서비스 Cloud Insight 설정 가이드" style="width:770px;align:center">

제목을 입력하고, 작업 기간, 작업 대상, 디멘션을 선택하면 아래와 같이 선택한 대상과 디멘션이 리스트로 나타납니다.  보통 위에서 설정했던 이벤트 규칙에 해당하는 항목들을 선택하면 됩니다. 

<img src="../../images/ncp_management_cloud_insight_guide_26.jpg" alt="네이버 클라우드 모니터링 서비스 Cloud Insight 설정 가이드" style="width:770px;align:center">

유지보수 계획을 설정하면 아래와 같이 일정에서 확인할 수 있으며 해당 기간 동안에는 이벤트 통보가 진행되지 않습니다. 

<img src="../../images/ncp_management_cloud_insight_guide_27.jpg" alt="네이버 클라우드 모니터링 서비스 Cloud Insight 설정 가이드" style="width:770px;align:center">


## 참고 URL
1.  Cloud Insight 소개
	- <a href="https://guide.ncloud-docs.com/docs/cloudinsight-cloudinsightoverview" target="_blank" style="word-break:break-all;">https://guide.ncloud-docs.com/docs/cloudinsight-cloudinsightoverview</a>

2.  Cloud Insight 사용 가이드
	- <a href="https://guide.ncloud-docs.com/docs/cloudinsight-cloudinsightconsole" target="_blank" style="word-break:break-all;">https://guide.ncloud-docs.com/docs/cloudinsight-cloudinsightconsole</a>
