---
date: 2021-03-10
update: 2021-10-08
title: Object Storage 접속용 Windows, MacOS Client Tool - Cyberduck
categories:
  - 4.storage
description: 네이버 클라우드 Object Storage 접속용 Windows, MacOS Client Tool - Cyberduck
type: Document
set: storage
order_number: 6
---

## 개요
네이버 클라우드 Object Storage에 접속해서 파일을 업로드, 다운로드 등의 관리를 할 수 있는 클라이언트 툴중에서 이번에는 Cyberduck이라는 무료 제품을 소개하려고 합니다.  
Object Storage는 AWS S3와 호환되기 때문에 S3를 지원하는 Cyberduck도 사용할 수 있는데 Cyberduck은 S3뿐만 아니라 
FTP, SFTP, WebDAV, Amazon S3, OpenStack Swift, Backblaze B2, Microsoft Azure & OneDrive, Google Drive, Dropbox 등에도 접속 가능합니다.

## 설치파일 다운로드
Cyberduck은 윈도우용과 macOS용 프로그램을 제공하고 있어, 원하는 제품을 다운 받으면 됩니다.

<a href="https://cyberduck.io/download/" target="_blank" style="word-break:break-all;">https://cyberduck.io/download/</a>

<img src="../../images/ncp_storage_object_storage_s3_client_cyberduck_01.jpg" alt="네이버 클라우드 Object Storage 접속용 Windows Client Tool - Cyberduck" style="width:800px;align:center">

## 설치
설치할 디렉토리를 선택하고, Cyberduck을 설치합니다.

<img src="../../images/ncp_storage_object_storage_s3_client_cyberduck_02.jpg" alt="네이버 클라우드 Object Storage 접속용 Windows Client Tool - Cyberduck" style="width:800px;align:center">

## 접속정보 확인
Cyberduck을 실행하고 새 연결을 메뉴를 선택하면 스토리지에 접속 정보를 입력하는 창이 나타납니다.  
여기서 필요한 정보는 서버 접속용 Endpoint URL, API 인증키 (접근 키 ID, Secret Access Key)가 필요한데, 관련된 정보는 아래쪽에서 다시 확인해보겠습니다.

<img src="../../images/ncp_storage_object_storage_s3_client_cyberduck_03.jpg" alt="네이버 클라우드 Object Storage 접속용 Windows Client Tool - Cyberduck" style="width:800px;align:center">


## API 인증키 생성
Cyberduck으로 Object Storage에 접속하기 위해서는 API 인증키가 필요합니다. 
API 인증키는 네이버 클라우드 포탈 -> 마이페이지 -> 계정관리 -> 인증키 관리 - API 인증키 관리 메뉴에서 Access Key ID와 Secret Key를 가져와야 하며, 아직 만들어진 Key가 없다면 새로 만들어야 합니다.

<a href="https://www.ncloud.com/mypage/manage/authkey" target="_blank" style="word-break:break-all;">https://www.ncloud.com/mypage/manage/authkey</a>

<img src="../../images/ncloud_api_auth_key_create.png" alt="AWS CLI를 이용한 Object Storage 접속 방법" style="width:770px;align:center">

## 스토리지 접속
Object Storage에 접속하기 위해 위에서 확인한 API 인증키를 이용하여 다음의 정보를 입력해야 합니다.

``` bash
# 서버: kr.object.ncloudstorage.com
# 접근 키 ID: 네이버 클라우드 Access Key ID
# Secret Access Key: 네이버 클라우드 Secret Key
```
``` bash
# 다른 해외 리전의 Object Storage 서버 주소는 다음과 같습니다.
# 미국: us.objectstorage.ncloud.com
# 싱가포르: sg.objectstorage.ncloud.com
# 일본: jp.objectstorage.ncloud.com
# 독일: de.objectstorage.ncloud.com
```

<img src="../../images/ncp_storage_object_storage_s3_client_cyberduck_04.jpg" alt="네이버 클라우드 Object Storage 접속용 Windows Client Tool - Cyberduck" style="width:800px;align:center">

Object Storage에 접속하면 이미 생성된 Bucket이 있을 경우 아래와 같이 Bucket 리스트가 나타납니다.

<img src="../../images/ncp_storage_object_storage_s3_client_cyberduck_05.jpg" alt="네이버 클라우드 Object Storage 접속용 Windows Client Tool - Cyberduck" style="width:800px;align:center">

## 업로드
Object Storage에 파일을 업로드 하기 위해서는 먼저 Bucket이 생성되어 있어야 합니다.  

#### Bucket (버킷) 생성
이미 Bucket을 만들었다면 그대로 사용하시면 되고, 새로 만드실 경우에는 [파일]-[새 폴더] 메뉴를 이용하시면 됩니다.

