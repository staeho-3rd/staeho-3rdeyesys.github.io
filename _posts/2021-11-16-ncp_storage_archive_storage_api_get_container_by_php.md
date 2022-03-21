---
date: 2021-11-16
last_modified_at: 2021-11-17
title: PHP로 Archive Storage API 호출하기 - 컨테이너(버킷) 오브젝트 목록 조회
categories:
  - 4.storage
description: 네이버 클라우드에서 PHP로 Archive Storage API 호출해서 컨테이너(버킷) 오브젝트 목록 조회하는 방법입니다
type: Document
set: storage
order_number: 12
v2_link: /storage/ncloud_storage_archive_storage_api_get_container_by_php.html
---

## 개요
네이버 클라우드(Ncloud) Archive Storage API를 이용해서 컨테이너(버킷)에 있는 오브젝트 전체 목록을 PHP로 조회하는 방법에 대해 정리해보겠습니다.

## API 정보
- OpenStack Swift API : 2.15.1 (Pike)
- OpenStack Keystone V3 API : v3.8


## 인증 토큰 생성
Archive Storage API를 호출할 때는 먼저 인증 토큰을 생성해야 하는데, 생성 방법은 내용이 다소 긴 관계로 다른 문서에서 자세히 설명해두었습니다. 
아래 문서를 참고 하시기 바랍니다.

<a href="/4.storage/ncp_storage_archive_storage_api_access_token_create/" target="_blank" style="word-break:break-all;">PHP로 Archive Storage API 인증 토큰 생성하는 방법 (docs.3rdeyesys.com)</a>


## 오브젝트 목록 조회 코드
``` php
<?php

  $x_auth_token = "Archive Storage API 인증 토큰";

  $ncloud_project_id = "Archive Storage 프로젝트 ID";
  $ncloud_container = "Archive Storage 컨테이너(버킷) 이름";
  
  $api_server = "https://kr.archive.ncloudstorage.com";
  $api_url = "/v1/AUTH_".$ncloud_project_id."/".$ncloud_container."?format=json";		

  // http 호출 헤더값 설정
  $http_header = array();
  $http_header[0] = "X-Auth-Token: ".$x_auth_token;
  $http_header[1] = "charset=UTF-8";

  // api 호출
  $ch = curl_init();
  curl_setopt($ch, CURLOPT_URL, $api_server.$api_url);
  curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, FALSE);
  curl_setopt($ch, CURLOPT_RETURNTRANSFER, TRUE);	
  curl_setopt($ch, CURLOPT_HTTPHEADER, $http_header);
  curl_setopt($ch, CURLOPT_HEADER, TRUE); //request에 header 값도 수신
  curl_setopt($ch, CURLOPT_POST, FALSE); //GET 방식으로 호출

  $response = curl_exec($ch);
  $http_code = curl_getinfo($ch, CURLINFO_HTTP_CODE);
  curl_close($ch);
  		
  if ($response)
  {
  	if ($http_code == 200)
	{
		// response에서 header 값 분리
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

		// response에서 json형태의 body 값 분리
		$json_response = substr($response, strpos($response, "\r\n\r\n"));
		$rows_response = json_decode($json_response, JSON_OBJECT_AS_ARRAY);

		$object_count = $headers["X-Container-Object-Count"];
		$used_bytes = $headers["X-Container-Bytes-Used"];
		$used_k_bytes = $used_bytes / 1024;
		$used_m_bytes = $used_k_bytes / 1024;
		$used_g_bytes = $used_m_bytes / 1024;
	}
	else if ($http_code == 404)
	{
		echo("존재하지 않는 컨테이너(버킷)입니다");

		$rows_response = [];	

		$object_count = 0;
		$used_bytes = 0;
		$used_k_bytes = 0;
		$used_m_bytes = 0;
		$used_g_bytes = 0;
	}
	else
	{
		echo($response);
	}

  }
  else
  {
  	echo("Error");
  }

?>


<?php
  // 오브젝트 목록 출력
  $cnt = 0;
  foreach ($rows_response as $row)
  {				
  	$archive_object_name = $row["name"];
  	$archive_object_hash = $row["hash"];
  	$archive_object_content_type = $row["content_type"];
  	$archive_object_last_modified = $row["last_modified"];
  	$archive_object_bytes = $row["bytes"];

  	$cnt++;
  ?>
	<tr> 	  						
	  <td><?php echo($cnt);?></td>
	  <td><?php echo($archive_object_name);?></td>
	  <td><?php echo($archive_object_content_type);?></td>
	  <td><?php echo($archive_object_hash);?></td>					  
	  <td><?php echo($archive_object_last_modified);?></td>
	  <td><?php echo(number_format($archive_object_bytes));?></td>
	</tr>

<?php	
  }
?>
```

