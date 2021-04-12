---
date: 2020-12-22
title: 서버 접속 가이드(Linux) - 공인IP 있을 때
categories:
  - 1.compute
description: 네이버 클라우드 서버 접속 가이드 - 공인IP 있을 때
type: Document
set: server
order_number: 10
---


## 개요
네이버 클라우드에서 리눅스 서버에 접속하는 방법 중에서 공인아이피가 있을 때 접속하는 방법에 대한 내용을 정리하였습니다.
여기서는 서버에 접속하는 방법을 정리하기 때문에 이미 서버는 생성되어 있다는 전제하게 정리하게 됩니다.

## 요약
우선은 전체 과정을 텍스트로 간단하게 요약해서 살펴보고 스크린샷을 포함한 상세 과정은 아래쪽에서 살펴보겠습니다.

1. 포트 포워딩 설정 해제 (설정되어 있을 경우)
2. 관리자(root) 비밀번호 확인
3. 터미널 프로그램(Putty) 실행
4. 공인IP로 접속
5. 위 2번 관리자 비밀번호 확인에서 기록한 비번 입력



## 포트 포워딩 설정 해제 (설정되어 있을 경우)

네이버 클라우드에서는 Server, Bare Metal Server에서 공인 IP와 포트 포워딩을 동시에 사용하면 22(Linux), 3389(Windows) 포트가 포트 포워딩에 먼저 할당되므로 공인 IP에서 해당 포트 사용이 불가해집니다.  
즉, **포트 포워딩이 설정된 상태에서는 Putty로 접속할 때 공인IP로는 22, 3389 포트가 접속 되지 않는다**는 뜻입니다.  
그러므로, 공인 IP로 22, 3389 포트에 접속하는 경우에는 포트 포워딩을 해제하시고,  
포트 포워딩을 설정하지 않았다면 다음 순서인 관리자(root) 비밀번호 확인으로 바로 이동하시면 되겠습니다.
<img src="../../images/ncp_server_connect_14.jpg" alt="포트 포워딩 설정" style="width:800px;align:center">

포트 포워딩을 해제 하시려면 아래 화면처럼 설정된 포트 포워딩에서 **삭제** 버튼을 클릭하시고 하단의 **적용** 버튼을 클릭합니다.
<img src="../../images/ncp_server_connect_15.jpg" alt="포트 포워딩 설정" style="width:800px;align:center">


## 관리자 비밀번호 확인

네이버 클라우드에서 처음 서버를 생성하고 접속하게 되면 root와 비밀번호로 접속하게 됩니다.
물론 매번 비번을 입력하고 접속하는 것은 번거롭기도 하고 보안 측면에서 좋지 않기 때문에 이후에 SSH Key를 생성해서 접속하는 방식으로 바꾸는 것이 좋습니다.
<img src="../../images/ncp_server_connect_06.jpg" alt="관리자 비밀번호 확인" style="width:800px;align:center">


관리자 비밀번호 확인 메뉴를 클릭하면 인증키를 확인하는 화면이 나옵니다.
인증키는 서버 생성시에 설정하고 PC에 다운로드 해 둔 확장지 *.pem 파일입니다. 
<img src="../../images/ncp_server_connect_07.jpg" alt="관리자 비밀번호 확인" style="width:800px;align:center">


인증키 파일을 마우스로 끌어오거나 화면을 클릭해서 선택합니다.
<img src="../../images/ncp_server_connect_08.jpg" alt="관리자 비밀번호 확인" style="width:800px;align:center">


인증키가 일치하면 아래와 같이 root 계정에 해당하는 비밀번호가 나타납니다.
이 비밀번호를 복사하여 저장해둡니다.
혹시 복사-저장하는 것을 잊으셨어도 언제든지 관리자 비밀번호 확인 메뉴에 들어가서 인증키를 인증하면 다시 확인할 수 있으니 걱정하지 않으셔도 됩니다.
<img src="../../images/ncp_server_connect_09.jpg" alt="관리자 비밀번호 확인" style="width:800px;align:center">


## 터미널 프로그램(Putty) 실행

Putty를 실행해서 공인IP를 입력합니다
<img src="../../images/ncp_server_connect_16.jpg" alt="터미널 프로그램(Putty) 실행" style="width:800px;align:center">


접속을 하면 rsa2 key fingerprint 를 Putty 캐시에 저장할 것인지 묻는 화면이 나타납니다. 
보통 예(Y)를 선택하면 됩니다.
<img src="../../images/ncp_server_connect_17.jpg" alt="터미널 프로그램(Putty) 실행" style="width:800px;align:center">


## root 계정과 관리자 비밀번호 입력

계정 root와 관리자 비밀번호 확인에서 저장해 둔 비밀번호를 입력합니다.
<img src="../../images/ncp_server_connect_18.jpg" alt="터미널 프로그램(Putty) 실행" style="width:800px;align:center">


접속이 완료되면 이렇게 화면이 나타납니다.
<img src="../../images/ncp_server_connect_19.jpg" alt="터미널 프로그램(Putty) 실행" style="width:800px;align:center">




## 참고 URL
<a href="https://guide.ncloud-docs.com/docs/compute-compute-3-1-v2" target="_blank" style="word-break:break-all;">https://guide.ncloud-docs.com/docs/compute-compute-3-1-v2.html</a>


> 문서 최종 수정일 : 2020-12-22