---
date: 2021-01-19
title: AutoScaling 이벤트 설정하는 방법
categories:
  - 1.compute
description: 네이버 클라우드 AutoScaling 그룹 이벤트 설정하는 방법
type: Document
set: server
order_number: 12
---

## 개요
네이버 클라우드에서 AutoScaling을 설정할 때 중요한 것이 이벤트 설정입니다.  
예를 들어 CPU 사용률이 70%가 넘는 상태가 1분 이상 지속되면 서버를 늘리는 작업이 진행되도록 설정한다고 할 때, CPU가 70% 이상인 상태가 1분 이상 지속되는 이벤트가 발생하는지 체크하는 것을 말합니다.  
현재 네이버 클라우드 Console에서는 AutoScaling 그룹을 생성한 후에 바로 이 이벤트 설정을 하는 방법에 대한 메뉴나 링크가 없어서 그에 대한 내용을 정리해보겠습니다.


## AutoScaling Group 생성
AutoScaling 이벤트를 설정하려면 우선 AutoScaling Group을 생성해야 합니다.  
**Console - Auto Scaling - Auto Scaling Group 메뉴**에서 설정할 수 있습니다.  
AutoScaling Group을 설정한 후에는 AutoScaling Group Event를 설정해야 하니 해당 기능이 있는 메뉴로 이동할 수 있도록 버튼이나 링크가 생겼으면 합니다.

<img src="../../images/ncp_server_autoscaling_event_setting_01.jpg" alt="네이버 클라우드 AutoScaling 그룹 이벤트 설정하는 방법" style="width:800px;align:center">

## AutoScaling Group Event 설정
AutoScaling의 그룹 이벤트를 설정하는 곳은 **Monitoring 메뉴**에 있습니다.  
CPU 사용량 등의 서버 상태를 확인해야 하는 것이다 보니 **Console - Monitoring - Group Event Setting 메뉴**에서 관련된 이벤트 설정을 할 수 있습니다.  
아래 스샷에서 확인할 수 있듯이 앞에서 생성한 AutoScaling Group이 나타납니다. 만약 AutoScaling Group을 생성하지 않았다면 여기서는 아무런 설정도 할 수 없습니다.  
혹시 AutoScaling Group이 생성되지 않은 상태에서 이 메뉴에 들어오는 경우에는 AutoScaling Group 생성 페이지로 이동하는 버튼이나 링크가 추가되길 바랍니다.

<img src="../../images/ncp_server_autoscaling_event_setting_02.jpg" alt="네이버 클라우드 AutoScaling 그룹 이벤트 설정하는 방법" style="width:800px;align:center">



## 참고 URL
<a href="https://docs.ncloud.com/ko/management/management-1-1.html" target="_blank">https://docs.ncloud.com/ko/management/management-1-1.html</a>


> 문서 최종 수정일 : 2021-01-19