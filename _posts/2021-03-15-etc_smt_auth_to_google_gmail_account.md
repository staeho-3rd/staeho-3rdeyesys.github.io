---
date: 2021-03-15
title: Gmail을 이용하여 smtp 메일 발송할 때 인증오류 해결 방법
categories:
  - 99.ETC
description: Gmail을 이용하여 smtp 메일 발송할 때 인증오류 해결 방법
type: Document
set: smtp
order_number: 1
---


## 개요
웹서버나 애플리케이션 등에서 gmail 계정을 이용해서 smtp로 메일을 발송하는 경우가 있습니다.  
관리자의 안내 메일이나 소수회원을 위한 다량메일 발송등을 하는 것이 그런 경우인데  
예전에는 **보안 수준이 낮은 앱의 액세스 허용** 옵션으로 가능했었지만, 최근에는 이 설정으로도 해결되지 않는 경우가 많습니다.  

이럴 때 확실하게 인증할 수 있는 방법이 구글계정 **2단계 인증 사용**과 **앱 비밀번호 설정**입니다.

## 인증 오류 메시지
위에서 이야기한 2단계 인증과 앱 비밀번호를 사용하지 않고 smtp로 구글계정에 로그인 하려고 하면 다음과 같은 오류 메시지가 나타나는 경우가 있습니다.

``` bash
The SMTP server requires a secure connection or the client was not authenticated.
The server response was: 5.7.0 Authentication Required.
```

이 문제, 인증 오류 메시지를 해결하려면 아래 내용대로 설정을 하시면 해결됩니다.

## 2단계 인증
2단계 인증이란 구글계정에 로그인 할 때 아이디와 비밀번호 외에 추가로 인증 절차를 거쳐 로그인을 인증하는 것을 말합니다.  
2단계 인증 방법에는 다음과 같은 종류가 있습니다.

- Google 메시지
- 앱 비밀번호
- 백업 코드
- 백업 전화
- 휴대전화 내장 보안 키

#### 인증 설정하기
우선 구글계정으로 로그인해서 계정-보안 메뉴로 이동합니다.  
보안 메뉴에서 [Google에 로그인]이라는 곳에 보면 [2단계 인증] 메뉴가 있습니다.   
현재는 사용 안함으로 설정되어 있는데 클릭해서 설정을 시작합니다.

<a href="https://myaccount.google.com/security" target="_blank" style="word-break:break-all;">https://myaccount.google.com/security</a>

<img src="../../images/etc_smtp_auth_to_google_gmail_account_01.jpg" alt="Gmail을 이용하여 smtp 메일 발송할 때 인증오류 해결 방법" style="width:800px;align:center">

2단계 인증에 대한 기본적인 안내와 함께 설정이 시작됩니다.

<img src="../../images/etc_smtp_auth_to_google_gmail_account_02.jpg" alt="Gmail을 이용하여 smtp 메일 발송할 때 인증오류 해결 방법" style="width:800px;align:center">

2단계 로그인할 때 메시지를 받을 기기가 나타납니다. 또는 보안 키 장치나 다른 방법을 옵션으로 선택할 수 있습니다.

<img src="../../images/etc_smtp_auth_to_google_gmail_account_03.jpg" alt="Gmail을 이용하여 smtp 메일 발송할 때 인증오류 해결 방법" style="width:800px;align:center">

로그인을 하고 나면 관련 메시지가 앞 단계에서 선택한 장치에 도착합니다. 저는 휴대폰을 선택했기에 Gmail 앱을 확인해보겠습니다.

<img src="../../images/etc_smtp_auth_to_google_gmail_account_04.jpg" alt="Gmail을 이용하여 smtp 메일 발송할 때 인증오류 해결 방법" style="width:800px;align:center">

이렇게 휴대폰의 Gmail 앱을 열면 안내 메시지가 도착해있고, [예] 버튼을 선택하면 됩니다.

<img src="../../images/etc_smtp_auth_to_google_gmail_account_05.jpg" alt="Gmail을 이용하여 smtp 메일 발송할 때 인증오류 해결 방법" style="width:720px;align:center">

휴대폰에서 [예]를 선택하면 설정화면이 자동으로 다음으로 넘어와 있습니다.  
여기서는 백업 옵션에 대한 방법을 선택합니다. 저는 문자 메시지를 선택했습니다.

<img src="../../images/etc_smtp_auth_to_google_gmail_account_06.jpg" alt="Gmail을 이용하여 smtp 메일 발송할 때 인증오류 해결 방법" style="width:800px;align:center">

잠시 후에 휴대폰 문자메시지로 [국외발신] G-254796(이)가 Google 인증코드입니다. 라는 메시지가 도착합니다.  
이 중에서 뒤에 있는 6자리 숫자 코드를 입력하면 됩니다.

