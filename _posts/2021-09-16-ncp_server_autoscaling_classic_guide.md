---
date: 2021-09-16
last_modified_at: 2021-09-16
title: Classic 환경에서 AutoScaling 설정하는 방법
categories:
  - 1.compute
description: 네이버 클라우드 Classic 환경에서 AutoScaling 설정하는 방법입니다.
type: Document
set: server
order_number: 21
creator: ljh0519
v2_link: /compute/ncloud_compute_autoscaling_classic_guide.html
---

## 개요
AutoScaling 서비스는 미리 등록한 설정에 따라 서버 수를 자동으로 증가 또는 감소시켜 안정적인 서비스를 유지하면서 비용을 절감할 수 있도록 해주는 서비스입니다.  
여기서는 네이버 클라우드 Classic 환경에서 AutoScaling 설정하는 방법을 정리해보겠습니다.

## 기본 설정
Auto Scaling 설정은 우선 [Auto Scaling] - [Launch Configuration]에서  
[Launch Configuration 생성] 버튼을 클릭하는 것으로 시작합니다.

<img src="/images/ncp_server_autoscaling_guide_classic_01.jpg" alt="네이버 클라우드 Classic 환경에서 AutoScaling 설정하는 방법" style="width:700px;align:center">

#### 서버 이미지 선택
서버 이미지는 네이버 클라우드에서 제공하는 기본 이미지를 선택할 수도 있고, 기존 서버로 만들어 둔 [내 서버 이미지]를 사용할 수도 있습니다. 여기서는 기본 이미지를 사용하는 것으로 하겠습니다.

<img src="/images/ncp_server_autoscaling_guide_classic_02.jpg" alt="네이버 클라우드 Classic 환경에서 AutoScaling 설정하는 방법" style="width:770px;align:center">

#### 서버 설정
스토리지 종류와 서버 타입 등을 선택합니다.

<img src="/images/ncp_server_autoscaling_guide_classic_03.jpg" alt="네이버 클라우드 Classic 환경에서 AutoScaling 설정하는 방법" style="width:770px;align:center">

#### 이름 설정
Launch Configuration의 이름을 입력합니다.

<img src="/images/ncp_server_autoscaling_guide_classic_04.jpg" alt="네이버 클라우드 Classic 환경에서 AutoScaling 설정하는 방법" style="width:770px;align:center">

#### 인증키 설정
인증키는 기존에 보유하고 있던 인증키를 이용해도 되고, 새로운 인증키를 설정해도 됩니다.

<img src="/images/ncp_server_autoscaling_guide_classic_05.jpg" alt="네이버 클라우드 Classic 환경에서 AutoScaling 설정하는 방법" style="width:770px;align:center">

#### 네트워크 접근 설정 (ACG)
ACG 설정도 기존에 보유하고 있던 ACG 중에서 선택해도 되고, 새로운 ACG를 생성해도 됩니다. 여기서는 새로운 ACG를 생성하는 방법으로 진행하겠습니다.

<img src="/images/ncp_server_autoscaling_guide_classic_06.jpg" alt="네이버 클라우드 Classic 환경에서 AutoScaling 설정하는 방법" style="width:770px;align:center">

ACG 이름을 입력하고, [myIp]를 클릭, 허용할 포트를 입력한 후 [추가] 버튼을 클릭합니다.

<img src="/images/ncp_server_autoscaling_guide_classic_07.jpg" alt="네이버 클라우드 Classic 환경에서 AutoScaling 설정하는 방법" style="width:770px;align:center">

#### 최종 확인
지금까지 설정한 정보를 마지막으로 확인 한 후에 이상이 없으면 [Launch Configuration 생성] 버튼을 클릭합니다.

<img src="/images/ncp_server_autoscaling_guide_classic_08.jpg" alt="네이버 클라우드 Classic 환경에서 AutoScaling 설정하는 방법" style="width:770px;align:center">


## Group 생성
다음으로 Auto Scaling Group을 생성합니다. [Auto Scaling] - [Auto Scaling Group]에서 [Auto Scaling Group 생성] 버튼을 클릭합니다.

