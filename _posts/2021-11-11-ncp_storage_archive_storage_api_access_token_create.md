---
date: 2021-11-11
last_modified_at: 2021-11-11
title: PHP로 Archive Storage API 인증 토큰 생성하는 방법
categories:
  - 4.storage
description: 네이버 클라우드 PHP로 Archive Storage API 인증 토큰 생성하는 방법입니다
type: Document
set: storage
order_number: 11
v2_link: /storage/ncloud_storage_archive_storage_api_access_token_create.html
---

## 개요
네이버 클라우드(Ncloud) Archive Storage API를 이용하려고 할 때 먼저 인증 토큰을 생성하고 생성된 토큰을 이용해서 API로 Archive Storage에 접근해야 합니다. 
여기서는 PHP로 API 인증 토큰을 생성하는 방법에 대해 우선 전체 소스코드를 살펴보고 다음으로 주요 코드를 상세하게 살펴보겠습니다.

## API 정보
- OpenStack Swift API : 2.15.1 (Pike)
- OpenStack Keystone V3 API : v3.8

## 인증 토큰 생성 코드
``` php
<?php

  // 전송해야 할 설정값
  $ncloud_accesskey = "네이버 클라우드 AccessKey";
  $ncloud_secretkey = "네이버 클라우드 SecretKey";
  $ncloud_domain_id = "Archive Storage 도메인 ID";
  $ncloud_project_id = "Archive Storage 프로젝트 ID";

  $api_server = "https://kr.archive.ncloudstorage.com:5000";
  $api_url = "/v3/auth/tokens";		 

  // http 호출 헤더값 설정
  $http_header = array();
  $http_header[0] = "Content-Type: application/json";

  // 전송할 값들을 배열 형태로 저장한다
  $postvars = [
  	"auth"=> [
  		"identity"=> [
  			"methods"=> [
  				"password"
  			],
  			"password"=> [
  				"user"=> [
  					"name"=> $ncloud_accesskey,
  					"password"=> $ncloud_secretkey,
  					"domain"=> [
  						"id"=> $ncloud_domain_id
  					]
  				]
  			]
  		],
  		"scope"=> [
  			"project"=> [
  				"id"=> $ncloud_project_id
  			]
  		]
  	]
  ];

  // 배열 형태로 저장한 값들을 json 형태로 변환해서 전송
  $json_portvars = json_encode($postvars);

  // api 호출
  $ch = curl_init();
  curl_setopt($ch, CURLOPT_URL, $api_server.$api_url);
  curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, FALSE);
  curl_setopt($ch, CURLOPT_RETURNTRANSFER, TRUE);	
  curl_setopt($ch, CURLOPT_POST, TRUE); //POST 방식으로 호출
  curl_setopt($ch, CURLOPT_HTTPHEADER, $http_header);
  curl_setopt($ch, CURLOPT_HEADER, TRUE); //response에 header 값도 수신
  curl_setopt($ch,CURLOPT_POSTFIELDS, $json_portvars);

  $response = curl_exec($ch);

  curl_close($ch);

  if ($response)
  {
  	// X-Subject-Token 토큰 값은 request body가 아닌 header로 전달되므로
  	// header를 분리해서 배열에 저장한다 
  	$headers = array();
  	$header_text = substr($response, 0, strpos($response, "\r\n\r\n"));

  	foreach (explode("\r\n", $header_text) as $i => $line) 
  	{
  		if ($i === 0)
  		{
  		   $headers["http_code"] = $line;
  		}
  		else
  		{
  		   list ($key, $value) = explode(": ", $line);
  		   $headers[$key] = $value;
  		}
  	}
	
	// 인증 토큰 확인
  	$x_auth_token = $headers["X-Subject-Token"]; 
  	echo($x_auth_token);

  	//var_dump($headers);
  	//echo("<hr>");
  	//var_dump($response);
  } 
  else 
  {
  	echo "Curl error: " . curl_error($ch);
  }
?>
```

## 코드 상세 설명
<br />
#### 네이버 클라우드 인증키
``` php
  $ncloud_accesskey = "네이버 클라우드 AccessKey";
  $ncloud_secretkey = "네이버 클라우드 SecretKey";
```
네이버 클라우드 인증키는 [네이버 클라우드 포탈] -> [마이페이지] -> [계정관리] -> [인증키 관리] - [API 인증키 관리] 메뉴에서 Access Key ID와 Secret Key를 가져오셔야 하며, 아직 만들어진 Key가 없다면 새로 만드셔야 합니다.

<img src="../../images/ncloud_api_auth_key_create.png" alt="네이버 클라우드 API 인증키 생성하기" style="width:770px;align:center">

#### Archive Storage API 이용 정보
``` php
  $ncloud_domain_id = "Archive Storage 도메인 ID";
  $ncloud_project_id = "Archive Storage 프로젝트 ID";
```
Archive Storage API 이용을 위한 Domain ID와 Project ID는 [네이버 클라우드 포탈] - [Archive Storage]에서 [API 이용 정보 확인] 버튼을 클릭하면 확인할 수 있습니다.