<img src="../../images/etc_smtp_auth_to_google_gmail_account_07.jpg" alt="Gmail을 이용하여 smtp 메일 발송할 때 인증오류 해결 방법" style="width:800px;align:center">

이제 마지막으로 2단계 인증을 사용할 것인지 최종 확인을 합니다. [사용 설정] 버튼을 클릭하시면 2단계 인증이 적용됩니다.

<img src="../../images/etc_smtp_auth_to_google_gmail_account_08.jpg" alt="Gmail을 이용하여 smtp 메일 발송할 때 인증오류 해결 방법" style="width:800px;align:center">

적용된 2단계 인증에 대한 안내화면입니다. Google 메시지가 2단계 인증 기본값으로 설정되어 있고, 그 외에 음성 또는 문자 메시지도 이용 가능하다는 내용입니다.  
이제 위쪽에 있는 뒤로 돌아가기 버튼을 클릭해서 메인화면으로 돌아갑니다.

<img src="../../images/etc_smtp_auth_to_google_gmail_account_09.jpg" alt="Gmail을 이용하여 smtp 메일 발송할 때 인증오류 해결 방법" style="width:800px;align:center">

메인화면으로 돌아가면 2단계 인증이 사용으로 설정된 것을 확인할 수 있고, 그 아래에 앱 비밀번호 항목이 새로 생긴 것을 알 수 있습니다.

<img src="../../images/etc_smtp_auth_to_google_gmail_account_10.jpg" alt="Gmail을 이용하여 smtp 메일 발송할 때 인증오류 해결 방법" style="width:800px;align:center">


## 앱 비밀번호 설정
앱 비밀번호 설정을 시작하면 어떤 앱에서 사용할 것인지, 기기는 어떤 것인지 선택하는 화면이 나옵니다.  
메일, 캘린더, 연락처, Youtube 등이 있는데 저희는 Smtp를 할 것이기 때문에 기타(맞춤 이름)을 선택합니다.

<img src="../../images/etc_smtp_auth_to_google_gmail_account_11.jpg" alt="Gmail을 이용하여 smtp 메일 발송할 때 인증오류 해결 방법" style="width:800px;align:center">

기타를 선택하면 기기선택은 필요없기 때문에 앱 이름만 원하는대로 입력합니다. 여기서는 Smtp Client라고 입력하고 생성 버튼을 클릭합니다.

<img src="../../images/etc_smtp_auth_to_google_gmail_account_12.jpg" alt="Gmail을 이용하여 smtp 메일 발송할 때 인증오류 해결 방법" style="width:800px;align:center">

드디어 16자리 앱 비밀번호가 생성되었습니다.  이 번호를 반드시 복사해서 따로 저장을 하고 확인 버튼을 클릭합니다.

<img src="../../images/etc_smtp_auth_to_google_gmail_account_13.jpg" alt="Gmail을 이용하여 smtp 메일 발송할 때 인증오류 해결 방법" style="width:800px;align:center">

앱 비밀번호 화면에서는 현재 생성된 비밀번호를 삭제하거나 추가 할 수 있습니다.

<img src="../../images/etc_smtp_auth_to_google_gmail_account_14.jpg" alt="Gmail을 이용하여 smtp 메일 발송할 때 인증오류 해결 방법" style="width:800px;align:center">

다시 계정 보안 메인 화면으로 돌아오면 앱 비밀번호 1개가 설정되었다는 것을 확인할 수 있습니다.

<img src="../../images/etc_smtp_auth_to_google_gmail_account_15.jpg" alt="Gmail을 이용하여 smtp 메일 발송할 때 인증오류 해결 방법" style="width:800px;align:center">


## 앱 비밀번호 적용
그러면 smtp 소스코드에서 어떻게 앱 비밀번호를 적용하는지 확인해보겠습니다.

#### PHP
``` php
$mail = new PHPMailer();
$mail->IsSMTP();
$mail->SMTPAuth = true;
$mail->SMTPSecure = "tls";
$mail->Host = "smtp.gmail.com";
$mail->Port = 587;
$mail->Username = "Gmail 계정";
$mail->Password = "앱 비밀번호";
```
<br />
#### .Net C#
``` c#
SmtpClient client = new SmtpClient("smtp.gmail.com", 587);
client.UseDefaultCredentials = false;
client.EnableSsl = true;
client.DeliveryMethod = SmtpDeliveryMethod.Network;
client.Credentials = new System.Net.NetworkCredential("Gmail 계정", "앱 비밀번호");
```

이렇게 기존에 gmail 계정 비밀번호를 입력하던 곳에 앱 비밀번호를 입력하면 문제없이 잘 접속됩니다.


> 문서 최종 수정일 : 2021-03-16