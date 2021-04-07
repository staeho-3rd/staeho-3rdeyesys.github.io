---
date: 2021-04-06
title: PHP로 네이버 클라우드 API를 호출하는 샘플 예제
categories:
  - 8.API
description: PHP로 네이버 클라우드 API를 호출하는 샘플 예제입니다.
type: Document
set: api
order_number: 1
---

## 개요
네이버 클라우드 인프라와 상품 및 솔루션 등의 활용을 도와주는 API를 PHP로 호출하는 샘플 예제를 정리해봅니다.  
네이버 클라우드 API는 RESTful API 방식으로 제공되며, XML와 JSON 형식으로 응답합니다
우선 전체 소스코드를 살펴보고 다음으로 주요 코드를 상세하게 살펴보겠습니다.


## API 호출 샘플 코드

``` php
<?php
	
	// 기본 데이터 설정
	$unixtimestamp =  round(microtime(true) * 1000);

	$ncp_accesskey = "네이버 클라우드 AccessKey";
	$ncp_secretkey = "네이버 클라우드 SecretKey";	

	$api_server = "https://billingapi.apigw.ntruss.com";

	// API URL 예시 : 상품별 가격 리스트 호출 api
	$api_url = "/billing/v1/product/getProductPriceList";
	$api_url = $api_url."?regionCode=KR&productItemKindCode=VSVR";

	$apicall_method = "GET";
	$space = " ";
	$new_line = "\n";

	// hmac으로 암호화할 문자열 설정
	$message = 
		$apicall_method
		.$space
		.$api_url
		.$new_line
		.$unixtimestamp
		.$new_line
		.$ncp_accesskey;	
	
	// hmac_sha256 암호화
	$msg_signature = hash_hmac("sha256", $message, $ncp_secretkey, true));
	$msg_signature = base64_encode($msg_signature);

	// http 호출 헤더값 설정
	$http_header = array();
	$http_header[0] = "x-ncp-apigw-timestamp:".$unixtimestamp."";
	$http_header[1] = "x-ncp-iam-access-key:".$ncp_accesskey."";
	$http_header[2] = "x-ncp-apigw-signature-v2:".$msg_signature."";

	// api 호출
	$ch = curl_init();
	curl_setopt($ch, CURLOPT_URL, $api_server.$api_url);
	curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, FALSE);
	curl_setopt($ch, CURLOPT_RETURNTRANSFER, TRUE);	
	curl_setopt($ch, CURLOPT_HTTPHEADER, $http_header);

	$response = curl_exec($ch);

	curl_close($ch);

?>
```

## 코드 상세 설명
<br />
#### 유닉스 타임 스탬프
``` php
$unixtimestamp =  round(microtime(true) * 1000);
```
네이버 클라우드 API를 호출할 때는 13자리 형식의 유닉스 타임 스탬프가 필요합니다.  
PHP에서 일반적으로 사용하는 time()함수는 10자리 형식이기 때문에 여기서는 microtime()을 사용합니다.  
microtime(true)은 float 형식의 값을 리턴하므로 1000을 곱하고 정수로 반올림합니다.

``` php
$val1 = time();
$val2 = microtime(true);
$val3 = round(microtime(true) * 1000);

/*
$val1 : 1617699570
$val2 : 1617699570.1146
$val3 : 1617699570115
*/
```
<br />
#### 네이버 클라우드 인증키
``` php
$ncp_accesskey = "네이버 클라우드 AccessKey";
$ncp_secretkey = "네이버 클라우드 SecretKey";	
```

네이버 클라우드 인증키는 네이버 클라우드 포탈 -> 마이페이지 -> 계정관리 -> 인증키 관리 - API 인증키 관리 메뉴에서 Access Key ID와 Secret Key를 가져오셔야 하며, 아직 만들어진 Key가 없다면 새로 만드셔야 합니다.

<img src="../../images/ncp_storage_object_storage_api_key_01.jpg" alt="AWS CLI를 이용한 Object Storage 접속 방법" style="width:800px;align:center">

#### hmac으로 암호화할 문자열 설정
``` php
$apicall_method = "GET";
$space = " ";
$new_line = "\n";
```
암호화할 문자열을 설정하는데 필요한 값을 사전에 정의합니다.