<img src="/images/ncp_server_autoscaling_guide_classic_09.jpg" alt="네이버 클라우드 Classic 환경에서 AutoScaling 설정하는 방법" style="width:770px;align:center">

#### Launch Configuration 선택
위에서 생성했던 Launch Configuration을 선택합니다.

<img src="/images/ncp_server_autoscaling_guide_classic_10.jpg" alt="네이버 클라우드 Classic 환경에서 AutoScaling 설정하는 방법" style="width:770px;align:center">

#### 그룹 설정
여기서는 생성될 서버들의 이름과 최소, 최대 개수 등을 설정합니다.  
Auto Scaling Group당 최대 30대의 서버를 생성할 수 있고, Zone, NAT Gateway 설정은 생성 후 변경할 수 없으며, 변경이 필요하면 새로 생성하여 사용해야 합니다.  

<img src="/images/ncp_server_autoscaling_guide_classic_11.jpg" alt="네이버 클라우드 Classic 환경에서 AutoScaling 설정하는 방법" style="width:770px;align:center">

- **서버이름 Prefix** : 최대7자까지 지정할 수 있고, 나머지 이름의 뒷부분은 영문,숫자의 조합으로 무작위로 자동 생성됩니다.
- **서버 용량** : 최소, 최대, 기대 용량은 서버 대수를 의미하며 각각 **0~30까지 입력 가능**합니다.
- **쿨다운 기본값** : 새로운 서버가 생성되었다고 해도, init script 실행이나 업데이트 설치 등의 이유로 실제 서비스를 수행할 수 있을 정도로 준비되기까지는 시간이 소요될 수 있습니다. 
  즉, 쿨다운(Cooldown) 시간이란 실제 Scaling이 수행 중이거나 수행 완료된 이후에 모니터링 이벤트 알람이 발생하더라도 반응하지 않고 무시하도록 설정한 기간입니다.  
  **값을 입력하지 않으면 기본값인 300초가 적용됩니다**.

- **헬스체크 보류기간** : 서버 인스턴스가 생성되어 상태가 ‘운영 중’으로 바뀌었더라도, 서버의 업데이트 설치 등 작업에 의해서 헬스 체크에 정상 응답하지 못하는 경우가 생길 수 있습니다. 
  이런 경우 헬스 체크 보류 기간을 지정하면 해당 기간 동안에는 헬스 체크에 실패하더라도 서버에 이상이 있다고 판단하지 않습니다.  
  **값을 입력하지 않으면 기본값인 300초가 적용됩니다**.

<br />

#### 정책/일정 설정
정책과 일정을 여기서 바로 설정할 수도 있고 나중에 설정할 수도 있습니다. 여기서는 [나중에 설정]으로 선택하고 아래쪽에서 다시 살펴보겠습니다. 

<img src="/images/ncp_server_autoscaling_guide_classic_12.jpg" alt="네이버 클라우드 Classic 환경에서 AutoScaling 설정하는 방법" style="width:770px;align:center">

#### 통보 설정
통보 설정을 여기서 바로 설정할 수도 있고 나중에 설정할 수도 있습니다. 여기서는 [나중에 설정]으로 선택하고 아래쪽에서 다시 살펴보겠습니다. 

<img src="/images/ncp_server_autoscaling_guide_classic_13.jpg" alt="네이버 클라우드 Classic 환경에서 AutoScaling 설정하는 방법" style="width:770px;align:center">


#### 최종확인
생성된 Group 설정값들을 마지막으로 확인하고 [Auto Scaling Group 생성] 버튼을 클릭합니다.

<img src="/images/ncp_server_autoscaling_guide_classic_14.jpg" alt="네이버 클라우드 Classic 환경에서 AutoScaling 설정하는 방법" style="width:770px;align:center">

## 서버 생성 확인
위에서 Auto Scaling Group을 생성하면 아래와 같이 즉시 서버가 생성되는 것을 확인할 수 있습니다. 처음에는 [기대용량]에 입력한 개수만큼 서버가 생성됩니다.

