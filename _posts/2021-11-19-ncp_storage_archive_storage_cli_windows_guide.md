---
date: 2021-11-19
last_modified_at: 2021-11-19
title: Archive Storage CLI 사용 가이드 - Windows 환경
categories:
  - 4.storage
description: 네이버 클라우드 Archive Storage CLI를 Windows 환경에서 사용하는 방법에 대한 가이드입니다.
type: Document
set: storage
order_number: 13
v2_link: /storage/ncloud_storage_archive_storage_cli_windows_guide.html
---

## 개요
네이버 클라우드(Ncloud) Archive Storage CLI를 Windows 환경에서 사용하는 방법에 대해 정리해보겠습니다.

## CLI 정보
Archive Storage가 OepnStack으로 구성되어 있고, Client는 Python 기반의 Client를 사용하게 됩니다.

- python-keystoneclient : 3.17.0
- python-swiftclient : 3.6.0


## Python 다운로드
먼저 Python을 다운로드 합니다. 권장하는 버전은 3.6 이상입니다. 여기서는 3.9를 설치하겠습니다.

<a href="https://www.python.org/downloads/" target="_blank" style="word-break:break-all;">https://www.python.org/downloads/</a>

<img src="../../images/python_install_01.png" alt="python 설치 화면" style="width:770px;align:center">

## Python 설치

#### PATH 추가
Python 설치 시작화면에 PATH에 python을 추가하는 옵션이 있습니다.  
"**Add Python 3.9 to PATH**" 옵션을 선택하고 설치를 시작하면 됩니다.

<img src="../../images/python_install_02.png" alt="python 설치 화면" style="width:660px;align:center">

#### PATH 문자 길이 제한 해제
Windows에는 기본설정에 파일경로가 최대 260자로 제한되어 있는데, 이 제한을 풀것인지 확인하는 과정입니다.  
"**Disable path length limit**" 옵션이 나오는데, 특별한 문제가 없다면 해제하고 가면 됩니다.

<img src="../../images/python_install_03.png" alt="python 설치 화면" style="width:660px;align:center">


## CLI Client 설치

#### python-keystoneclient : 3.17.0 설치
우선 OepnStack 서비스의 인증을 담당하는  KeyStone Client를 설치합니다.

``` bash
pip install python-keystoneclient==3.17.0
```
<img src="../../images/ncloud_storage_archive_storage_cli_windows_guide_01.png" alt="네이버 클라우드 Archive Storage CLI를 Windows 환경에서 사용하는 방법에 대한 가이드" style="width:770px;align:center">

#### python-swiftclient : 3.6.0 설치
다음으로 실제 명령을 수행하는 Swift Client를 설치합니다.

``` bash
pip install python-swiftclient==3.6.0
```

<img src="../../images/ncloud_storage_archive_storage_cli_windows_guide_02.png" alt="네이버 클라우드 Archive Storage CLI를 Windows 환경에서 사용하는 방법에 대한 가이드" style="width:770px;align:center">


## Client 설치 오류
Python Clinet를 설치하는 도중에 아래와 같은 메시지가 나타나면서 오류가 발생하는 경우가 있습니다. 
Microsoft Visual C++ Build Tools 등이 설치되어 있지 않아서 인데 설치하는 방법 2가지 중에서 하나를 선택해서 설치하시면 됩니다.

``` bash
error: Microsoft Visual C++ 14.0 is required. Get it with "Microsoft Visual C++ Build Tools" : https://visualstudio.microsoft.com/downloads/
```

``` bash
fatal error C1083: 포함 파일을 열 수 없습니다. 'basetsd.h' : No such file or directory
error: command 'C:\\Program Files (x86)\\Microsoft Visual Studio\\....... .....\\cl.exe' failed with exit code 2
```
<img src="../../images/ncloud_storage_archive_storage_cli_windows_guide_21.png" alt="네이버 클라우드 Archive Storage CLI를 Windows 환경에서 사용하는 방법에 대한 가이드" style="width:770px;align:center">

#### 방법 1 : Build Tools 직접 설치
첫번째 방법은 Build Tools를 따로 설치하는 방법입니다. 
아래 링크에서 [Microsoft Build Tools 2015 업데이트3]를 다운로드 받아서 설치하시면 되는데, [Visual Studio]가 설치되어 있는 경우에는 설치가 실패하기도 합니다.  
이때는 아래에 나오는 두번째 방법으로 설치하시면 됩니다.

<a href="https://visualstudio.microsoft.com/ko/vs/older-downloads/" target="_blank" style="word-break:break-all;">https://visualstudio.microsoft.com/ko/vs/older-downloads/</a>