## 코드 상세 설명
<br />

#### Archive Storage API 이용 정보
``` php
  $ncloud_project_id = "Archive Storage 프로젝트 ID";
  $ncloud_container = "Archive Storage 컨테이너(버킷) 이름";
```
Archive Storage API 이용을 위한 Project ID는 [네이버 클라우드 포탈] - [Archive Storage]에서 [API 이용 정보 확인] 버튼을 클릭하면 확인할 수 있습니다.  
테스트에 사용할 컨테이너(버킷)은 [test]로 설정해두었습니다.

<img src="../../images/ncloud_storage_archive_storage_api_get_container_by_php_04.png" alt="네이버 클라우드에서 PHP로 Archive Storage API 호출하기 - 컨테이너(버킷) 오브젝트 목록 조회하는 방법" style="width:770px;align:center">

[API 이용 정보 확인] 창에서 Project ID를 확인하고, PHP 소스코드에 입력합니다.

<img src="../../images/ncloud_storage_archive_storage_api_access_token_create_02.png" alt="네이버 클라우드 PHP로 Archive Storage API 인증 토큰 생성하는 방법" style="width:500px;align:center">

#### API 서버와 URL 설정
``` php
  $api_server = "https://kr.archive.ncloudstorage.com";
  $api_url = "/v1/AUTH_".$ncloud_project_id."/".$ncloud_container."?format=json";
```
Archive Storage API 서버와 컨테이너(버킷)에 있는 오브젝트 목록을 조회하기 위한 URL 정보는 위와 같습니다. 
[프로젝트 ID]와 [컨테이너(버킷) 이름]을 URL에 포함시키고, 파라미터로는 전달받을 목록의 형태를 설정하게 되는데, 여기서는 json형태로 받겠습니다.


#### Header 값 설정
``` php
  // http 호출 헤더값 설정
  $http_header = array();
  $http_header[0] = "X-Auth-Token: ".$x_auth_token;
  $http_header[1] = "charset=UTF-8";
```
Archive Storage API 인증 토큰은 [X-Auth-Token] 라는 이름으로 header에 담아서 전송합니다.

#### API 호출
``` php
  // api 호출
  $ch = curl_init();
  curl_setopt($ch, CURLOPT_URL, $api_server.$api_url);
  curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, FALSE);
  curl_setopt($ch, CURLOPT_RETURNTRANSFER, TRUE);	
  curl_setopt($ch, CURLOPT_HTTPHEADER, $http_header);
  curl_setopt($ch, CURLOPT_HEADER, TRUE); //response에 header 값도 수신
  curl_setopt($ch, CURLOPT_POST, FALSE); //GET 방식으로 호출
```
이제 위에서 준비한 값들을 사용해서 API를 호출합니다.  
curl_setopt($ch, CURLOPT_HEADER, TRUE); 는 Response에 body 뿐만 아니라 header 값도 수신하기 위해 설정합니다.

#### API 호출 응답 수신
``` php
  $response = curl_exec($ch);
  $http_code = curl_getinfo($ch, CURLINFO_HTTP_CODE);
  curl_close($ch);
```
API를 호출하고 응답을 수신합니다. 성공, 실패 등에 대한 HTTP 상태코드도 확인합니다.

#### Header, Body 값 분리
``` php
  // response에서 header 값 분리
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

  // response에서 json형태의 body 값 분리
  $json_response = substr($response, strpos($response, "\r\n\r\n"));
  $rows_response = json_decode($json_response, JSON_OBJECT_AS_ARRAY);
```
Resoponse에서 Header, Body 값을 따로 분리해서 배열에 저장합니다.