<img src="../../images/ncp_storage_object_storage_s3_client_cyberduck_06.jpg" alt="네이버 클라우드 Object Storage 접속용 Windows Client Tool - Cyberduck" style="width:800px;align:center">
<img src="../../images/ncp_storage_object_storage_s3_client_cyberduck_07.jpg" alt="네이버 클라우드 Object Storage 접속용 Windows Client Tool - Cyberduck" style="width:800px;align:center">
<img src="../../images/ncp_storage_object_storage_s3_client_cyberduck_08.jpg" alt="네이버 클라우드 Object Storage 접속용 Windows Client Tool - Cyberduck" style="width:800px;align:center">

#### 파일 업로드
업로드할 대상 Bucket을 선택하고 마우스 오른쪽 버튼을 클릭하면 업로드 메뉴를 확인할 수 있습니다.

<img src="../../images/ncp_storage_object_storage_s3_client_cyberduck_09.jpg" alt="네이버 클라우드 Object Storage 접속용 Windows Client Tool - Cyberduck" style="width:800px;align:center">

파일 선택창에서 원하는 파일을 선택하면 되고, 파일이 여러개일 경우 다중 선택도 가능합니다.

<img src="../../images/ncp_storage_object_storage_s3_client_cyberduck_10.jpg" alt="네이버 클라우드 Object Storage 접속용 Windows Client Tool - Cyberduck" style="width:800px;align:center">

전송결과 화면
<img src="../../images/ncp_storage_object_storage_s3_client_cyberduck_11.jpg" alt="네이버 클라우드 Object Storage 접속용 Windows Client Tool - Cyberduck" style="width:800px;align:center">

업로드 완료 화면
<img src="../../images/ncp_storage_object_storage_s3_client_cyberduck_12.jpg" alt="네이버 클라우드 Object Storage 접속용 Windows Client Tool - Cyberduck" style="width:800px;align:center">

## 권한설정
Object Storage에 업로드한 파일을 외부에서 접근해야 하는 경우에는 파일에 대한 권한을 변경해야 합니다.  

권한을 변경할 파일을 선택하고 마우스 오른쪽 버튼을 클릭하면 [정보] 메뉴를 확인할 수 있습니다.

<img src="../../images/ncp_storage_object_storage_s3_client_cyberduck_13.jpg" alt="네이버 클라우드 Object Storage 접속용 Windows Client Tool - Cyberduck" style="width:800px;align:center">

파일정보 팝업창에서 [권한] 메뉴를 선택하시면 왼쪽 아래에서 권한 설정 기능에서 [모두]를 선택합니다.

<img src="../../images/ncp_storage_object_storage_s3_client_cyberduck_14.jpg" alt="네이버 클라우드 Object Storage 접속용 Windows Client Tool - Cyberduck" style="width:600px;align:center">

[모두]에 대한 권한을 READ로 선택합니다.

<img src="../../images/ncp_storage_object_storage_s3_client_cyberduck_15.jpg" alt="네이버 클라우드 Object Storage 접속용 Windows Client Tool - Cyberduck" style="width:600px;align:center">

여러 파일의 권한을 동시에 변경할 경우 해당 파일들을 전부 선택하고 권한을 변경할 수도 있습니다.

<img src="../../images/ncp_storage_object_storage_s3_client_cyberduck_16.jpg" alt="네이버 클라우드 Object Storage 접속용 Windows Client Tool - Cyberduck" style="width:800px;align:center">

## 다운로드
Object Storage에 저장된 파일을 로컬로 다운로드 받을 경우에는 대상 파일을 선택하고 마우스 오른쪽 버튼을 클릭하여 [지정된 위치로 내려받기] 메뉴를 선택합니다.

<img src="../../images/ncp_storage_object_storage_s3_client_cyberduck_17.jpg" alt="네이버 클라우드 Object Storage 접속용 Windows Client Tool - Cyberduck" style="width:800px;align:center">

다운로드 받을 폴더를 선택합니다.
<img src="../../images/ncp_storage_object_storage_s3_client_cyberduck_18.jpg" alt="네이버 클라우드 Object Storage 접속용 Windows Client Tool - Cyberduck" style="width:800px;align:center">

다운로드가 완료되었습니다.
<img src="../../images/ncp_storage_object_storage_s3_client_cyberduck_19.jpg" alt="네이버 클라우드 Object Storage 접속용 Windows Client Tool - Cyberduck" style="width:800px;align:center">

## 동기화
이번에는 업로드, 다운로드가 아닌, 로컬 폴더와 Object Storage에 있는 Bucket을 서로 동기화 하는 기능에 대해 확인해보겠습니다.

동기화할 Bucket을 선택하고 마우스 오른쪽 버튼을 클릭해 동기화 메뉴를 선택합니다.

<img src="../../images/ncp_storage_object_storage_s3_client_cyberduck_20.jpg" alt="네이버 클라우드 Object Storage 접속용 Windows Client Tool - Cyberduck" style="width:800px;align:center">