<img src="../../images/ncloud_storage_archive_storage_cli_windows_guide_03.png" alt="네이버 클라우드 Archive Storage CLI를 Windows 환경에서 사용하는 방법에 대한 가이드" style="width:770px;align:center">

#### 방법 2 : Visual Studio Installer 설치
Visual Studio가 설치되어 있는 경우에는 위 1번 방법으로 설치가 되지 않는 경우가 있으므로 Visual Studio Installer에서 설치하도록 하겠습니다.  
[Visual Studio Installer]를 실행하셔서 설치된 Visual Studio 메뉴의 [수정] 버튼을 클릭합니다.

<img src="../../images/ncloud_storage_archive_storage_cli_windows_guide_04.png" alt="네이버 클라우드 Archive Storage CLI를 Windows 환경에서 사용하는 방법에 대한 가이드" style="width:770px;align:center">

나타난 화면에서 [C++를 사용한 데스크톱 개발]을 선택하시고 오른쪽 [설치 세부정보]에서  다음 2가지를 선택해서 설치하시면 됩니다. 
- MSVC vXXX - VS 20XX C++ x64/x86 빌드 도구
- Windows 10 SDK

<img src="../../images/ncloud_storage_archive_storage_cli_windows_guide_05.png" alt="네이버 클라우드 Archive Storage CLI를 Windows 환경에서 사용하는 방법에 대한 가이드" style="width:770px;align:center">

## 인증 토큰 생성
CLI Client가 모두 설치되었으면 이제 접속을 위한 인증 토큰을 생성해야 합니다. 
인증 토큰을 생성하는 명령어는 다음과 같은데 여기에 필요한 값이 4가지 있습니다.

``` bash
swift --os-auth-url https://kr.archive.ncloudstorage.com:5000/v3 --auth-version 3 --os-username {access_key_id} --os-password {secret_key} --os-user-domain-id {domain_id} --os-project-id {project_id} auth
```
<br />
#### 네이버 클라우드 API 인증키 : access_key_id, secret_key
두가지 Key는 네이버 클라우드 API 인증키로 [네이버 클라우드 포탈] -> [마이페이지] -> [계정관리] -> [인증키 관리] - [API 인증키 관리] 메뉴에서 Access Key ID와 Secret Key를 가져오셔야 하며, 아직 만들어진 Key가 없다면 새로 만드셔야 합니다.

<img src="../../images/ncloud_api_auth_key_create.png" alt="네이버 클라우드 API 인증키 생성하기" style="width:770px;align:center">

#### Archive Storage API 정보:  domain_id, project_id
두가지 id 값은 Archive Storage API 이용을 위한 Domain ID와 Project ID로 [네이버 클라우드 포탈] - [Archive Storage]에서 [API 이용 정보 확인] 버튼을 클릭하면 확인할 수 있습니다.

<img src="../../images/ncloud_storage_archive_storage_api_access_token_create_01.png" alt="네이버 클라우드 PHP로 Archive Storage API 인증 토큰 생성하는 방법" style="width:770px;align:center">

[API 이용 정보 확인] 창에서 Domain ID와 Project ID를 확인하고, 인증토큰 생성 코드에 입력합니다.

<img src="../../images/ncloud_storage_archive_storage_api_access_token_create_02.png" alt="네이버 클라우드 PHP로 Archive Storage API 인증 토큰 생성하는 방법" style="width:500px;align:center">

위에서 확인한 설정 값 4가지를 추가해서 인증토큰 생성 명령을 실행하면 아래와 같이 생성된 인증토큰이 출력됩니다.
``` bash
export OS_STORAGE_URL=https://kr.archive.ncloudstorage.com/v1/AUTH_{project_id}
export OS_AUTH_TOKEN={인증 토큰}
```

<img src="../../images/ncloud_storage_archive_storage_cli_windows_guide_06.png" alt="네이버 클라우드 Archive Storage CLI를 Windows 환경에서 사용하는 방법에 대한 가이드" style="width:770px;align:center">

## 인증 토큰 유효 시간
API 인증 토큰의 유효 시간은 24시간이고 삭제 요청을 호출하면 삭제할 수 있습니다.

## 환경 변수 설정
위에서 생성된 인증토큰과 URL을 환경 변수에 설정합니다. Windows에서는 export 명령을 set로 변경해서 실행합니다.
``` bash
set OS_STORAGE_URL=https://kr.archive.ncloudstorage.com/v1/AUTH_{project_id}
set OS_AUTH_TOKEN={인증 토큰}
```