<img src="../../images/ncloud_storage_archive_storage_api_access_token_create_01.png" alt="네이버 클라우드 PHP로 Archive Storage API 인증 토큰 생성하는 방법" style="width:770px;align:center">

[API 이용 정보 확인] 창에서 Domain ID와 Project ID를 확인하고, PHP 소스코드에 입력합니다.

<img src="../../images/ncloud_storage_archive_storage_api_access_token_create_02.png" alt="네이버 클라우드 PHP로 Archive Storage API 인증 토큰 생성하는 방법" style="width:500px;align:center">

#### API 서버와 URL 설정
``` php
  $api_server = "https://kr.archive.ncloudstorage.com:5000";
  $api_url = "/v3/auth/tokens";
```
Archive Storage API 서버와 토큰 생성을 위한 URL 정보는 위와 같습니다.

#### HTTP 호출 header 값 설정
``` php
  $http_header = array();
  $http_header[0] = "Content-Type: application/json";
```
HTTP header에는 json 형태로 호출한다는 것을 설정합니다.

#### 전송할 값 설정
``` php
  // 전송할 값들을 배열 형태로 저장
  $postvars = [
  	"auth"=> [
  		"identity"=> [
  			"methods"=> [
  				"password"
  			],
  			"password"=> [
  				"user"=> [
  					"name"=> $ncloud_accesskey,
  					"password"=> $ncloud_secretkey,
  					"domain"=> [
  						"id"=> $ncloud_domain_id
  					]
  				]
  			]
  		],
  		"scope"=> [
  			"project"=> [
  				"id"=> $ncloud_project_id
  			]
  		]
  	]
  ];

  // 배열 형태로 저장한 값들을 json 형태로 변환해서 전송
  $json_portvars = json_encode($postvars);
```
네이버 클라우드 AccessKey, SecretKey, Archive Storage 도메인 ID, 프로젝트 ID를 전송하기 위해 지정된 형태의 배열로 저장한 후에 json 형태로 변환합니다. 
물론 처음부터 json 형태로 저장해도 됩니다.

#### API 호출
``` php
  $ch = curl_init();
  curl_setopt($ch, CURLOPT_URL, $api_server.$api_url);
  curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, FALSE);
  curl_setopt($ch, CURLOPT_RETURNTRANSFER, TRUE);	
  curl_setopt($ch, CURLOPT_POST, TRUE); //POST 방식으로 호출
  curl_setopt($ch, CURLOPT_HTTPHEADER, $http_header);
  curl_setopt($ch, CURLOPT_HEADER, TRUE); //response에 header 값도 수신
  curl_setopt($ch,CURLOPT_POSTFIELDS, $json_portvars);

  $response = curl_exec($ch);
  curl_close($ch);
```
이제 위에서 준비한 값들을 사용해서 API를 호출합니다.  
curl_setopt($ch, CURLOPT_HEADER, TRUE); 는 Response에 body 뿐만 아니라 header 값도 수신하기 위해 설정합니다.

#### API 인증 토큰 분리
``` php
  if ($response)
  {
  	$headers = array();
  	$header_text = substr($response, 0, strpos($response, "\r\n\r\n"));

  	foreach (explode("\r\n", $header_text) as $i => $line) 
  	{
  		if ($i === 0)
  		{
  		   $headers["http_code"] = $line;
  		}
  		else
  		{
  		   list ($key, $value) = explode(": ", $line);
  		   $headers[$key] = $value;
  		}
  	}
	
	// 인증 토큰 확인
  	$x_auth_token = $headers["X-Subject-Token"]; 
  	echo($x_auth_token);

  	//var_dump($headers);
  	//echo("<hr>");
  	//var_dump($response);
  } 
```
API 인증 토큰값은 X-Subject-Token이라는 이름으로 request body가 아닌 header로 전달되므로 header를 분리해서 배열에 저장합니다.
실제 전송되는 header 값은 아래와 같은 형태입니다.
``` bash
HTTP/1.1 201 Created 
Date: Thu, 11 Nov 2021 07:59:32 GMT 
Server: Apache/2.4.6 (CentOS) OpenSSL/1.0.2k-fips mod_wsgi/3.4 Python/2.7.5 
X-Subject-Token: gAAAAABhjM1lbeTW3Vq......중간 생략 ......txWYsWGrC1siPt8CE0rs_KgNMTQ 
Vary: X-Auth-Token 
x-openstack-request-id: req-1ce......중간 생략 ......a85eb5b 
Content-Length: 1762 
Content-Type: application/json
```

## 인증 토큰 유효 시간
API 인증 토큰의 유효 시간은 24시간이고 삭제 요청을 호출하면 삭제할 수 있습니다.

## 참고 URL
1.  Archive Storage API 기본 가이드
	- <a href="https://api.ncloud-docs.com/docs/common-archivestorageapi-archivestorageapi" target="_blank" style="word-break:break-all;">https://api.ncloud-docs.com/docs/common-archivestorageapi-archivestorageapi</a>

2.  OpenStack Keystone V3 API 가이드
	- <a href="https://docs.openstack.org/api-ref/identity/v3/" target="_blank" style="word-break:break-all;">https://docs.openstack.org/api-ref/identity/v3/</a>