#### 컨테이너(버킷) 기본 정보 확인
``` php
  $object_count = $headers["X-Container-Object-Count"];
  $used_bytes = $headers["X-Container-Bytes-Used"];
  $used_k_bytes = $used_bytes / 1024;
  $used_m_bytes = $used_k_bytes / 1024;
  $used_g_bytes = $used_m_bytes / 1024;
```
분리한 Header 값에서 컨테이너(버킷)의 기본 정보를 확인합니다.  
[전체 오브젝트 개수], [사용 중인 용량]을 확인하고 용량은 Bytes 단위이기에 KB, MB, GB 단위로도 변환합니다.

#### HTTP 상태코드
``` php
  if ($http_code == 200){
    //성공
  }
  else if ($http_code == 404){
    echo("존재하지 않는 컨테이너(버킷)입니다");
  }
```
요청이 성공하게 되면 OK (200), 컨테이너(버킷)이 존재하지 않는 경우는 Not Found (404) 상태 코드를 응답합니다.

#### 오브젝트 목록 html 출력
``` php
<?php
  // 오브젝트 목록 출력
  $cnt = 0;
  foreach ($rows_response as $row)
  {				
  	$archive_object_name = $row["name"];
  	$archive_object_hash = $row["hash"];
  	$archive_object_content_type = $row["content_type"];
  	$archive_object_last_modified = $row["last_modified"];
  	$archive_object_bytes = $row["bytes"];

  	$cnt++;
  ?>
	<tr> 	  						
	  <td><?php echo($cnt);?></td>
	  <td><?php echo($archive_object_name);?></td>
	  <td><?php echo($archive_object_content_type);?></td>
	  <td><?php echo($archive_object_hash);?></td>					  
	  <td><?php echo($archive_object_last_modified);?></td>
	  <td><?php echo(number_format($archive_object_bytes));?></td>
	</tr>

<?php	
  }
?>
```
오브젝트 목록을 html로 출력합니다.  
출력하는 정보는 [이름], [해시값], [Content Type], [최종 수정일], [용량(Bytes)] 입니다.

## 목록 출력 예시
위의 코드를 실행하면 다음과 같이 오브젝트 목록이 출력됩니다.

<img src="../../images/ncloud_storage_archive_storage_api_get_container_by_php_02.png" alt="네이버 클라우드에서 PHP로 Archive Storage API 호출하기 - 컨테이너(버킷) 오브젝트 목록 조회하는 방법" style="width:770px;align:center">

출력된 오브젝트 정보가 올바른지 확인하기 위해 네이버 클라우드(Ncloud) 콘솔에서 해당 컨테이너(버킷)의 정보를 확인하면 다음과 같이 일치하는 것을 알 수 있습니다.

<img src="../../images/ncloud_storage_archive_storage_api_get_container_by_php_01.png" alt="네이버 클라우드에서 PHP로 Archive Storage API 호출하기 - 컨테이너(버킷) 오브젝트 목록 조회하는 방법" style="width:770px;align:center">

## 제한 사항
Archive Storage API를 이용해서 가져올 수 있는 오브젝트 목록의 최대 개수는 10,000개입니다. 
1만개 이상이 등록된 컨테이너(버킷)에서 오브젝트 목록을 요청해도 아래와 같이 최대 10,000개 까지만 가져올 수 있습니다.

<img src="../../images/ncloud_storage_archive_storage_api_get_container_by_php_03.png" alt="네이버 클라우드에서 PHP로 Archive Storage API 호출하기 - 컨테이너(버킷) 오브젝트 목록 조회하는 방법" style="width:770px;align:center">

{: .success }
컨테이너(버킷)에 10,000개 이상의 오브젝트가 저장되어 있을 경우에는 아래쪽에서 소개하는 방법처럼 폴더별로 오브젝트 목록을 따로 조회하시면 됩니다.

## 오브젝트 검색
위에서는 컨테이너(버킷)에 저장된 모든 오브젝트들을 조회하는 기능을 확인해보았습니다.  
다음으로는 특정 이름으로 시작되는 오브젝트나 특정 폴더에 있는 오브젝트만 조회하는 방법에 대해 위에서 확인한 PHP 예제 소스코드에서 변경이 필요한 부분만 확인해보겠습니다.

테스트에 사용된 오브젝트와 폴더 구조는 다음과 같습니다.
``` php
- Test_Folder_0001.png
- Test_Folder
    L Sub_Folder
        L Sub_Sub_Folder
```

