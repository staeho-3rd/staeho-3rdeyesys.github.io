---
date: 2021-05-26
last_modified_at: 2021-05-31
title: Classic 환경에서 서버 이미지를 다른 계정에 공유하는 방법
categories:
  - 1.compute
description: 네이버 클라우드 Classic 환경에서 서버 이미지를 다른 계정에 공유하는 방법을 소개합니다.
type: Document
set: server
order_number: 16
v2_link: /compute/ncloud_compute_server_image_share_classic.html
---

## 개요
네이버 클라우드 Classic 환경에서 내 서버 이미지를 다른 계정에 공유하는 방법입니다.  
Linux와 Windows OS별 차이는 없으며, 일단은 서버 이미지가 생성되어 있는 것을 가정해서 내용을 정리해보겠습니다.

## 공유 권한 설정
네이버 클라우드 콘솔 [Server] - [Server Image]에서 공유할 서버 이미지를 선택하고 상단 메뉴에 있는 [공유 권한 설정]을 클릭합니다.

<img src="../../images/ncp_server_server_image_share_classic_01.jpg" alt="네이버 클라우드 Classic 환경에서 서버 이미지를 다른 계정에 공유하는 방법 " style="width:770px;align:center">

다음으로 서버 이미지를 공유할 계정 즉, 로그인 ID를 입력합니다.

<img src="../../images/ncp_server_server_image_share_classic_02.jpg" alt="네이버 클라우드 Classic 환경에서 서버 이미지를 다른 계정에 공유하는 방법 " style="width:770px;align:center">

로그인 ID 입력 후 [권한 추가] 버튼을 클릭하고, [적용] 버튼을 클릭해 설정을 적용합니다.

<img src="../../images/ncp_server_server_image_share_classic_03.jpg" alt="네이버 클라우드 Classic 환경에서 서버 이미지를 다른 계정에 공유하는 방법 " style="width:770px;align:center">

공유 권한 설정을 적용하면 서버 이미지 정보에 [공유 중]이라고 표시되고, [공유 받은 계정] 정보도 확인할 수 있습니다.

#### Linux (CentOS, Ubuntu)
<img src="../../images/ncp_server_server_image_share_classic_04.jpg" alt="네이버 클라우드 Classic 환경에서 서버 이미지를 다른 계정에 공유하는 방법 " style="width:770px;align:center">

#### Windows
<img src="../../images/ncp_server_server_image_share_classic_12.jpg" alt="네이버 클라우드 Classic 환경에서 서버 이미지를 다른 계정에 공유하는 방법 " style="width:770px;align:center">

## 공유 상태 확인

이제 공유 받은 계정으로 접속해보면 [Server Image]에 추가된 서버 이미지가 보이고, [공유 받음] 표시를 확인할 수 있습니다.

#### Linux (CentOS, Ubuntu)
<img src="../../images/ncp_server_server_image_share_classic_05.jpg" alt="네이버 클라우드 Classic 환경에서 서버 이미지를 다른 계정에 공유하는 방법 " style="width:770px;align:center">

#### Windows
<img src="../../images/ncp_server_server_image_share_classic_13.jpg" alt="네이버 클라우드 Classic 환경에서 서버 이미지를 다른 계정에 공유하는 방법 " style="width:770px;align:center">

다음으로 공유 받은 서버 이미지를 선택하고, [서버 생성] 버튼을 클릭합니다.

<img src="../../images/ncp_server_server_image_share_classic_06.jpg" alt="네이버 클라우드 Classic 환경에서 서버 이미지를 다른 계정에 공유하는 방법 " style="width:770px;align:center">

서버 생성 정보 입력 화면이 나타나면서 문제 없이 서버를 생성할 수 있다는 것을 알 수 있습니다.

<img src="../../images/ncp_server_server_image_share_classic_07.jpg" alt="네이버 클라우드 Classic 환경에서 서버 이미지를 다른 계정에 공유하는 방법 " style="width:770px;align:center">

## 공유 권한 삭제

이제 공유된 권한을 삭제해보겠습니다.  
처음 서버 이미지를 공유할 때 처럼 서버 이미지를 선택하고, [공유 권한 설정]을 클릭하고, [공유 받은 계정]을 삭제합니다.

<img src="../../images/ncp_server_server_image_share_classic_08.jpg" alt="네이버 클라우드 Classic 환경에서 서버 이미지를 다른 계정에 공유하는 방법 " style="width:770px;align:center">

공유된 계정을 삭제하고, [적용] 버튼을 클릭해 변경될 설정을 적용합니다.

<img src="../../images/ncp_server_server_image_share_classic_09.jpg" alt="네이버 클라우드 Classic 환경에서 서버 이미지를 다른 계정에 공유하는 방법 " style="width:770px;align:center">

아까 공유 받았던 계정으로 접속해 [Server Image]에 들어가보면 공유되었던 서버 이미지가 삭제된 것을 확인할 수 있습니다.

<img src="../../images/ncp_server_server_image_share_classic_10.jpg" alt="네이버 클라우드 Classic 환경에서 서버 이미지를 다른 계정에 공유하는 방법 " style="width:770px;align:center">


## 참고 URL
<a href="https://guide.ncloud-docs.com/docs/compute-compute-5-1-v2" target="_blank" style="word-break:break-all;">https://guide.ncloud-docs.com/docs/compute-compute-5-1-v2</a>
