---
date: 2021-05-14
title: Classic LoadBalancer 설정할 때 프로토콜 선택 기준
categories:
  - 2.networking
description: 네이버 클라우드에서 Classic LoadBalancer를 설정할 때 프로토콜을 선택하는 기준에 대한 설명입니다.
type: Document
set: networking
order_number: 7
---

## 개요
네이버 클라우드에서 Classic LoadBalancer(클래식 로드밸런서)를 설정할 때 프로토콜은 언듯 간단해보이지만, 상황에 따라 달라지는 몇가지 중요한 기준이 있어서 내용을 정리해보겠습니다.


## 프로토콜 선택 기준
로드밸런서에 연결된 서버가 어떤 통신을 하는 애플리케이션이냐에 따라서,  
즉 IIS,  Apache 등의 http/https 통신을 하는 웹 애플리케이션(웹서버)이냐 소켓통신을 하는 애플리케이션이냐에 따라 달라지며 
SSL 인증서를 사용하는 경우 인증서를 어디에 설치하느냐에 따라서도 프로토콜이 달라집니다.


#### 서버가 IIS, Apache 등 웹 애플리케이션인 경우

``` bash
- SSL 인증서를 로드밸런서에 설치했을 경우 : HTTP/HTTS 선택
```
``` bash
- SSL 인증서를 서버에 설치했을 경우 : TCP 선택
```

<br />
#### 서버가 소켓 통신을 하는 경우

``` bash
- 소켓 통신 : TCP 선택
```


## 주의사항

아래와 같이 로드밸런서와 서버 양쪽에 SSL 인증서 설치해서 HTTPS 구성을 했을 경우에는 올바르게 동작하지 않습니다. 

- 로드밸런서에 HTTPS 구성 --> 서버에 HTTPS 구성 (**문제 발생**)


## 참고 URL
<a href="https://guide.ncloud-docs.com/docs/networking-loadbalancer-classiclbconsole" target="_blank" style="word-break:break-all;">https://guide.ncloud-docs.com/docs/networking-loadbalancer-classiclbconsole.html</a>


> 문서 최종 수정일 : 2021-05-14