<img src="/images/ncp_server_autoscaling_guide_classic_15.jpg" alt="네이버 클라우드 Classic 환경에서 AutoScaling 설정하는 방법" style="width:770px;align:center">


## 설정 관리 - 정책
위에서 나중에 설정하기로 하고 넘어갔던 Auto Scaling Group의 정책, 일정, 이력, 통보 설정 등을 확인해보겠습니다.  
[Auto Scaling Group]에서 해당 그룹을 선택하고, [설정 및 관리] 버튼을 클릭합니다.

<img src="/images/ncp_server_autoscaling_guide_classic_16.jpg" alt="네이버 클라우드 Classic 환경에서 AutoScaling 설정하는 방법" style="width:770px;align:center">

#### 정책 설정
먼저 [정책] 탭을 선택하고, [생성] 버튼을 클릭합니다.

<img src="/images/ncp_server_autoscaling_guide_classic_17.jpg" alt="네이버 클라우드 Classic 환경에서 AutoScaling 설정하는 방법" style="width:770px;align:center">

우선 서버를 증가시킬 정책으로 increase라는 이름을 입력하고, 증가시킬 서버 개수를 입력 후 [추가] 옵션을 선택하고 [생성] 버튼을 클릭합니다. 

<img src="/images/ncp_server_autoscaling_guide_classic_18.jpg" alt="네이버 클라우드 Classic 환경에서 AutoScaling 설정하는 방법" style="width:770px;align:center">

다음으로 서버를 감소시킬 정책으로 decrease라는 이름을 입력하고, 감소시킬 서버 개수를 입력 후 [반납] 옵션을 선택하고 [생성] 버튼을 클릭합니다. 

<img src="/images/ncp_server_autoscaling_guide_classic_19.jpg" alt="네이버 클라우드 Classic 환경에서 AutoScaling 설정하는 방법" style="width:770px;align:center">

서버 증가, 감소를 위한 정책 2가지가 생성된 것을 확인할 수 있습니다.

<img src="/images/ncp_server_autoscaling_guide_classic_20.jpg" alt="네이버 클라우드 Classic 환경에서 AutoScaling 설정하는 방법" style="width:770px;align:center">

## 그룹 이벤트 설정
위에서 설정한 정책이 언제 실행되게할 것인가 즉, 서버에 어떤 이벤트가 발생했을 때 정책을 실행할 것인가를 결정하는 그룹 이벤트를 설정합니다.  
그룹 이벤트 설정은 [Classic] - [Monitoring] - [Group Event Setting]에서 할 수 있으며, 위에서 만든 Auto Scaling 그룹을 선택하고 [그룹 이벤트 설정] 버튼을 클릭합니다.

<img src="/images/ncp_server_autoscaling_guide_classic_21.jpg" alt="네이버 클라우드 Classic 환경에서 AutoScaling 설정하는 방법" style="width:770px;align:center">

그룹 이벤트 설정 화면에 들어가면 위에서 만든 increase, decrease 2가지 정책이 리스트에 나타나는 것을 확인할 수 있습니다.

<img src="/images/ncp_server_autoscaling_guide_classic_22.jpg" alt="네이버 클라우드 Classic 환경에서 AutoScaling 설정하는 방법" style="width:770px;align:center">

정책이 발동되게 하는 이벤트는 여러가지가 있는데, 가장 많이 선택하는 것이 CPU 사용률입니다.  
여기서는 **CPU 사용률(used)이 60% 이상일 때 increase 정책**, **CPU 사용률(used)이 30% 이하일 때 decrease 정책**이 실행되도록 이벤트를 추가했습니다.

<img src="/images/ncp_server_autoscaling_guide_classic_23.jpg" alt="네이버 클라우드 Classic 환경에서 AutoScaling 설정하는 방법" style="width:770px;align:center">

