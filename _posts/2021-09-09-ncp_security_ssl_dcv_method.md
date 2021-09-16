---
date: 2021-09-09
title: SSL인증서 DCV (Domain Control Validation) 인증 방법과 유의사항 정리
categories:
  - 3.security
description: 네이버 클라우드 SSL인증서 DCV (Domain Control Validation) 인증 방법과 유의사항 정리
type: Document
set: security
order_number: 3
---

## 개요
HTTPS 접속을 위한 SSL 인증서를 발급 받기 위해서는 DCV (Domain Control Validation)를 인증받아야 하는데, 이때 필요한 인증방법과 유의 사항을 정리해보겠습니다.

## DCV 인증 방법
DCV 인증방법은 3가지가 있습니다.

1. Email 인증
2. DNS 인증
3. http 인증



## Email 인증
Email 인증은 도메인 등록정보에서 확인되는 이메일과 추가로 5개의 임의로 지정된 메일주소로 인증메일을 발송합니다.

#### 도메인 등록 정보 이메일
도메인 등록 기관에서 도메인 소유자 정보를 노출하지 않는 블라인드 서비스를 이용하고 있을 경우 인증메일을 받을 수 없습니다.
Email 인증을 하기 전에 블라인드 서비스를 해제하고 인증을 요청하셔야 합니다.

#### 추가 5개의 임의로 지정된 이메일
임의로 지정된 이메일 주소는 admin, administrator, hostmaster, postmaster, webmaster 등 5가지이며 추가/수정이 불가능합니다.

> 위의 총 6개 메일 주소 중에서 적어 1개는 유효한 메일주소여야 이메일 인증을 문제없이 완료할 수 있습니다.


#### 이메일 인증 유의사항
- 해외 발신 이메일이 차단되도록 설정되어 있지 않은지 확인이 필요합니다.
- 메일함 용량 부족으로 반송되지 않도록 확인이 필요합니다.
- 자체 메일 서버일 경우 메일서버가 장애가 생기지 않도록 확인이 필요합니다.
- 스팸 차단 서비스나 장비에서 차단이 될 수도 있으므로 확인이 필요합니다.

참고 : <a href="https://www.sslcert.co.kr/products/domain-control-validation#email" target="_blank" style="word-break:break-all;">https://www.sslcert.co.kr/products/domain-control-validation#email</a>


## DNS 인증
DNS 인증은 다음과 같은 순서로 진행하면 됩니다.

1. 인증서 신청 사이트에서 CNAME 처리를 위한 DNS 인증용 Host, Record 용 값을 확인합니다.
2. DNS에 위에서 확인한 값으로 CNAME Record를 등록합니다.  
3. 인증서 신청 사이트에서 DNS 인증 요청 버튼을 클릭합니다.

``` bash
# 예시
# {Host값}.test.co.kr CNAME {Record Value}
_PVG823NLK4DFSVFSANLK.test.co.kr CNAME 089DFCHKJFDSUIFDSLKJ38NF.ssltest.com
```
<br />

#### DNS 인증 유의사항
- Host 값 첫번째 문자열이 _(언더바)입니다. CNAME 등록시에 빠뜨리지 않도록 주의해야 합니다.
- DNS서버에서 TTL 값을 너무 길게 설정하면 그 시간 만큼 인증을 받을 수 없으므로 주의해야 합니다.

참고 : <a href="https://www.sslcert.co.kr/products/domain-control-validation#dns" target="_blank" style="word-break:break-all;">https://www.sslcert.co.kr/products/domain-control-validation#dns</a>


## http 인증
http인증은 http 인증용 코드를 다운로드 받아 해당 도메인 웹사이트에 등록하는 방법입니다.

1. 인증서 신청 사이트에서 http 인증용 코드 파일을 다운로드 받습니다.
2. 해당 도메인 사이트에  /.well-known/pki-validation/ 경로를 생성합니다.
3. 다운로드 받은 인증 코드 파일을 위 경로에 업로드 합니다.
4. 인증서 신청 사이트에서 http 인증 요청 버튼을 클릭합니다.

해당 도메인이 test.co.kr 일 경우 인증 코드 파일의 경로는 다음과 같은 형식이어야 합니다.
``` html
   http://test.co.kr/.well-known/pki-validation/[파일명].txt
```
<br />

#### http 인증 유의사항
- 인증 코드 파일은 수정하면 안됩니다.
- 인증 코드 파일 포맷은 ANSI 포맷이어야 합니다.
- SSL 발급 신청시 도메인에 www. 입력하였더라도, DCV 인증시에는 www를 제외하고 검사가 진행됩니다.  
  만약  현재 사이트가 www. 로만 접속가능한 상태라면, DCV 인증을 위해 임시라도 www. 제외된 접속이 가능하게 만들어 놓아야 합니다.

참고 : <a href="https://www.sslcert.co.kr/products/domain-control-validation#http" target="_blank" style="word-break:break-all;">https://www.sslcert.co.kr/products/domain-control-validation#http</a>


#### 로드밸런서(Load Balancer)를 사용하면서 http 인증할 때 사전작업
http 인증을 하려고 할 때 로드밸런서를 사용하게 될 경우 로드밸런서의 도메인을 DNS에서 CNAME 처리를 먼저 진행하고, DNS 전파가 완료된 것을 확인한 후에 http 인증을 요청해야 합니다.

예를 들어 네이버 클라우드에서는 다음과 같이 처리하면 됩니다.

``` bash
# DNS CNAME 처리 예제
www.test.co.kr  CNAME  slb-****.ncloudslb.com
```


## 참고 URL
1. 인증서 발급 사이트 : SecureSign
	- <a href="https://www.sslcert.co.kr/" target="_blank" style="word-break:break-all;">https://www.sslcert.co.kr/</a>

2. DCV 인증 절차 안내
	- <a href="https://www.sslcert.co.kr/products/domain-control-validation" target="_blank" style="word-break:break-all;">https://www.sslcert.co.kr/products/domain-control-validation</a>


> 문서 최종 수정일 : 2021-09-10
