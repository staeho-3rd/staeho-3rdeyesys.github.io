---
date: 2021-01-21
last_modified_at: 2021-06-02
title: Object Storage Lifecycle Management 관리대상 설정 방법
categories:
  - 4.storage
description: 네이버 클라우드 Object Storage Lifecycle Management의 관리대상(Source) Object 접두어 설정 방법
type: Document
set: storage
order_number: 2
---

## 개요
네이버 클라우드 Object Storage에 저장된 Object 즉, 파일들의 Lifecycle(수명주기)를 설정할 때 관리대상이 되는 Object를 결정하는 규칙에 대해 정리해보겠습니다.

## Lifecycle Management (수명주기) 정책 설정
수명주기 정책 설정은 크게 정책, 관리대상, 이동위치 3가지 항목으로 구성됩니다.

### 정책
정책 유형은 다음의 3가지가 있습니다.
- 만료 삭제 : 설정된 기간이 지난 파일을 삭제
- 이관 : 설정된 기간이 지난 파일을 Archive Storage로 이동
- 이관 후 삭제 : 설정된 기간이 지난 파일을 Archive Storage로 이동한 후 Object Storage에서 삭제

그리고 이동 시점은 파일이 Object Storage에 저장-생성된 후 경과한 일자를 기준으로 하며 1일 ~ 3,650일 사이의 값을 입력합니다.

<img src="../../images/ncp_storage_object_storage_lifecycle_01.jpg" alt="네이버 클라우드 Object Storage Lifecycle Management의 관리대상(Source) Object 접두어 설정 방법" style="width:650px;align:center">

### 관리대상 (Source)
관리대상의 버킷(Bucket)을 선택하고 Object 이름의 규칙을 접두어 방식으로 입력합니다.

<img src="../../images/ncp_storage_object_storage_lifecycle_02.jpg" alt="네이버 클라우드 Object Storage Lifecycle Management의 관리대상(Source) Object 접두어 설정 방법" style="width:650px;align:center">

### 이동위치 (Target)
이동할 위치는 Archive Storage로 고정이며, Archive Storage의 컨테이너(버킷)을 선택하고 세부경로 즉, 폴더를 입력합니다.  
세부경로에 아무것도 입력하지 않으면 Source 즉, Object Storage의 위치, 폴더 구조 그대로 이동됩니다.

<img src="../../images/ncp_storage_object_storage_lifecycle_03.jpg" alt="네이버 클라우드 Object Storage Lifecycle Management의 관리대상(Source) Object 접두어 설정 방법" style="width:650px;align:center">


## 관리대상(Source) Object 이름 규칙
- 네이버 클라우드에서 채택하고 있는 규칙은 **접두어 방식**입니다.  
예를 들어 규칙을 **ncp**라고 설정하면 이름이 ncp로 시작되는 모든 파일과 폴더가 대상이 됩니다.  
하지만, 접두어 규칙이기 때문에 img_ncp_01.png 처럼 파일명 중간이나 끝에 **ncp**가  들어간 파일과 폴더는 대상이 아닙니다.

## Object 이름 규칙의 특수 문자 사용
- < > : " \ \| ? * % 는 사용할수 없습니다
- "/"는 첫 글자에 사용할 수 없습니다.
- "//"처럼 "/"는 연속해서 사용할 수 없습니다.

## 적용 예시
아래 스샷처럼 폴더와 파일이 저장되어 있다고 가정하고 예를 들어보겠습니다.  
다른 항목들은 동일하고, 관리대상(Source) Object 이름 규칙에 따라 어떤 결과가 나오지는 확인해보겠습니다.  
물론 아래의 예시들에서 공통적으로 위에서 지정한 수명주기 날짜에 해당하는 파일들만 이동하게 됩니다.

<img src="../../images/ncp_storage_object_storage_lifecycle_04.jpg" alt="네이버 클라우드 Object Storage Lifecycle Management의 관리대상(Source) Object 접두어 설정 방법" style="width:700px;align:center">