## 설정 관리 - 일정
위에서는 서버에 설정한 이벤트가 발생했을 때 정책이 발동되도록 했지만, 그 외에도 특정한 시간대에 사용자가 몰리는 피크 타임이 일정하다면 해당 시간 전후로 서버를 늘리거나 감소시키는 방법도 가능합니다.  
여기 [일정] 탭에서 해당 내용을 설정해두면 피크 타임에 미리 서버를 증가시켜서 좀 더 안정적인 서비스가 가능하게 설정할 수 있습니다.

<img src="/images/ncp_server_autoscaling_guide_classic_24.jpg" alt="네이버 클라우드 Classic 환경에서 AutoScaling 설정하는 방법" style="width:770px;align:center">

## 설정 관리 - 이력
[이력] 탭에서는 서버가 생성되고 반납된 기록을 확인할 수 있습니다.

<img src="/images/ncp_server_autoscaling_guide_classic_25.jpg" alt="네이버 클라우드 Classic 환경에서 AutoScaling 설정하는 방법" style="width:770px;align:center">

## 설정 관리 - 통보
[통보] 탭에서는 서버가 생성되거나 반납될 때 지정한 담당자에게 SMS나 Email로 통보하도록 설정할 수 있습니다.

<img src="/images/ncp_server_autoscaling_guide_classic_26.jpg" alt="네이버 클라우드 Classic 환경에서 AutoScaling 설정하는 방법" style="width:770px;align:center">

## 설정 관리 - 서버
[서버] 탭은 현재 작동중인 서버 리스트를 확인할 수 있습니다.

<img src="/images/ncp_server_autoscaling_guide_classic_27.jpg" alt="네이버 클라우드 Classic 환경에서 AutoScaling 설정하는 방법" style="width:770px;align:center">


## 중지 - 서버삭제
마지막으로, Auto Scaling을 중지하고, 작동중인 서버를 한번에 모두 삭제하는 방법을 확인해보겠습니다.  

[Auto Scaling] - [Auto Scaling Group]에서 해당 그룹을 선택하고 상단에 있는 [수정] 버튼을 클릭합니다.

<img src="/images/ncp_server_autoscaling_guide_classic_28.jpg" alt="네이버 클라우드 Classic 환경에서 AutoScaling 설정하는 방법" style="width:770px;align:center">


여기서 **[최소 용량], [최대 용량], [기대 용량] 항목의 값을 모두 0으로 설정**하면, 현재 작동중인 서버가 모두 반납되고, 더 이상 서버가 추가로 생성되지 않아서 Auto Scaling이 중지되게 됩니다.  
사실 [최소 용량], [기대 용량] 2가지 항목만 0으로 설정해도 되지만, 혹시나 모르는 상황을 위해서 깔끔하게 3가지 항목 모두 0으로 설정합니다.

<img src="/images/ncp_server_autoscaling_guide_classic_29.jpg" alt="네이버 클라우드 Classic 환경에서 AutoScaling 설정하는 방법" style="width:770px;align:center">


## 서비스 제한사항
Auto Scaling 설정과 서버 스펙 등에 대한 제한 사항을 정리해보겠습니다.

#### 스펙 및 서비스 환경 제한 사항
- 총 디스크 사이즈 150GB 이하 서버만 가능
- Windows OS는 Windows 2012. 2016만 지원
- 내 서버 이미지의 경우, 원본 서버의 부팅 디스크 크기가 50GB인 경우만 지원(100GB 디스크에 대해서는 추후 지원 예정)

#### 설정 제한 사항
- 고객별 생성 가능한 Auto Scaling Group 최대 수: 10
- 고객별 생성 가능한 Launch Configuration 최대 수: 100
- Auto Scaling Group당 생성 가능한 스케줄(Scheduled Action) 최대 수: 100
- Auto Scaling Group당 생성 가능한 Scaling Policy 최대 수: 10
- Auto Scaling Group당 생성 가능한 최대 서버 수: 30대
- Auto Scaling Group당 연결 가능한 Load Balancer 최대 수 : 10


## 용어 정리
Auto Scaling에서 사용되는 주요 용어들을 정리해보겠습니다.

