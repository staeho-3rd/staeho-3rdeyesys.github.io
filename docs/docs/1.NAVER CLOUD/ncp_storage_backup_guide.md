# 백업 서비스 가이드와 신청 절차

## 개요
백업 솔루션을 사용해 서버의 데이터 즉, 소스 등의 파일과 데이터베이스 데이터를 정기적으로 백업하고 보관하는 서비스입니다.

## 이용 방법
네이버 클라우드의 백업 서비스는 이용자가 직접 모든 과정을 마칠 수 없고, 고객센터에 백업신청서를 제출해야 백업이 진행됩니다.

## 지원 OS와 DB 
- Centos : 6.3, 6.6, 7.2, 7.3
- ubuntu : 16.04, 18.04
- Windows : 2012 R2, 2016
- mssql : 2008std, 2012std, 2014std, 2016std, 2016exp
- mysql : 5.7.17, 5.6.34
- 모두 64bit만 지원


## 백업 방식

### 파일백업
- 일회성
- 1일 1회 전체 백업
- 1주 ~ 4주에 1회 전체 백업
- 1주 1회 전체 백업 및 1일 1회 증분 백업

### DB 백업
- mssql 1일 1회 전체 백업
- mssql 1주 1회 전체 백업
- mssql 1주 1회 전체 백업 및 1일 1회 증분 백업
- mysql 1일 1회 전체 백업
- mysql 1주 1회 전체 백업

!!! attention "증분 백업(변경된 데이터만 백업)의 경우 DBMS는 정합성을 보장하나 파일에 대해서는 정합성을 보장하지 않음"

## 백업 신청 절차
1. 네이버 클라우드 플랫폼 포털(https://www.ncloud.com)에 접속하여 로그인
2. 고객지원 > 자료를 클릭하여 **네이버 클라우드 플랫폼 백업 서비스 신청서**와 **백업 Agent**를 다운로드
3. 다운로드한 신청서 양식의 내용에 예를 참고하여 알맞게 기입.
4. 다운로드한 백업 Agent는 OS에 맞는 버전을 VM에 복사하여 설치
5. 백업 Agent가 설치 완료되었다면 네이버 클라우드 플랫폼 포털 내 백업 상품 소개 페이지의 "이용 문의하기"를 클릭
6. 문의하기 페이지에서 제목을 "**백업서비스 신청**"으로 기입하고 작성한 백업 서비스 신청서를 첨부하면 백업 신청이 완료


## 백업 Agent 설치 방법

### Windows
1. 원격 데스크톱을 이용하여 VM 서버에 원격 접속
2. 다운로드한 백업 Agent 설치 파일(NCP_Backup_Windows.zip)을 VM 내 {==C:\Temp==} 하위에 복사하여 압축을 풀면 NCP 이름으로 총 5개의 파일이 생성
3. NCP_Backup_Install.bat을 실행하면 자동으로 설치 및 구성이 진행되며 설치 완료
4. 이후 백업 Agent 관련 파일들은 자동으로 삭제됨

!!! tip "백업 Agent설치 후 파일이 자동 삭제되기에 반드시 설치위치는 C:\Temp에서 수행을 권고"


### Linux
1. Winscp 등을 이용하여 VM 서버의 {==/tmp==} 하위로 백업 Agent 프로그램(NCP_Backup_Linux.tar.gz)을 복사
2. 원격 접속 프로그램(ex.putty)을 이용하여 VM 서버에 원격 접속
3. 백업 Agent 프로그램을 저장한 /tmp 폴더로 이동 후 **tar xvfz NCP_Backup_Linux.tar.gz**을 실행하여 압축 해제
4. NCP_Backup_Install.sh 파일을 실행하면 자동으로 백업 Agent 설치 및 구성이 완료
5. 이후 관련 파일은 자동으로 삭제됨

!!! tip "백업 Agent설치 후 파일이 자동 삭제되기에 반드시 설치위치는/tmp에서 수행을 권고"


## 백업 Agent 정보

### 백업 S/W
- Quest Netvault(구 Dell Netvault)

### 백업 S/W 통신 정보
- TCP/UDP 20031~21631

### 백업 프로그램 설치 위치
- Linux: /usr/Netvault
- Windows: C:\Program Files (x86)\Quest Software\NetVault Backup


## Secure Zone에서 백업하기
- Secure Zone에서 사용 가능한 Agent가 별도로 존재하지 않기 때문에, 프록시 구성을 하던가 해서 Agent를 설치, 실행해야 함.


## 참고 URL
<a href="https://docs.ncloud.com/ko/storage/storage-5-1.html" target="_blank">https://docs.ncloud.com/ko/storage/storage-5-1.html</a>


!!! info "문서 최종 수정일 : 2020-11-25" 