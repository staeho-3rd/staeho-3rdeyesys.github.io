---
date: 2021-08-19
last_modified_at: 2021-08-19
title: Sub Account 생성 가이드
categories:
  - 81.manage
description: 네이버 클라우드Sub Account 생성 가이드입니다
type: Document
set: account
order_number: 1
---

## 개요
Sub Account(서브 계정)는 네이버 클라우드의 서비스 자원을 여러 사용자가 동시에 이용, 관리해야 할 때 필요한 만큼만 권한을 부여해서 사용할 수 있게 해주는 서비스입니다.  
Sub Account를 사용하면 사내 담당부서나 담당자별로 지정된 자원에만 접근하도록 하거나, 협력사에게 일부 접근권한을 부여해야 할 때 효과적입니다.

## 특징
네이버 클라우드의 Sub Account는 다음과 같은 특징이 있습니다.

- 별도의 로그인 페이지를 이용하여 접속
- 대시보드에서 서브 계정 수, 그룹 수, 정책 수, 접속 페이지 설정을 확인할 수 있음
- 그룹, 정책, 역할을 생성해 상세한 권한 설정을 할 수 있음
- Access Key를 별도로 생성해서 사용할 수 있음


## 주요 권한

- 네이버 클라우드 메인 계정과 동일한 접근 권한  
	**NCP_ADMINISTRATOR** 정책을 부여하시면 메인 계정과 동일하게 네이버클라우드플랫폼 내 포털, 콘솔을 접근할 수 있습니다

- 네이버 클라우드 콘솔 내 모든 상품/서비스 접근 권한  
	**NCP_INFRA_MANAGER** 정책을 부여하시면 메인 계정과 동일하게 콘솔 내 모든 상품/서비스에 접근할 수 있습니다.

- 네이버 클라우드 콘솔 내 각 상품/서비스별 접근 권한  
	**NCP_상품/서비스명_MANAGER/VIEWER** 정책을 부여하시면 해당 상품/서비스에 접근할 수 있습니다.

- 네이버 클라우드 포털 내 마이페이지 "이용관리" 메뉴 접근 권한  
	**NCP_FINANCE_MANAGER** 정책을 부여하시면 포털 마이페이지 내 "서비스 이용내역/현황, 프로모션 내역, 청구 내역 추세" 메뉴에 접근할 수 있습니다.


## 서브 계정 생성
[Console] - [Sub Account] - [Sub Accounts]에서 [서브 계정 생성] 버튼을 클릭합니다.

<img src="../../images/ncp_manage_sub_account_01.jpg" alt="네이버 클라우드Sub Account 생성 가이드" style="width:770px;align:center">

서브 계정 생성 화면에서는 로그인 아이디, 이름, 이메일 주소를 우선 입력합니다.  

다음으로 접근 유형으로 [Console 접근]과 [API 접근]을 모두 허용할 것인지, 하나만 허용할 것인지 선택하고, 
Conosole 접근의 경우에 지정한 IP대역에서만 접근하게 할 것인지, 모두 허용할 것인지도 선택합니다.  

휴대폰 문자인증 또는 이메일 인증 등의 2차 인증을 적용할 것인지도 선택합니다.

<img src="../../images/ncp_manage_sub_account_02.jpg" alt="네이버 클라우드Sub Account 생성 가이드" style="width:770px;align:center">


마지막으로 로그인 비밀번호를 입력하고, 해당 서브 계정으로 로그인 시에 비밀번호를 변경하도록 할 것인지 선택할 수 있습니다.

<img src="../../images/ncp_manage_sub_account_03.jpg" alt="네이버 클라우드Sub Account 생성 가이드" style="width:770px;align:center">


## 계정 정책 추가
생성된 서브 계정을 클릭하면 서브 계정 상세 설정 화면으로 이동합니다.

<img src="../../images/ncp_manage_sub_account_04.jpg" alt="네이버 클라우드Sub Account 생성 가이드" style="width:770px;align:center">

서브 계정 상세 화면에서는 정책, 그룹, Access Key를 추가하고 관리할 수 있습니다.  우선 정책 탭에서 [추가] 버튼을 클릭합니다.

<img src="../../images/ncp_manage_sub_account_05.jpg" alt="네이버 클라우드Sub Account 생성 가이드" style="width:770px;align:center">

정책 추가 화면에서는 네이버 클라우드에서 기본으로 제공하는 [관리형 정책]과 사용자가 직접 정의하는 [사용자 정의 정책]이 있습니다.  
우선 [관리형 정책]에서 필요한 정책을 선택하고 [추가] 버튼을 클릭합니다.

<img src="../../images/ncp_manage_sub_account_06.jpg" alt="네이버 클라우드Sub Account 생성 가이드" style="width:770px;align:center">

여기서는 네이버 클라우드의 Server 상품 내 모든 기능을 이용할 수 있는 권한인 [NCP_SERVER_MANAGER] 선택했고, 아래와 같이 추가된 것을 확인할 수 있습니다. 

