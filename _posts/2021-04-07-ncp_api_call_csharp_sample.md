---
date: 2021-04-07
title: C#으로 네이버 클라우드 API를 호출하는 샘플 예제
categories:
  - 8.API
description: C#으로 네이버 클라우드 API를 호출하는 샘플 예제입니다.
type: Document
set: api
order_number: 2
---

## 개요
네이버 클라우드 인프라와 상품 및 솔루션 등의 활용을 도와주는 API를 C#으로 호출하는 샘플 예제중에서 핵심인 인증을 위한 암호화 문자열 생성 코드를 정리해보겠습니다.


## 암호화 샘플 코드

``` c#
public string unixTimeStamp;
public string ncpAccessKey = "네이버 클라우드 AccessKey";
public string ncpSecretKey = "네이버 클라우드 SecretKey";	
public string apiCallMethod = "GET";
public string apiServer = "https://billingapi.apigw.ntruss.com";
public string apiUrl = "네이버 클라우드 API URL";

public string MakeSignature()
{
	string msgSignature;
	string space = " ";
	string newLine = "\n";
	
	// 13자리 유닉스 타임스탬프 설정
	DateTime now = DateTime.Now;
	DateTime unixOriginalTime = new DateTime(1970, 1, 1, 9, 0, 0);
	double totalMilliSeconds = now.Subtract(unixOriginalTime).TotalMilliseconds;
	unixTimeStamp = Math.Round(totalMilliSeconds).ToString();

	// hmac으로 암호화할 문자열 설정
	string message = new StringBuilder()
		.Append(apiCallMethod)
		.Append(space)
		.Append(apiUrl)
		.Append(newLine)
		.Append(unixTimeStamp)
		.Append(newLine)
		.Append(ncpAccessKey)
		.ToString();

	// hmac_sha256 암호화
	byte[] hmac_key = Encoding.UTF8.GetBytes(ncpSecretKey);
	byte[] bytes = Encoding.UTF8.GetBytes(message);
	using (HMACSHA256 sha256 = new HMACSHA256(hmac_key))
	{
		byte[] hash = sha256.ComputeHash(bytes);
		msgSignature = Convert.ToBase64String(hash);
	}
	return msgSignature;
}
```

## 코드 상세 설명
<br />
#### 네이버 클라우드 인증키
``` c#
public string ncpAccessKey = "네이버 클라우드 AccessKey";
public string ncpSecretKey = "네이버 클라우드 SecretKey";	
```

네이버 클라우드 인증키는 네이버 클라우드 포탈 -> 마이페이지 -> 계정관리 -> 인증키 관리 - API 인증키 관리 메뉴에서 Access Key ID와 Secret Key를 가져오셔야 하며, 아직 만들어진 Key가 없다면 새로 만드셔야 합니다.

<img src="../../images/ncloud_api_auth_key_create.png" alt="AWS CLI를 이용한 Object Storage 접속 방법" style="width:770px;align:center">

<br />
#### 유닉스 타임스탬프
``` c#
DateTime now = DateTime.Now;
DateTime unixOriginalTime = new DateTime(1970, 1, 1, 9, 0, 0);
double totalMilliSeconds = now.Subtract(unixOriginalTime).TotalMilliseconds;
unixTimeStamp = Math.Round(totalMilliSeconds).ToString();
```
네이버 클라우드 API를 호출할 때는 13자리 형식의 유닉스 타임 스탬프가 필요하므로 TotalMilliseconds 값을 가져와서 소수점 아래는 반올림해서 정수값만 취합니다.  
여기서 전달하는 타임스탬프값이 네이버 클라우드 API Gateway의 시간과 5분 이상 차이가 나면 인증 실패가 됩니다.  
그런데 API Gateway는 UTC 기준으로 설정되어 있기 때문에 C#으로 만든 애플리케이션을 로컬PC등의 UTC+9 시간으로 설정된 곳에서 1970년 1월 1일 00시 기준으로 계산하면 9시간의 시간차가 발생해 인증이 실패하게 됩니다.  
정리하면 다음과 같습니다.
- API Gateway 타임스탬프 : UTC 현재시간 - 1970년 1월 1일 00시
- 로컬PC 타임스탬프 : UTC+9 현재시간 - 1970년 1월 1일 09시

즉, **로컬PC 등은 API Gateway보다 9시간 빠르기 때문에 동일한 타임스탬프 값을 얻으려면 1970년 1월 1일 09시 기준으로 계산해야 한다**는 것입니다.  
그래서 위의 코드에서 DateTime unixOriginalTime = new DateTime(1970, 1, 1, 9, 0, 0); 이렇게 적용했습니다.

#### hmac으로 암호화할 문자열 설정
``` c#
public string apiCallMethod = "GET";

string space = " ";
string newLine = "\n";
```
암호화할 문자열을 설정하는데 필요한 값을 사전에 정의합니다.

``` c#
string message = new StringBuilder()
	.Append(apiCallMethod)
	.Append(space)
	.Append(apiUrl)
	.Append(newLine)
	.Append(unixTimeStamp)
	.Append(newLine)
	.Append(ncpAccessKey)
	.ToString();
```
네이버 클라우드 API에서는 호출 방식(GET, POST)과 도메인을 제외한 API URL, 유닉스 타임스탬프, API ACCESSKEY를 공백과 개행문자 \n을 이용해서 하나의 문자열로 설정합니다.  

API URL은 용도별로 각기 다른데, 네이버 클라우드 API 가이드 사이트에서 확인할 수 있습니다.  
<a href="https://api.ncloud-docs.com/docs/ko/home/" target="_blank" style="word-break:break-all;">https://api.ncloud-docs.com/docs/ko/home/</a>


#### hmac_sha256 방식으로 암호화
``` c#
byte[] hmac_key = Encoding.UTF8.GetBytes(ncpSecretKey);
byte[] bytes = Encoding.UTF8.GetBytes(message);
using (HMACSHA256 sha256 = new HMACSHA256(hmac_key))
{
	byte[] hash = sha256.ComputeHash(bytes);
	msgSignature = Convert.ToBase64String(hash);
}
```
hmac sha256 방식으로 네이버 클라우드 API SecretKey를 이용하여 message 문자열을 암호화 즉, 해시값을 만들고, 다시 base64로 인코딩합니다.


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