다음으로 동기화할 로컬 폴더를 선택합니다.

<img src="../../images/ncp_storage_object_storage_s3_client_cyberduck_21.jpg" alt="네이버 클라우드 Object Storage 접속용 Windows Client Tool - Cyberduck" style="width:800px;align:center">

이제 동기화를 시작할 준비가 되었습니다. 동기화 창에서 [계속] 버튼을 클릭해 동기화를 시작합니다.

<img src="../../images/ncp_storage_object_storage_s3_client_cyberduck_22.jpg" alt="네이버 클라우드 Object Storage 접속용 Windows Client Tool - Cyberduck" style="width:800px;align:center">

동기화가 완료되었지만 화면에서는 즉시 반영이 되지 않습니다. Bucket을 선택하고 마우스 오른쪽 버튼을 선택해 [다시보기] 메뉴를 선택하면 새로 고침이 되면서 동기화된 파일을 확인할 수 있습니다.

<img src="../../images/ncp_storage_object_storage_s3_client_cyberduck_23.jpg" alt="네이버 클라우드 Object Storage 접속용 Windows Client Tool - Cyberduck" style="width:800px;align:center">

## 삭제
Bucket이나 파일을 삭제할 경우에는 대상 파일 등을 선택하고 마우스 오른쪽 버튼을 클릭해 [삭제] 메뉴를 선택합니다.

<img src="../../images/ncp_storage_object_storage_s3_client_cyberduck_24.jpg" alt="네이버 클라우드 Object Storage 접속용 Windows Client Tool - Cyberduck" style="width:800px;align:center">

삭제 기능은 한번 더 정말 삭제할 것인지 확인하는 단계가 있습니다.

<img src="../../images/ncp_storage_object_storage_s3_client_cyberduck_25.jpg" alt="네이버 클라우드 Object Storage 접속용 Windows Client Tool - Cyberduck" style="width:800px;align:center">

## 환경설정
Cyberduck의 환경설정에서 중요한 것들을 살펴 보겠습니다.  

위쪽 메뉴에서 [편집]-[환경설정]을 선택합니다.

<img src="../../images/ncp_storage_object_storage_s3_client_cyberduck_26.jpg" alt="네이버 클라우드 Object Storage 접속용 Windows Client Tool - Cyberduck" style="width:800px;align:center">

환경설정 중에서 우선 [전송]-[일반]에 들어가시면 다운로드와 업로드에 대한 설정을 할 수 있습니다.  
여기서는 기본 다운로드 폴더를 설정할 수 있고, 업로드할 때 파일명이 겹칠 경우 덮어쓸 것인지, 물어보기 할 것인지 설정할 수 있습니다.

<img src="../../images/ncp_storage_object_storage_s3_client_cyberduck_27.jpg" alt="네이버 클라우드 Object Storage 접속용 Windows Client Tool - Cyberduck" style="width:800px;align:center">

[전송]-[권한] 설정에서는 업로드 되는 파일의 기본 권한을 원하는 설정으로 변경할 수 있습니다.

<img src="../../images/ncp_storage_object_storage_s3_client_cyberduck_28.jpg" alt="네이버 클라우드 Object Storage 접속용 Windows Client Tool - Cyberduck" style="width:800px;align:center">

[전송]-[필터] 설정에서는 다운로드와 업로드 할 때 특정 형식이나 확장자의 파일을 건너띄기 할 수 있는 정규식 기반의 설정을 제공합니다.

<img src="../../images/ncp_storage_object_storage_s3_client_cyberduck_29.jpg" alt="네이버 클라우드 Object Storage 접속용 Windows Client Tool - Cyberduck" style="width:800px;align:center">

[대역폭] 설정에서는 업로드와 다운로드할 때의 네트워크의 대역폭을 설정할 수 있습니다.

<img src="../../images/ncp_storage_object_storage_s3_client_cyberduck_30.jpg" alt="네이버 클라우드 Object Storage 접속용 Windows Client Tool - Cyberduck" style="width:800px;align:center">


## 평가
마지막으로 Cyberduck 클라이언트 툴의 장점과 단점을 정리해보겠습니다.

#### 장점
- 무료제품으로 상업적인 용도로도 사용 가능하다.
- S3와 그 호환 스토리지 뿐만 아니라 FTP, Dropbox, Google Drive 등 다양한 방식의 접속을 지원한다.
- 메뉴가 한글화 되어 있다.

#### 단점
- 인터페이스가 탐색기 형식이 아니라서 업로드, 다운로드할 때 불편하다.
- 제공되는 접속용 프로필이 다양하지 않아서 서버 정보를 일일이 입력해야 한다.
- 스토리지 <--> 스토리지 방식의 파일 전송을 지원하지 않는다.


> 문서 최종 수정일 : 2021-03-11