<img src="../../images/ncp_manage_sub_account_07.jpg" alt="네이버 클라우드Sub Account 생성 가이드" style="width:770px;align:center">

## 접속 환경 설정
네이버 클라우드 서브 계정은 별도의 접속 페이지가 존재하는데, 
[Sub Account] - [Dashboard]에서 서브 계정으로 접속하기 위한 페이지 주소를 입력할 수 있습니다.  
https://www.ncloud.com/nsa/ 까지는 공통이고, 그 다음 페이지 주소를 직접 입력하고 저장하시면 됩니다. 

<img src="../../images/ncp_manage_sub_account_09.jpg" alt="네이버 클라우드Sub Account 생성 가이드" style="width:770px;align:center">

다음으로 [미사용 세션 만료 설정]에서는 로그인된 서브 계정이 아무 활동없이 미사용일 경우 지정한 시간 기준으로 자동 로그아웃이 되도록 설정할 수 있습니다.

<img src="../../images/ncp_manage_sub_account_10.jpg" alt="네이버 클라우드Sub Account 생성 가이드" style="width:680px;align:center">

[비밀번호 만료 설정]은 비밀번호 만료를 활성화할 경우 지정된 만료일을 초과했을 때 비밀번호를 변경해야만 접속 할 수 있습니다.  
활성화 하지 않았을 경우에는 90일이 지난 후에 비밀번호 변경 안내 팝업만 나타납니다.

<img src="../../images/ncp_manage_sub_account_11.jpg" alt="네이버 클라우드Sub Account 생성 가이드" style="width:680px;align:center">

## 접속 - 로그인
위에서 설정한 접속 페이지 [ **https://www.ncloud.com/nsa/\*\*\*\*\*\***] 에 접속하면 아래와 같이 서브 계정 로그인 화면을 확인할 수 있습니다.  
바꿔말하면 서브 계정(Sub Account)은 반드시 위에서 설정한 접속 페이지로 접속해야 합니다.

<img src="../../images/ncp_manage_sub_account_18.jpg" alt="네이버 클라우드Sub Account 생성 가이드" style="width:770px;align:center">


## Access Key 설정
서브 계정에서 Access Key를 사용해야 할 경우 서브 계정 상세 화면 [Access Key]탭에서 [추가] 버튼을 클릭해 생성할 수 있습니다. 

<img src="../../images/ncp_manage_sub_account_08.jpg" alt="네이버 클라우드Sub Account 생성 가이드" style="width:770px;align:center">

## 계정 그룹 설정
여러 개의 서브 계정을 묶어서 하나의 그룹으로 구성하면 해당 그룹의 서브 계정에 동일한 정책을 동시에 적용할 수 있습니다.

<img src="../../images/ncp_manage_sub_account_12.jpg" alt="네이버 클라우드Sub Account 생성 가이드" style="width:770px;align:center">

## 사용자 정의 정책 생성
서브 계정 정책에는 네이버 클라우드에서 기본으로 제공하는 [관리형 정책] 외에도 사용자가 직접 설정하는 [사용자 정의 정책]도 사용할 수 있습니다. 
[Sub Account] - [Plicies]에서 [정책 생성] 버튼을 클릭합니다. 

<img src="../../images/ncp_manage_sub_account_13.jpg" alt="네이버 클라우드Sub Account 생성 가이드" style="width:770px;align:center">

#### VPC 환경에서 정책 생성
정책 이름을 입력하고, VPC를 선택 후, 어떤 서비스 상품에 적용할 것인지 선택합니다.

<img src="../../images/ncp_manage_sub_account_14.jpg" alt="네이버 클라우드Sub Account 생성 가이드" style="width:770px;align:center">

다음으로 Actions 항목에서는 읽기 권한인 View 또는 수정 권한인 Change를 선택하면 아래쪽에 리소스별로 상세한 권한 설정을 할 수 있는 화면이 나타납니다. 
모든 설정을 마치고, 아래쪽 [적용대상 추가] 버튼을 클릭하면 됩니다. 

<img src="../../images/ncp_manage_sub_account_17.jpg" alt="네이버 클라우드Sub Account 생성 가이드" style="width:770px;align:center">

#### Classic 환경에서 정책 생성
Classic 환경에서는 리소스별 상세 권한은 설정할 수 없고, View 또는 Change를 선택한 후에 [적용대상 추가] 버튼을 클릭하면 모든 권한이 추가됩니다.
<img src="../../images/ncp_manage_sub_account_15.jpg" alt="네이버 클라우드Sub Account 생성 가이드" style="width:770px;align:center">

아래와 같이 선택한 서비스 상품에 대해 모든 리전, 모든 리소스에 선택한 권한이 지정됩니다.

<img src="../../images/ncp_manage_sub_account_16.jpg" alt="네이버 클라우드Sub Account 생성 가이드" style="width:770px;align:center">




## 참고 URL
1.  Sub Account 사용 가이드
	- <a href="https://guide.ncloud-docs.com/docs/management-management-4-1" target="_blank" style="word-break:break-all;">https://guide.ncloud-docs.com/docs/management-management-4-1</a>