``` php
$message = 
	$apicall_method
	.$space
	.$api_url
	.$new_line
	.$unixtimestamp
	.$new_line
	.$ncp_accesskey;	
```
네이버 클라우드 API에서는 호출 방식(GET, POST)과 도메인을 제외한 API URL, 유닉스 타임스탬프, API ACCESSKEY를 공백과 개행문자 \n을 이용해서 하나의 문자열로 설정합니다.  

API URL은 용도별로 각기 다른데, 네이버 클라우드 API 가이드 사이트에서 확인할 수 있습니다.  
<a href="https://api.ncloud-docs.com/docs/ko/home/" target="_blank" style="word-break:break-all;">https://api.ncloud-docs.com/docs/ko/home/</a>


#### hmac_sha256 방식으로 암호화
``` php
$msg_signature = hash_hmac("sha256", $message, $ncp_secretkey, true));
$msg_signature = base64_encode($msg_signature);
```
hmac sha256 방식으로 네이버 클라우드 API SecretKey를 이용하여 $message 문자열을 암호화 즉, 해시값을 만들고, 다시 base64로 인코딩합니다.

#### http 호출 헤더값 설정
``` php
$http_header = array();
$http_header[0] = "x-ncp-apigw-timestamp:".$unixtimestamp."";
$http_header[1] = "x-ncp-iam-access-key:".$ncp_accesskey."";
$http_header[2] = "x-ncp-apigw-signature-v2:".$msg_signature."";
```
API를 호출할 때는 http 헤더값에 다음 3가지를 추가해서 호출합니다.
- 유닉스 타임스탬프
- 네이버 클라우드 API AccessKey
- hmac_256 으로 암호화한 문자열

여기서 전송하는 타임스탬프는 위에서 $message를 암호화할 때 사용한 타임스탬프와 동일한 값이어야 합니다.

#### api 호출
``` php
$ch = curl_init();
curl_setopt($ch, CURLOPT_URL, $api_server.$api_url);
curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, FALSE);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, TRUE);	
curl_setopt($ch, CURLOPT_HTTPHEADER, $http_header);

$response = curl_exec($ch);
```
이제 위에서 준비한 값들을 사용해서 API를 호출합니다.  
리턴값은 호출하는 용도별로 json 또는 xml 형태로 반환됩니다.

##  응답 예시
``` xml
<?xml version="1.0" encoding="UTF-8"?>
<getProductPriceListResponse>
  <requestId>9a6b9f7c-f688-4cec-841f-634d355cef1e</requestId>
  <returnCode>0</returnCode>
  <returnMessage>success</returnMessage>
  <totalRows>2</totalRows>
  <productPriceList>
    <productPrice>
      <productItemKind>
        <code>VSVR</code>
        <codeName>Server (VPC)</codeName>
      </productItemKind>
      <productItemKindDetail>
        <code>BM</code>
        <codeName>BareMetal</codeName>
      </productItemKindDetail>
     ''' 중략 '''
      <softwareType/>
      <productType>
        <code>BM</code>
        <codeName>BareMetal</codeName>
      </productType>
      <productType>
        <code>BM</code>
        <codeName>BareMetal</codeName>
      </productType>
      <productTypeDetail/>
      <gpuCount>0</gpuCount>
      <cpuCount>24</cpuCount>
      <memorySize>137438953472</memorySize>
      <baseBlockStorageSize>4123168604160</baseBlockStorageSize>
      <dbKind/>
      <osInfomation/>
      <platformType/>
      <osType/>
      <platformCategoryCode/>
      <diskType>
        <code>LOCAL</code>
        <codeName>Local storage</codeName>
      </diskType>
      <diskDetailType>
        <code>SSD</code>
        <codeName>SSD</codeName>
      </diskDetailType>
      <generationCode>G1</generationCode>     
    ''' 중략 '''
  </productPriceList>
</getProductPriceListResponse>
```


## 참고 URL
<a href="https://api.ncloud-docs.com/docs/ko/home/" target="_blank" style="word-break:break-all;">https://api.ncloud-docs.com/docs/ko/home/</a>


> "문서 최종 수정일 : 2021-04-07"

