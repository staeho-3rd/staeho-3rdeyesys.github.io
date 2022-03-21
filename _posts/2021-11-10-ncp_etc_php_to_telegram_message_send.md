---
date: 2021-11-10
last_modified_at: 2021-11-10
title: PHP로 텔레그램 비공개 채널에 메시지 전송하기
categories:
  - 99.ETC
description: PHP로 텔레그램 비공개 채널에 메시지 전송하는 방법입니다
type: Document
set: PHP
order_number: 1
v2_link: /etc/etc_php_to_telegram_message_send.html
---

## 개요
업무를 진행하다보면 서버나 서비스에 장애가 발생했을 때 즉시 알림을 받거나 서버 상태 모니터링이나 매출 등의 서비스 이용 지표 등을 매일 자동으로 통보 받는 경우가 있습니다. 
예전에는 이메일이나 SMS 메시지로 받는 경우가 대부분이었는데, 요즘은 관련된 사람들이 함께 들어와 있는 메신저 채팅방으로 알림을 동시에 받는 경우도 많아지고 있습니다.  
그럴 때 사용하는 방법 중에서 여기서는 텔레그램(Telegram)의 대화방인 비공개 채널에 메시지를 전송하는 것을 PHP로 구현해보겠습니다.

## 구현 순서
1. 텔레그램 채팅 봇을 생성
2. 텔레그램 채널을 비공개로 생성
3. 비공개 채널에 위에서 생성한 채팅 봇을 관리자로 등록
4. PHP로 채팅 봇을 이용해 비공개 채널에 메시지를 전송


## 채팅 봇 생성
텔레그램(Telegram) 메신저에서 채팅 봇을 생성하고 관리해주는 봇인 [**BotFather**]를 검색합니다. 
여러 개의 봇들이 나타나는데 그 중에서 인증 마크가 붙어 있는 텔레그램 공식 봇을 선하고 채팅 방에  들어갑니다.

<img src="/images/etc_php_to_telegram_msg_send_01.png" alt="PHP로 텔레그램(Telegram) 비공개 채널에 메시지 전송하는 방법" style="width:770px;align:center">

[BotFather] 채팅 봇 방에 들어가면 채팅 봇에 대한 설명과 봇 API 매뉴얼 링크를 확인할 수 있습니다.

<img src="/images/etc_php_to_telegram_msg_send_02.png" alt="PHP로 텔레그램(Telegram) 비공개 채널에 메시지 전송하는 방법" style="width:642px;align:center">

이제 채팅 봇 생성을 위해 **/start** 명령을 입력하면 아래와 같이 채팅 봇 생성, 관리에 대한 명령어들을 확인할 수 있습니다.

<img src="/images/etc_php_to_telegram_msg_send_03.png" alt="PHP로 텔레그램(Telegram) 비공개 채널에 메시지 전송하는 방법" style="width:637px;align:center">

다음으로 새로운 채팅 봇을 생성하는 명령어인 **/newbot**을 입력하면, 채팅 봇 이름과 채팅 봇 유저명을 입력하라는 안내가 나옵니다. 
차례대로 입력하면 되는데, 여기서 입력하는 이름을 텔레그램 내에서 고유한 값이므로 간단한 이름을 입력하면 중복된 이름이라 생성이 되지 않으니 자신만의 고유한 이름을 입력합니다.  

그리고 두번째로 입력하는 **채팅 봇 유저명(username for your bot)은 반드시 마지막이 bot**로 끝나야 합니다. 
두가지 모두 입력하고 나면 아래쪽에 채팅 봇과 연동하기 위한 API Token을 확인할 수 있는데, 이 Token을 PHP에서 사용하게 됩니다. 

<img src="/images/etc_php_to_telegram_msg_send_04.png" alt="PHP로 텔레그램(Telegram) 비공개 채널에 메시지 전송하는 방법" style="width:637px;align:center">

채팅 봇이 생성되었는지 텔레그램에서 검색해보면 아래와 같이 확인할 수 있습니다.

<img src="/images/etc_php_to_telegram_msg_send_05.png" alt="PHP로 텔레그램(Telegram) 비공개 채널에 메시지 전송하는 방법" style="width:632px;align:center">

## 채널 생성
채널을 생성하기 위해 텔레그램 왼쪽 메뉴를 클릭합니다.

<img src="/images/etc_php_to_telegram_msg_send_06.png" alt="PHP로 텔레그램(Telegram) 비공개 채널에 메시지 전송하는 방법" style="width:632px;align:center">

메뉴 화면에서 [채널 만들기]를 클릭합니다.

<img src="/images/etc_php_to_telegram_msg_send_07.png" alt="PHP로 텔레그램(Telegram) 비공개 채널에 메시지 전송하는 방법" style="width:632px;align:center">

원하는 채널명을 입력하고, 만들기를 클릭하면, 공개-비공개를 선택할 수 있는데 여기서는 [비공개 채널]을 선택하고 저장합니다.

<img src="/images/etc_php_to_telegram_msg_send_08.png" alt="PHP로 텔레그램(Telegram) 비공개 채널에 메시지 전송하는 방법" style="width:770px;align:center">

저장하고 나면 다음으로 [참가자 추가] 화면이 나타나는데 위에서 생성했던 채팅 봇 이름을 입력해서 확인하고, 선택 후에 [추가] 버튼을 클릭합니다.

<img src="/images/etc_php_to_telegram_msg_send_10.png" alt="PHP로 텔레그램(Telegram) 비공개 채널에 메시지 전송하는 방법" style="width:770px;align:center">

그러면 아래와 같이 [봇은 관리자로서만 추가될 수 있습니다]라는 메시지가 나타납니다. 바로 [관리자로 세우기] 버튼을 클릭합니다.