| 용어 | | 설명|
| :----: | :----: | :---- |
| Scale-in / Scale-out | | Auto Scaling Group을 생성하여 고객이 설정한 Policy에 따라 사용하고 있는 가상 서버의 자동 확장(Scale-out) 및 자동 축소(Scale-in)하도록 제공합니다. |
| Auto Scaling Group | | 여러 개의 서버 인스턴스들을 Auto Scaling Group 이라는 하나의 그룹으로 묶어 놓게 됩니다. |
| Launch Configuration | | Auto Scaling Group에서 가상 서버를 시작 구성하는 데 사용하는 템플릿입니다. Auto Scaling Group을 생성할 때는 Launch Configuration을 지정해야 합니다. |
| Auto Scaling Group의 최소 용량/최대 용량 | | Auto Scaling Group의 최소/최대 서버 수를 말합니다. 최소 서버 수의 경우, 항상 이 값과 같거나 이 값보다 더 큰 서버 수가 유지됩니다. 서버를 한 대도 보유하지 않을 수 있게 하려면 0으로 설정합니다. |
| 기대 용량 (Desired Capacity) | | 서버의 수는 기대 용량값에 따라서 조정됩니다. 이 값은 최소 용량 이상, 최대 용량 이하여야 합니다. 이 값이 지정되어 있지 않으면 초기에 최소 용량만큼 서버를 생성합니다. |
| 쿨다운 기본값(초) (Default Cooldown) | | Default Cooldown(초) 새로운 서버가 생성되었다고 해도, Init-Script 실행이나 업데이트 설치 등의 이유로 실제 서비스를 수행할 수 있을 정도로 준비되기까지는 시간이 소요될 수 있습니다. 쿨다운(Cooldown) 시간이란 실제 Scaling이 수행 중이거나 수행 완료된 이후에 모니터링 이벤트 알람이 발생하더라도 무시하도록 설정한 기간입니다. |
| 헬스체크 | | Auto Scaling Group의 가상 서버에 주기적인 상태 확인을 수행하여 상태가 비정상인 가상 서버를 식별하도록 Health Check를 합니다. |
| 헬스체크 보류 기간 | |	서버가 생성되어 ‘운영중’으로 변경되었더라도 서버의 업데이트 설치 등 작업에 의해서 헬스 체크에 정상 응답하지 못하는 경우가 생길 수 있습니다. 이런 경우 헬스 체크 보류기간을 지정하면 해당 기간 동안에는 헬스 체크에 실패하더라도 서버 헬스에 이상이 있다고 판단하지 않습니다. |
| 헬스체크 유형 | | 서버와 Load Balancer 둘 중에 선택할 수 있습니다. Auto Scaling Group 설정에서 Load Balancer 이름을 지정한 경우에는 헬스 체크 유형 역시 Load Balancer로 설정합니다. 이런 경우 Auto Scaling은 Load Balancer 헬스 체크 방식과 기준에 따라 서버의 상태를 판단합니다. |
| 반납 정책 | | Auto Scaling 과정에서 추가된 서버에 대한 Scale-in 작업에 대해, 고객이 API 질의 형식으로먼저 반납할 서버를 지정할 수 있습니다. 기본 설정은 먼저 생성된 서버부터 반납합니다. |
| Policy | | Auto Scaling이 일어나는 방식을 정의하고 있는데, 이를 ‘Policy’로 정의하고 있습니다. Auto Scale-out 이 발생할 때, 몇 대의 가상 서버를 늘릴 것인지, 반대로 Scale-in이 발생할 때 몇 대의 가상서버를 줄일 것인지를 정의합니다. 대수로 정의할 수 도 있고, %로 정의할 수도 있습니다. |

## 참고 URL
<a href="https://guide.ncloud-docs.com/docs/compute-autoscaling-autoscalingoverview" target="_blank" style="word-break:break-all;">https://guide.ncloud-docs.com/docs/compute-autoscaling-autoscalingoverview</a>