<img src="../../images/ncloud_storage_archive_storage_cli_windows_guide_07.png" alt="네이버 클라우드 Archive Storage CLI를 Windows 환경에서 사용하는 방법에 대한 가이드" style="width:770px;align:center">


## 컨테이너(버킷) 조회
현재 Archive Storage에 생성되어 있는 컨테이너(버킷)을 확인할 수 있는 명령어는 다음과 같습니다.
``` bash
swift list
```
<img src="../../images/ncloud_storage_archive_storage_cli_windows_guide_08.png" alt="네이버 클라우드 Archive Storage CLI를 Windows 환경에서 사용하는 방법에 대한 가이드" style="width:770px;align:center">

#### 컨테이너(버킷)의 모든 오브젝트 조회
특정 컨테이너(버킷)의 모든 오브젝트 목록을 확인하는 명령어는 마지막에 컨테이너(버킷) 이름을 적어주면 됩니다.
``` bash
swift list {컨테이너(버킷) 이름}
```
<img src="../../images/ncloud_storage_archive_storage_cli_windows_guide_09.png" alt="네이버 클라우드 Archive Storage CLI를 Windows 환경에서 사용하는 방법에 대한 가이드" style="width:770px;align:center">


## 파일 업로드
파일 업로드 명령은 특정 파일을 업로드 하는 명령과 폴더를 통째로 업로드 하는 명령을 각각 확인해보겠습니다.

#### 폴더 업로드
폴더를 통째로 업로드 하는 명령은 다음과 같습니다.
``` bash
swift upload {컨테이너(버킷) 이름} --object-name {저장할 Archive Storage 폴더명} {로컬PC 폴더명}
```

폴더 파일을 업로드 후에 list 명령어로 컨테이너(버킷)의 오브젝트 목록을 확인할 수 있습니다.

<img src="../../images/ncloud_storage_archive_storage_cli_windows_guide_10.png" alt="네이버 클라우드 Archive Storage CLI를 Windows 환경에서 사용하는 방법에 대한 가이드" style="width:770px;align:center">

#### 개별 파일 업로드 
특정 파일을 업로드 하려면 다음처럼 명령을 실행하면 됩니다.
``` bash
swift upload {컨테이너(버킷) 이름} --object-name {저장할 Archive Storage 폴더명/저장할 파일명} {로컬PC 파일 경로}
```

마찬가지로 파일을 업로드 후에 list 명령어로 확인해보시면 됩니다.

<img src="../../images/ncloud_storage_archive_storage_cli_windows_guide_11.png" alt="네이버 클라우드 Archive Storage CLI를 Windows 환경에서 사용하는 방법에 대한 가이드" style="width:770px;align:center">

## 파일 삭제
파일 삭제도 개별 파일 삭제와 폴더 삭제 2가지로 나누어서 확인해보겠습니다.

#### 개별 파일 삭제
특정 파일을 삭제하는 명령은 다음과 같습니다.
``` bash
swift delete {컨테이너(버킷) 이름} {Archive Storage 파일 전체 경로}
```
<img src="../../images/ncloud_storage_archive_storage_cli_windows_guide_12.png" alt="네이버 클라우드 Archive Storage CLI를 Windows 환경에서 사용하는 방법에 대한 가이드" style="width:770px;align:center">

#### 폴더 삭제
폴더 전체를 삭제하는 명령은 prefix 옵션이 들어갑니다.
``` bash
swift delete {컨테이너(버킷) 이름} --prefix {Archive Storage 폴더 경로}
```
<img src="../../images/ncloud_storage_archive_storage_cli_windows_guide_13.png" alt="네이버 클라우드 Archive Storage CLI를 Windows 환경에서 사용하는 방법에 대한 가이드" style="width:770px;align:center">


## 파일 다운로드
파일 다운로드는 개별 파일 다운로드와 폴더 다운로드 그리고, 컨테이너(버킷) 파일 전체 다운로드를 실행해 보고, 로컬PC에서 다운로드 된 것을 확인해보겠습니다.

#### 개별 파일 다운로드
개별 파일 다운로드에는 output 옵션이 필요합니다.
``` bash
swift download {컨테이너(버킷) 이름} --output {저장할 로컬PC 파일 전체 경로} {Archive Storage 파일 전체 경로}
```
<img src="../../images/ncloud_storage_archive_storage_cli_windows_guide_14.png" alt="네이버 클라우드 Archive Storage CLI를 Windows 환경에서 사용하는 방법에 대한 가이드" style="width:770px;align:center">

