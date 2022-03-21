---
date: 2021-04-30
last_modified_at: 2021-04-30
title: PHP로 nShortURL 서비스 이용하기 샘플 예제
categories:
  - 9.application service
description: 네이버 클라우드에서 길고 복잡한 URL을 간단하고 짧게 바꿔주는 API nShortURL을 PHP로 이용하는 샘플 예제입니다.
type: Document
set: api
order_number: 3
v2_link: /application-service/ncloud_application_service_nshorturl_php_sample.html
---

## 개요
SNS를 사용하거나 SMS를 보낼 때 길고 복잡한 URL은 무척 불편하기에 짧게 바꿔주는 서비스들이 인기를 얻었습니다.  
대표적으로 goo.gl, bit.ly 등이 있는데 현재 구글의 goo.gl는 서비스가 종료되었습니다.  
그래서 이를 대신해서 네이버 클라우드에 있는 길고 복잡한 URL을 간단하고 짧게 바꿔주는 API 서비스 nShortURL 서비스를 추천합니다.
nShortURL은 OpenAPI 형태로 제공되는데 여기서는 PHP로 호출하는 방식을 살펴볼텐데, 우선 전체 소스코드를 살펴보고 다음으로 주요 코드를 상세하게 살펴보겠습니다.

## 이용 요금
**nShortURL은 무료로 제공되는 서비스**이며, 일 25,000건, 월 750,000건 내에서 사용 가능하며 상향이 필요한 경우 고객지원으로 문의하면 됩니다.

## API 이용 신청
nShortURL을 이용하기 위해서는 [네이버 클라우드 콘솔] - [AI·NAVER API] -  [Application]에서 등록을 해야 합니다.  
아래와 같이 NAVER 서비스에서 nShortURL을 선택하고, 이름과 사용할 서비스 환경을 입력하고 등록하면 됩니다.  
nShortURL은 웹페이지, 모바일앱 어디서든 사용 가능하며 URL이나 앱 패키지이름, Bundle ID 등을 입력하시면 됩니다.

<img src="/images/ncp_application_service_nshorturl_php_01.jpg" alt="PHP로 nShortURL 서비스 이용하기 샘플 예제" style="width:800px;align:center">


Application 등록을 하고 나면 다음과 같은 화면을 볼 수 있는데 여기서 인증 정보를 확인해야 합니다.

<img src="/images/ncp_application_service_nshorturl_php_02.jpg" alt="PHP로 nShortURL 서비스 이용하기 샘플 예제" style="width:800px;align:center">


## 인증키 확인
nShortURL API를 호출하려면 Client ID와 Client Secret 로 이루어진 Application Key를 사용해야 하는데 아래와 같이 [인증 정보] 버튼을 클릭하면 확인할 수 있습니다.  

<img src="/images/ncp_application_service_nshorturl_php_03.jpg" alt="PHP로 nShortURL 서비스 이용하기 샘플 예제" style="width:800px;align:center">


## API 호출 샘플 코드

``` php
<?php
	
	$long_url = "변환할 URL";	
		
	$client_id = "Client ID";
	$client_secret = "Client Secret";
	$api_url = "https://naveropenapi.apigw.ntruss.com/util/v1/shorturl";
	$enc_url = urlencode($long_url);
	$postvars = "url=".$enc_url;	
	$is_post = true;

	$ch = curl_init();
	curl_setopt($ch, CURLOPT_URL, $api_url);
	curl_setopt($ch, CURLOPT_POST, $is_post);
	curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
	curl_setopt($ch, CURLOPT_POSTFIELDS, $postvars);

	$headers = array();
	$headers[] = "X-NCP-APIGW-API-KEY-ID: ".$client_id;
	$headers[] = "X-NCP-APIGW-API-KEY: ".$client_secret;

	curl_setopt($ch, CURLOPT_HTTPHEADER, $headers);

	$json_response = curl_exec ($ch);

	$status_code = curl_getinfo($ch, CURLINFO_HTTP_CODE);

	curl_close ($ch);

	if($status_code == 200) 
	{
		$rows_response = json_decode($json_response, JSON_OBJECT_AS_ARRAY);
		$rows_result = $rows_response["result"];
		$short_url = $rows_result["url"];
		$hash_val = $rows_result["hash"];
		$org_url = $rows_result["orgUrl"];
	} 
	else 
	{
		$short_url = "Error 내용:".$json_response;
	}

?>
```

## 코드 상세 설명
<br />
#### Application Key
``` php
$client_id = "Client ID";
$client_secret = "Client Secret";
```
네이버 클라우드 콘솔에서 nShortURL 서비스를 등록하고 인증 정보에서 확인한 [Client ID] 와 [Client Secret]를 가져와서 사용하면 됩니다.

#### API URL
``` php
$api_url = "https://naveropenapi.apigw.ntruss.com/util/v1/shorturl";
```

nShortURL의 API URL은 위와 같습니다.

#### 파라미터 설정
``` php
$enc_url = urlencode($long_url);
$postvars = "url=".$enc_url;	
$is_post = true;
```
변환할 URL을 urlencode로 인코딩하고, POST 방식으로 호출하면서 넘겨줄 변수에 할당합니다.


#### http 호출 헤더값 설정
``` php
$headers = array();
$headers[] = "X-NCP-APIGW-API-KEY-ID: ".$client_id;
$headers[] = "X-NCP-APIGW-API-KEY: ".$client_secret;
```
위에서 가져온 Application Key를 호출할 API에 헤더값으로 설정해서 호출하게 됩니다.


#### 결과값 반환
``` php
$rows_response = json_decode($json_response, JSON_OBJECT_AS_ARRAY);
$rows_result = $rows_response["result"];
$short_url = $rows_result["url"];
$hash_val = $rows_result["hash"];
$org_url = $rows_result["orgUrl"];
```
nShortURL에서 json형태로 반환된 값을 배열에 담아 사용하면 됩니다.  
반환되는 값은 짧게 변환된 URL와 해시값, 그리고 원본 URL입니다.


## 사용 한도 및 알람 설정
nShortURL 서비스는 과도한 사용을 방지하기 위해 일별 25,000회, 월별 750,000회의 제한이 있습니다.  
그리고 지정된 한도 내에서 일정한 사용을 초과하면 알람을 받도록 설정할 수 있습니다.  
아래처럼 Application 등록 화면에서 [한도 및 알람 설정] 버튼을 클릭하면 확인할 수 있습니다.

<img src="/images/ncp_application_service_nshorturl_php_04.jpg" alt="PHP로 nShortURL 서비스 이용하기 샘플 예제" style="width:800px;align:center">


## 참고 URL
<a href="https://api.ncloud-docs.com/docs/ai-naver-nshorturl" target="_blank" style="word-break:break-all;">https://api.ncloud-docs.com/docs/ai-naver-nshorturl</a>
