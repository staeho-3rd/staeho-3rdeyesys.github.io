---
date: 2021-01-14
title: Classic Load Balancer 운영을 위한 ACG 설정 방법
categories:
  - 2.networking
description: 네이버 클라우드 Classic Load Balancer 운영을 위한 ACG 설정 방법
type: Document
set: networking
order_number: 5
---

## 개요
Load Balancer가 정상적으로 동작하기 위해서는 Load Balancer --> Server의 지정된 포트로 접근할 수 있어야 합니다.  
그러기 위해서는 Server의 ACG에 Load Balancer가 접근할 수 있도록 권한을 설정해주어야 하는데, 여기서는 네이버 클라우드의 Classic Load Balancer를 운영할 때 ACG 설정을 어떻게 하는가에 대한 내용을 정리해보겠습니다.

## 서버연결 실패 상황
서버 등록하고 Load Balancer의 다른 설정들을 모두 올바르게 했는데도 아래와 같이 서버연결 상태가 실패로 나타나고 Load Balancer는 동작이 정지되는 경우가 있습니다.  
이런 경우가 바로 ACG에 Load Balancer의 접근 권한을 설정하지 않았을 때 입니다.

<img src="../../images/ncp_networking_load_balancer_acg_01.jpg" alt="Load Balancer 운영을 위한 ACG 설정" style="width:800px;align:center">

## ACG 권한 설정
ACG에 Load Balancer가 접근할 수 있도록 권한을 설정하려면 접근소스에 아래와 같이 입력합니다.
``` bash
접근소스 : ncloud-load-balancer
```

그 외에 프로토콜과 허용포트도 지정된 정보를 입력하고 추가를 하면 됩니다.

<img src="../../images/ncp_networking_load_balancer_acg_02.jpg" alt="Load Balancer 운영을 위한 ACG 설정" style="width:800px;align:center">
<img src="../../images/ncp_networking_load_balancer_acg_03.jpg" alt="Load Balancer 운영을 위한 ACG 설정" style="width:800px;align:center">


## 서버연결 성공
ACG에 접근권한을 설정하고 나서 잠시 기다리면 Load Balancer가 서버에 접근시도를 하고 정상 접근이 되면서 서버연결 상태가 성공으로 바뀌고 Load Balancer도 정상 작동하게 됩니다.

<img src="../../images/ncp_networking_load_balancer_acg_04.jpg" alt="Load Balancer 운영을 위한 ACG 설정" style="width:800px;align:center">




## 참고 URL
<a href="https://docs.ncloud.com/ko/networking/loadbalancer/classiclb_console.html" target="_blank" style="word-break:break-all;">https://docs.ncloud.com/ko/networking/loadbalancer/classiclb_console.html</a>


> 문서 최종 수정일 : 2021-01-14