로컬PC에서 확인을 해보면 아래와 같이 다운로드된 파일을 확인할 수 있습니다.

<img src="../../images/ncloud_storage_archive_storage_cli_windows_guide_15.png" alt="네이버 클라우드 Archive Storage CLI를 Windows 환경에서 사용하는 방법에 대한 가이드" style="width:770px;align:center">

#### 폴더 다운로드
폴더 다운로드에는 output-dir 옵션이 필요합니다.
``` bash
swift download {컨테이너(버킷) 이름} --output-dir {저장할 로컬PC 폴더 경로} {Archive Storage 폴더 경로}
```

<img src="../../images/ncloud_storage_archive_storage_cli_windows_guide_16.png" alt="네이버 클라우드 Archive Storage CLI를 Windows 환경에서 사용하는 방법에 대한 가이드" style="width:770px;align:center">

로컬PC에서 확인을 해보면 아래와 같이 폴더가 다운로드 된 것을 확인할 수 있습니다.

<img src="../../images/ncloud_storage_archive_storage_cli_windows_guide_17.png" alt="네이버 클라우드 Archive Storage CLI를 Windows 환경에서 사용하는 방법에 대한 가이드" style="width:770px;align:center">

#### 컨테이너(버킷) 전체 다운로드
컨테이너(버킷)에 있는 모든 파일을 다운로드 할 때는 아래와 같이 폴더 다운로드 명령에서 마지막에 있는 파일명이나 폴더명 파라미터를 지우시면 됩니다.
``` bash
swift download {컨테이너(버킷) 이름} --output-dir {저장할 로컬PC 폴더 경로}
```
<img src="../../images/ncloud_storage_archive_storage_cli_windows_guide_18.png" alt="네이버 클라우드 Archive Storage CLI를 Windows 환경에서 사용하는 방법에 대한 가이드" style="width:770px;align:center">

로컬PC에서 확인을 해보면 폴더와 파일이 모두 다운로드 된 것을 확인할 수 있습니다.

<img src="../../images/ncloud_storage_archive_storage_cli_windows_guide_19.png" alt="네이버 클라우드 Archive Storage CLI를 Windows 환경에서 사용하는 방법에 대한 가이드" style="width:770px;align:center">


## 주의 사항

#### Archive Storage CLI를 사용해야 하는 이유
마지막으로 Archive Storage를 관리할 때는 AWS S3용 Client Tool (ex, CloudBerry Explorer, S3 Browser) 대신에 Archive Storage CLI를 사용해야 하는 이유에 대해 정리해보겠습니다.  

네이버 클라우드(Ncloud) Archive Storage는 Object Storage의 데이터를 장기 백업하기 위한 용도 등으로 주로 사용되다 보니 Object Storage와 비슷한 시스템이라고 오해하는 경우가 많습니다. 
하지만, Object Storage가 AWS S3와 호환되는 시스템 구조로 되어 있는 것에 반해, Archive Storage는 OpenStack 기반의 시스템 구조로 되어 있어 전혀 다르다고 보시면 됩니다.  

{: error.box }
그러다 보니, Object Storage를 관리하는데 자주 사용되는 AWS S3용 Client Tool (ex, CloudBerry Explorer, S3 Browser) 등을 Archive Storage를 관리할 때도 사용하는 경우가 있는데, 가급적 사용하지 않는 것이 좋습니다. 
왜냐하면 **AWS S3용 Client Tool로 Archive Storage에서 업로드, 다운로드, 삭제, 이름변경 등의 작업을 진행하면 해당 파일에 문제가 생기거나 때로는 컨테이너(버킷) 데이터 전체에 문제가 생길 수도 있기 때문**입니다.  

혹시나 AWS S3용 Client Tool을 사용하더라도 파일(오브젝트)을 조회하는 용도 정도로만 한정해서 사용하는 것을 추천합니다. 물론 파일 조회도 가능하면 Archive Storage용의 CLI나 API를 이용하는 것이 좋습니다.


## 참고 URL
1.  Archive Storage CLI 가이드
	- <a href="https://cli.ncloud-docs.com/docs/guide-archivestorage" target="_blank" style="word-break:break-all;">https://cli.ncloud-docs.com/docs/guide-archivestorage</a>

2.  OpenStack CLI 가이드
	- <a href="https://docs.openstack.org/ocata/cli-reference/swift.html" target="_blank" style="word-break:break-all;">https://docs.openstack.org/ocata/cli-reference/swift.html</a>
