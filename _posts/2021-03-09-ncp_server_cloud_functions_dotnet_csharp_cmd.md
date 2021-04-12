---
date: 2021-03-09
title: Cloud Functions Action을 .Net (C#)을 사용하여 윈도우 명령프롬프트(cmd)에서 만드는 방법 
categories:
  - 1.compute
description: 네이버 클라우드 Cloud Functions Action을 .Net (C#)을 사용하여 윈도우 명령프롬프트(cmd)에서 만드는 방법 
type: Document
set: server
order_number: 13
---

## 개요
네이버 클라우드 Cloud Functions에서 Action을 만들 수 있는 언어로는 Node.js, Python, Java, Swift, PHP, .Net, Go Language 등이 있는데  
다른 언어들은 네이버 클라우드 콘솔에서 직접 코드를 입력하면 되지만, Java는 로컬에서 작업 후 jar 파일로 .Net은 zip 파일로 압축해서 따로 등록해야 합니다.  
여기서는 윈도우 명령프롬프트(cmd)에서 .Net 그 중에서도 C#으로 Action을 만들고 zip 파일로 압축 후에 콘솔에 등록하고 테스트 하는 과정까지 정리해보겠습니다.

## .Net 설치
네이버 클라우드 Cloud Functions는 .Net Standard 2.0 규격을 요구하는데, 이 규격을 지원하는 버전을 설치하려면 .Net 5.0 또는 .Net Core 2.1 이상을 설치하면 됩니다.  
가장 간단한 방법은 Visual Studio를 설치하는 방법이고 그 외에는 .Net 또는 .Net Core SDK만 별도로 설치하는 방법이 있습니다.

- Visual Studio 무료버전 : <a href="https://visualstudio.microsoft.com/ko/free-developer-offers/" target="_blank" style="word-break:break-all;">https://visualstudio.microsoft.com/ko/free-developer-offers/</a>
- .Net, .Net Core SDK : <a href="https://dotnet.microsoft.com/download" target="_blank" style="word-break:break-all;">https://dotnet.microsoft.com/download</a>

## 프로젝트 생성
CloudFunctionsTestConsole 라는 이름의 Class Library 프로젝트를 생성하면서 언어는 C#, 타겟 프레임워크는 .Net Standard 2.0으로 지정하는 명령어입니다.  
다음으로 생성된 프로젝트 폴더로 이동해서 Newtonsoft.Json이라는 Json 패키지를 설치합니다.

``` bat
:: 프로젝트 생성
D:\>dotnet new classlib -n CloudFunctionsTestConsole -lang "C#" -f netstandard2.0

:: 폴더 이동
D:\>cd CloudFunctionsTestConsole

:: Json 패키지 설치
D:\CloudFunctionsTestConsole>dotnet add package Newtonsoft.Json
```
<br />
실제 명령프롬프트(cmd)에서 위 명령을 실행해본 화면은 다음과 같습니다.

<img src="../../images/ncp_server_cloud_functions_dotnet_csharp_cmd_01.jpg" alt="네이버 클라우드 Cloud Functions Action을 .Net (C#)을 사용하여 윈도우 명령프롬프트(cmd)에서 만드는 방법 " style="width:800px;align:center">

## Hello.cs 작성
이제 name이라는 파라미터를 json 형태로 받아서 출력해주는 Hello.cs 스크립트를 작성합니다.

```c#
using System;
using Newtonsoft.Json.Linq;

namespace CloudFunctionsTestConsole
{
    public class Hello
    {
        public JObject Main(JObject args)
        {
            string name = "no name";
            if (args.ContainsKey("name")) {
                name = args["name"].ToString();
            }
            JObject message = new JObject();
            message.Add("greeting", new JValue($"Hello, {name}!"));
            return (message);
        }
    }
}
```

<img src="../../images/ncp_server_cloud_functions_dotnet_csharp_cmd_04.jpg" alt="네이버 클라우드 Cloud Functions Action을 .Net (C#)을 사용하여 윈도우 명령프롬프트(cmd)에서 만드는 방법 " style="width:600px;align:center">

## 프로젝트 게시
이제 위에서 작성한 스크립트를 publish폴더로 게시하고, zip 파일로 압축합니다.  
여기서 만든 zip 파일을 네이버 클라우드 콘솔에서 등록하게 됩니다.

``` bat
:: 프로젝트 게시
D:\CloudFunctionsTestConsole>dotnet publish -c Release -o publish

:: 폴더 이동
D:\CloudFunctionsTestConsole>cd publish

:: 파일 압축
D:\CloudFunctionsTestConsole\publish>zip -r -0 CloudFunctionsTestConsole.zip *

:: zip 명령이 실행되지 않을 경우 아래와 같이 윈도우 탐색기에서 직접 압축해도 됩니다.
```

<img src="../../images/ncp_server_cloud_functions_dotnet_csharp_cmd_02.jpg" alt="네이버 클라우드 Cloud Functions Action을 .Net (C#)을 사용하여 윈도우 명령프롬프트(cmd)에서 만드는 방법 " style="width:800px;align:center">

> zip 명령이 실행되지 않을 경우 아래와 같이 윈도우 탐색기에서 직접 압축해도 됩니다.

<img src="../../images/ncp_server_cloud_functions_dotnet_csharp_cmd_05.jpg" alt="네이버 클라우드 Cloud Functions Action을 .Net (C#)을 사용하여 윈도우 명령프롬프트(cmd)에서 만드는 방법 " style="width:800px;align:center">

<img src="../../images/ncp_server_cloud_functions_dotnet_csharp_cmd_03.jpg" alt="네이버 클라우드 Cloud Functions Action을 .Net (C#)을 사용하여 윈도우 명령프롬프트(cmd)에서 만드는 방법 " style="width:650px;align:center">



## CF 이용신청
네이버 클라우드 콘솔에서 Cloud Functions에 들어가 이용신청을 합니다.

<img src="../../images/ncp_server_cloud_functions_dotnet_csharp_01.jpg" alt="네이버 클라우드 Cloud Functions Action을 .Net (C#)을 사용하여 만드는 방법 " style="width:800px;align:center">

## CF Action 생성

#### 액션 생성
버튼을 선택해 액션을 생성합니다.

<img src="../../images/ncp_server_cloud_functions_dotnet_csharp_02.jpg" alt="네이버 클라우드 Cloud Functions Action을 .Net (C#)을 사용하여 만드는 방법 " style="width:800px;align:center">

#### 트리거 선택
액션을 생성하기 전에 트리거 조건을 설정해야 하는데, 여기서는 트리거 설정 없이 액션 만들기를 선택하겠습니다.

<img src="../../images/ncp_server_cloud_functions_dotnet_csharp_03.jpg" alt="네이버 클라우드 Cloud Functions Action을 .Net (C#)을 사용하여 만드는 방법 " style="width:800px;align:center">

#### 이름 입력
액션의 이름은 특별한 규칙이 없으니 알아보기 쉬운 것으로 입력하면 됩니다.

<img src="../../images/ncp_server_cloud_functions_dotnet_csharp_cmd_06.jpg" alt="네이버 클라우드 Cloud Functions Action을 .Net (C#)을 사용하여 만드는 방법 " style="width:800px;align:center">

#### 소스코드 언어 선택
소스코드 언어 중에서 저희는 dotnet:2.2를 선택합니다.

<img src="../../images/ncp_server_cloud_functions_dotnet_csharp_05.jpg" alt="네이버 클라우드 Cloud Functions Action을 .Net (C#)을 사용하여 만드는 방법 " style="width:800px;align:center">

#### 소스코드 업로드
소스코드 타입은 코드와 파일이 있지만, java와 .Net은 파일 업로드만 가능합니다.
앞에서 만든 소스코드를 선택하고 업로드 합니다.

<img src="../../images/ncp_server_cloud_functions_dotnet_csharp_06.jpg" alt="네이버 클라우드 Cloud Functions Action을 .Net (C#)을 사용하여 만드는 방법 " style="width:800px;align:center">

<br />
소스코드가업로드 되었습니다. 등록된 소스코드는 나중에 다운로드 할 수도 있고 다른 파일을 재업로드 할 수도 있습니다.

<img src="../../images/ncp_server_cloud_functions_dotnet_csharp_07.jpg" alt="네이버 클라우드 Cloud Functions Action을 .Net (C#)을 사용하여 만드는 방법 " style="width:800px;align:center">

#### VPC 연결 정보 선택
VPC 환경에서는 연결할 VPC와 Subnet을 선택해야 합니다. Classic 환경에서는 다음 단계로 바로 이동하시면 됩니다.

<img src="../../images/ncp_server_cloud_functions_dotnet_csharp_09.jpg" alt="네이버 클라우드 Cloud Functions Action을 .Net (C#)을 사용하여 만드는 방법 " style="width:800px;align:center">

#### 옵션 설정
실행할 Main 함수의 이름을 {Assembly}::{Class Full Name}::{Method} 형태의 풀네임으로 입력합니다. 
위에서 만든 Hello.cs에서는 **CloudFunctionsTestConsole::CloudFunctionsTestConsole.Hello::Main** 으로 입력합니다.

액션 메모리와 Timeout은 기본으로 두셔도 되고 익숙해진 이후에 상황에 맞게 조정하시면 됩니다.

<img src="../../images/ncp_server_cloud_functions_dotnet_csharp_10.jpg" alt="네이버 클라우드 Cloud Functions Action을 .Net (C#)을 사용하여 만드는 방법 " style="width:800px;align:center">

<br />
입력할 Main 함수 이름을 어떻게 적으면 되는지 한번 더 살펴보겠습니다.  
아래 소스코드 화면에서 보시면 namespace, class, Main 이렇게 이름이 적혀 있는 곳에서  
{ 1 }::{ 1 }.{ 2 }::{ 3 } 이렇게 연결해서 적으면 CloudFunctionsTestConsole::CloudFunctionsTestConsole.Hello::Main 이렇게 완성이 되고  
이 이름을 Main 함수 이름 칸에 입력하시면 됩니다.

<img src="../../images/ncp_server_cloud_functions_dotnet_csharp_16.jpg" alt="네이버 클라우드 Cloud Functions Action을 .Net (C#)을 사용하여 만드는 방법 " style="width:800px;align:center">

#### 생성 완료
디폴트 파라미터의 경우 필요하실 경우 설정하시면 됩니다.  
모든 준비가 끝났으면 생성 버튼을 클릭하여 액션을 생성합니다.

<img src="../../images/ncp_server_cloud_functions_dotnet_csharp_11.jpg" alt="네이버 클라우드 Cloud Functions Action을 .Net (C#)을 사용하여 만드는 방법 " style="width:800px;align:center">


## CF Action 실행
이제 생성된 액션을 실행해보겠습니다.
액션의 기본정보 화면에서는 액션 실행과 수정, 삭제를 할 수 있고, 모니터링 화면에서는 액션이 실행된 통계 정보를 확인할 수 있습니다.

<img src="../../images/ncp_server_cloud_functions_dotnet_csharp_12.jpg" alt="네이버 클라우드 Cloud Functions Action을 .Net (C#)을 사용하여 만드는 방법 " style="width:800px;align:center">

<br />
액션 실행화면 왼쪽에는 파라미터를 입력할 수 있고,
오른쪽에는 결과가 나오는데 전체 결과 메시지를 확인하거나 결과만 보기 옵션으로 최종 성공 실패에 대한 결과 메시지만 볼 수도 있습니다.  
우선은 파라미터 없이 실행해 보았고, 무사히 성공한 결과가 나왔습니다.

<img src="../../images/ncp_server_cloud_functions_dotnet_csharp_13.jpg" alt="네이버 클라우드 Cloud Functions Action을 .Net (C#)을 사용하여 만드는 방법 " style="width:800px;align:center">

<br />
이번에는 파라미터를 입력하고 액션을 실행해보았습니다.  파라미터는 json 형태로 입력하면 됩니다.
왼쪽 창에서 입력한 파라미터가 결과에 잘 반영되어 나왔습니다.

<img src="../../images/ncp_server_cloud_functions_dotnet_csharp_14.jpg" alt="네이버 클라우드 Cloud Functions Action을 .Net (C#)을 사용하여 만드는 방법 " style="width:800px;align:center">

<br />
결과만 보기 옵션을 끄고 실행하면 이렇게 스크롤 해야만 전체를 확인할 수 있을 정도로 긴 결과 메시지가 나타납니다.

<img src="../../images/ncp_server_cloud_functions_dotnet_csharp_15.jpg" alt="네이버 클라우드 Cloud Functions Action을 .Net (C#)을 사용하여 만드는 방법 " style="width:800px;align:center">


## 오류 메시지
위의 순서대로 진행을 하면 문제 없이 사용 가능한데, 이미 다른 방법으로 진행해보면서 오류 메시지를 경험하는 경우도 있을 듯하여 가장 많이 겪게 되는 오류 상황 2가지를 정리해보겠습니다.

#### Main 함수 이름 입력 오류 
위에서 설명한 액션 Main 함수 이름을 올바르게 입력하지 않으면 다음과 같은 오류 메시지가 나타납니다.  

``` json
"error" : "main required format is \"Assembly::Type::Function\"."
```
<img src="../../images/ncp_server_cloud_functions_dotnet_csharp_17.jpg" alt="네이버 클라우드 Cloud Functions Action을 .Net (C#)을 사용하여 만드는 방법 " style="width:800px;align:center">

<br />
상단에 설명한 [CF Action 생성] - [옵션 설정]에서 Main 함수 입력하는 부분을 참고하셔서 정확하게 입력하시면 문제가 해결됩니다.

#### .Net 대상 프레임워크 오류
네이버 클라우드 Cloud Functions은 .Net Standard 2.0 규약을 지원하고 있습니다.  
그런데 이 대상 프레임워크가 맞지 않으면 다음과 같은 오류 메시지가 나타납니다.

``` json
"error" : "Unable to locate requested type (\"CloudFunctionsTestConsole.Hello\")."
```
<img src="../../images/ncp_server_cloud_functions_dotnet_csharp_18.jpg" alt="네이버 클라우드 Cloud Functions Action을 .Net (C#)을 사용하여 만드는 방법 " style="width:800px;align:center">

<br />
위 [프로젝트 생성]에서 설명한 것처럼 프로젝트 생성할 때 **-f netstandard2.0** 옵션을 반드시 넣어주셔어야 합니다.

``` bat
:: 프로젝트 생성
D:\>dotnet new classlib -n CloudFunctionsTestConsole -lang "C#" -f netstandard2.0
```



## 참고 URL
<a href="https://guide.ncloud-docs.com/docs/compute-compute-15-2-6" target="_blank" style="word-break:break-all;">https://guide.ncloud-docs.com/docs/compute-compute-15-2-6.html</a>

> 문서 최종 수정일 : 2021-03-09