<img src="/images/etc_php_to_telegram_msg_send_12.png" alt="PHP로 텔레그램(Telegram) 비공개 채널에 메시지 전송하는 방법" style="width:632px;align:center">

관리자의 권한은 여러가지가 있는데 원하는 권한을 부여하고 [저장] 버튼을 클릭합니다.

<img src="/images/etc_php_to_telegram_msg_send_13.png" alt="PHP로 텔레그램(Telegram) 비공개 채널에 메시지 전송하는 방법" style="width:632px;align:center">

## 채팅 방 ID 확인
메시지를 전송하기 위해서는 현재 생성된 비공개 채널의 chat_id를 확인해야 합니다.  
우선, 채널에서 아무 메시지나 입력합니다. (**꼭 하셔야 합니다**)

<img src="/images/etc_php_to_telegram_msg_send_14.png" alt="PHP로 텔레그램(Telegram) 비공개 채널에 메시지 전송하는 방법" style="width:632px;align:center">

다음으로 웹브라우저에서 다음 주소를 입력합니다.  주소에는 위에서 확인했던 API Token이 들어갑니다.

``` html
https://api.telegram.org/bot[API Token]/getUpdates

# 예시
https://api.telegram.org/bot123456789:JFDS89FMJK8932-MKJE8FNBH3289I/getUpdates
```
<br />
입력하면 나타나면 결과에서 "chat" : {"id" : -12586498, ....} 와 같은 형식의 **id** 값을 복사합니다. 이 값이 바로 비공개 채널의 chat_id 입니다.

<img src="/images/etc_php_to_telegram_msg_send_15.png" alt="PHP로 텔레그램(Telegram) 비공개 채널에 메시지 전송하는 방법" style="width:770px;align:center">

## 메시지 전송 코드 작성
마지막으로 메시지를 전송하는 PHP 코드를 작성해보겠습니다.  
위에서 구했던 [API Token], [chat_id]를 아래 코드에 입력하고 실행하면 됩니다.
``` php
<?php

  function php_to_telegram_msg_send($msg) 
  {
    $api_token = "API Token";
    $chat_id = "채팅방 ID";
    $api_url = "https://api.telegram.org/bot".$api_token."/sendMessage";

    $post_vars = "chat_id=".$chat_id."&text=".urlencode($msg);
    $content_type = "Content-Type: application/x-www-form-urlencoded";

    $ch = curl_init();

    curl_setopt($ch, CURLOPT_URL, $api_url);
    curl_setopt($ch, CURLOPT_HEADER, false);
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
    curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, false);
    curl_setopt($ch, CURLOPT_POST, true);
    curl_setopt($ch, CURLOPT_TIMEOUT, 5);
    curl_setopt($ch, CURLOPT_HTTPHEADER, array($content_type));
    curl_setopt($ch, CURLOPT_POSTFIELDS, $post_vars);

    $return = curl_exec($ch);

    curl_close($ch);

    return $return;
  }


  $tgm_msg = "";		
  $tgm_msg = $tgm_msg."메시지 테스트";

  $output = php_to_telegram_msg_send($tgm_msg);	
?>
```
<br />
위 코드를 실행하면 아래와 같이 비공개 채널에 메시지가 도착한 것을 확인할 수 있습니다.

<img src="/images/etc_php_to_telegram_msg_send_16.png" alt="PHP로 텔레그램(Telegram) 비공개 채널에 메시지 전송하는 방법" style="width:632px;align:center">

## 메시지 스타일 적용
텔레그램 채팅 봇 API는 메시지에 bold, underline 등의 몇가지 html 스타일을 지원합니다.  
html 스타일을 적용하기 위해서는 전송 파라미터에 **parse_mode=html**을 추가해야 합니다.  
그러면 위 전송 코드 중에서 변경해야 하는 코드만 정리해보겠습니다.

``` php
<?php

  /* 전송 파라미터에 parse_mode=html을 추가합니다. */
  $post_vars = "chat_id=".$chat_id."&text=".urlencode($msg)."&parse_mode=html";		

  $tgm_msg = $tgm_msg."<b>bold</b>"."\r\n";
  $tgm_msg = $tgm_msg."<i>italic</i>"."\r\n";
  $tgm_msg = $tgm_msg."<code>\$now_date = date('Y-m-d');</code>"."\r\n";
  $tgm_msg = $tgm_msg."<s>strike</s>"."\r\n";
  $tgm_msg = $tgm_msg."<u>underline</u>"."\r\n";
  $tgm_msg = $tgm_msg."<pre language='php'>\$now_date = date('Y-m-d');</pre>"."\r\n";
  $tgm_msg = $tgm_msg."email@test.com"."\r\n";
  $tgm_msg = $tgm_msg."<pre>email@test.com</pre>"."\r\n";
?>
```
<br />
위 코드처럼 스타일을 적용하고 실행하면 아래와 같이 표시됩니다.

<img src="/images/etc_php_to_telegram_msg_send_17.png" alt="PHP로 텔레그램(Telegram) 비공개 채널에 메시지 전송하는 방법" style="width:632px;align:center">

## 참고 URL
1.  텔레그램 채팅 봇 안내
	- <a href="https://core.telegram.org/bots/" target="_blank" style="word-break:break-all;">https://core.telegram.org/bots/</a>

2.  텔레그램 채팅 봇 API 가이드
	- <a href="https://core.telegram.org/bots/api/" target="_blank" style="word-break:break-all;">https://core.telegram.org/bots/api/</a>

3.  텔레그램 채팅 봇 API 메시지 스타일 가이드
	- <a href="https://core.telegram.org/api/entities/" target="_blank" style="word-break:break-all;">https://core.telegram.org/api/entities/</a>