<br />
#### 특정 이름으로 시작하는 오브젝트 목록
``` php
  $ncloud_object_prefix = "Test_Folder";

  $api_url = "/v1/AUTH_".$ncloud_project_id."/".$ncloud_container."?format=json&prefix=".$ncloud_object_prefix;		
```
API URL을 호출할 때 파라미터로 prefix를 전송하면 prefix 값에 해당하는 특정 문자열로 시작하는 오브젝트 목록을 모두 가져 옵니다.  
예를 들어 prefix 값을 [Test_Folder]로 설정하면, 아래와 같이 [Test_Folder]라는 이름으로 시작되는 모든 오브젝트 목록을 가져오게 됩니다.

<img src="../../images/ncloud_storage_archive_storage_api_get_container_by_php_05.png" alt="네이버 클라우드에서 PHP로 Archive Storage API 호출하기 - 컨테이너(버킷) 오브젝트 목록 조회하는 방법" style="width:770px;align:center">

#### 특정 폴더 아래에 있는 오브젝트 목록
``` php
  $ncloud_object_prefix = "Test_Folder/";

  $api_url = "/v1/AUTH_".$ncloud_project_id."/".$ncloud_container."?format=json&prefix=".$ncloud_object_prefix;		
```
특정 폴더 아래에 있는 오브젝트 목록을 가져 올 때는 prefix 값 마지막에 슬래시 [ / ]를 추가하면 됩니다.
예를 들어 prefix 값을 [Test_Folder/]로 설정하면, 아래와 같이 [Test_Folder]라는 이름의 폴더 아래에 있는 모든 오브젝트 목록을 가져오게 됩니다.  

위쪽에서 확인한 [특정 이름으로 시작하는 오브젝트 목록]의 결과 예시와는 다르게 폴더 자체인 [Test_Folder] 그리고 [Test_Folder_0001.png]는 포함되어 있지 않습니다.

<img src="../../images/ncloud_storage_archive_storage_api_get_container_by_php_06.png" alt="네이버 클라우드에서 PHP로 Archive Storage API 호출하기 - 컨테이너(버킷) 오브젝트 목록 조회하는 방법" style="width:770px;align:center">


## API 기타 기능
Archive Storage API에서 지원하는 기능은 컨테이너(버킷)의 오브젝트 목록 외에도 아래와 같이 여러가지가 있습니다. 
위에서 사용한 것은 컨테이너(버킷) 오퍼레이션의 GET 기능입니다.

#### 어카운트 오퍼레이션
- GET : 어카운트에 속한 컨테이너(버킷) 목록을 조회합니다.
- HEAD : 어카운트의 메타데이터를 조회합니다.
- POST : 어카운트에 메타데이터를 설정 및 변경합니다.

#### 컨테이너(버킷) 오퍼레이션
- PUT : 컨테이너(버킷)을 생성합니다.
- GET : 컨테이너(버킷)에 속한 오브젝트 목록을 조회합니다.
- HEAD : 컨테이너(버킷)의 메타데이터를 조회합니다.
- POST : 컨테이너(버킷)에 메타데이터를 설정 및 변경합니다.
 DELETE : 빈 컨테이너(버킷)을 삭제합니다.

#### 오브젝트 오퍼레이션
- PUT : 오브젝트를 업로드합니다. 동일한 이름의 오브젝트가 있을 경우 덮어쓰기를 합니다.
- COPY : 다른 위치에 있는 오브젝트를 복제합니다.
- GET : 오브젝트를 다운로드합니다.
- HEAD : 오브젝트의 메타데이터를 조회합니다.
- POST : 오브젝트에 메타데이터를 설정 및 변경합니다.
- DELETE : 오브젝트를 삭제합니다.

## 참고 URL
1.  Archive Storage API 기본 가이드
	- <a href="https://api.ncloud-docs.com/docs/common-archivestorageapi-archivestorageapi" target="_blank" style="word-break:break-all;">https://api.ncloud-docs.com/docs/common-archivestorageapi-archivestorageapi</a>

2.  Archive Storage API 상세 가이드
	- <a href="https://api.ncloud-docs.com/docs/storage-archivestorage" target="_blank" style="word-break:break-all;">https://api.ncloud-docs.com/docs/storage-archivestorage</a>

3.  OpenStack Keystone V3 API 가이드
	- <a href="https://docs.openstack.org/api-ref/identity/v3/" target="_blank" style="word-break:break-all;">https://docs.openstack.org/api-ref/identity/v3/</a>