``` bash
- 3rdeyesys
	- img_01.png
	- screenshot_01.png
- 3rdeyesys_img
	- img_02.png
- ncp
	- 
- 3rdeyesys_biz.png
- ncp_server_acg_classic.png
- ncp_server_acg_vpc_inbound.png
- vpc_acg_nacl_ncp.png
```

이렇게 3rdeyesys, 3rdeyesys_img 2개의 폴더에는 각각 파일이 존재하고, ncp 폴더에는 아무것도 없습니다.  그리고 4개 파일이 루트에 저장되어 있습니다.

#### Object 이름 규칙(접두어) : 3rdeyesys
이 경우 Archive Storage로 이동하는 파일과 폴더는 다음과 같습니다.  
즉, 3rdeyesys로 시작하는 파일과 폴더 아래에 있는 파일까지 모두 이동하게 됩니다.
``` bash
- 3rdeyesys
	- img_01.png
	- screenshot_01.png
- 3rdeyesys_img
	- img_02.png
- 3rdeyesys_biz.png
```
<br />
#### Object 이름 규칙(접두어) : ncp
이 경우 Archive Storage로 이동하는 파일과 폴더는 다음과 같습니다.  
위의 경우와 다르게 ncp 폴더는 이동하지 않는데 그 이유는 ncp 폴더 아래에 아무 파일도 없기 때문에 이동할 파일이 없어 폴더도 이동하지 않습니다.  

``` bash
- ncp_server_acg_classic.png
- ncp_server_acg_vpc_inbound.png
```
마찬가지로 ncp 폴더 아래에 파일이 존재하더라도 위에서 지정한 수명주기 날짜에 해당하는 파일이 없는 경우에도 ncp 폴더는 이동하지 않습니다.  
또한 접두어 방식이기 때문에 파일명 중간에 ncp가 들어간 vpc_acg_nacl_ncp.png 파일은 해당되지 않아서 이동하지 않습니다.

#### Object 이름 규칙(접두어) : 3rdeyesys/
이렇게 뒤에 "/"를 입력하여 폴더라고 명시한 경우에 Archive Storage로 이동하는 파일과 폴더는 다음과 같습니다.  

``` bash
- 3rdeyesys
	- img_01.png
	- screenshot_01.png
```
즉, 끝에 "/"를 입력했기 때문에 3rdeyesys로 시작하는 폴더만 대상이 되어 다른 파일은 이동하지 않습니다.

#### Object 이름 규칙(접두어) : 3rdeyesys/img
이렇게 폴더와 파일명 접두어까지 함께 입력한 경우 Archive Storage로 이동하는 파일과 폴더는 다음과 같습니다.  

``` bash
- 3rdeyesys
	- img_01.png
```
즉, 3rdeyesys 폴더 아래에 있는 파일들 중에서 img로 시작하는 이름을 가진 파일만 이동하게 됩니다.

#### Object 이름 규칙(접두어) : 아무것도 입력하지 않았을 때
아무것도 입력하지 않았을 때는 모든 파일과 폴더 아래에 있는 파일들이 이동하게 됩니다.  
물론 마찬가지로 수명주기 날짜에 해당하는 파일만 이동하게되고, 폴더 아래에 해당하는 파일이 없을 경우 해동 폴더는 이동하지 않습니다.


## 정책 실행 시간
Lifecycle Management(수명주기) 정책 실행시간은 아래와 같습니다. 

 - 01:00~02:00,  07:00~08:00, 13:00~14:00, 19:00~20:00  
 (※ 파일용량이 클 경우 일부 변동될 수 있음)

예시) 정책 유형(이관), 이동 시점(생성 후 1일)로 정책을 생성하고, 대상 파일이 15시에 업로드 되었다면 다음 날 19~20시 사이에 이관 완료.

## 참고 URL
<a href="https://guide.ncloud-docs.com/docs/storage-storage-6-1" target="_blank" style="word-break:break-all;">https://guide.ncloud-docs.com/docs/storage-storage-6-1.html</a>
