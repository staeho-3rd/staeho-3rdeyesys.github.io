---
date: 2021-05-12
last_modified_at: 2021-05-12
title: Cloud Outbound Mailer로 대량 메일 발송하는 방법
categories:
  - 9.application service
description: 네이버 클라우드에서 Cloud Outbound Mailer로 대량 메일 발송하는 방법입니다.
type: Document
set: mail
order_number: 1
v2_link: /application-service/ncloud_application_service_cloud_outbound_mailer_bulk_mail_send.html
---

## 개요
네이버 클라우드 서비스 중에서 각 종 공지나 이벤트, 마케팅으로 회원들에게 대량의 메일을 발송해야 할 때 사용할 수 있는 것이 Cloud Outbound Mailer 입니다.  
대량 메일을 발송하는 방법은 여러가지 있지만, 그 중에서도 가장 자주, 간편하게 사용하는 것이 Excel 등의 파일을 업로드 해서 발송하는 방법입니다.  


## 이용 신청
Cloud Outbound Mailer는 먼저 이용신청을 하셔야 합니다.
[Console] - [Cloud Outbound Mailer] - [Mailing list] 에서 [이용 신청] 버튼을 클릭하시면 됩니다.

<img src="/images/cloud_outbound_mailer_01.jpg" alt="Cloud Outbound Mailer로 대량 메일 발송하는 방법" style="width:800px;align:center">


다음으로 서비스 이용약관에 동의하시면 됩니다.

<img src="/images/cloud_outbound_mailer_02.jpg" alt="Cloud Outbound Mailer로 대량 메일 발송하는 방법" style="width:800px;align:center">


## 메일 작성
메일링 리스트 화면에서 발송하기 버튼을 클릭합니다.
<img src="/images/cloud_outbound_mailer_03.jpg" alt="Cloud Outbound Mailer로 대량 메일 발송하는 방법" style="width:800px;align:center">


메일 작성-발송하기 화면입니다.  
이중에서 보내는 메일주소, 제목, 내용, 받는 사람은 필수 입력 사항입니다.

<img src="/images/cloud_outbound_mailer_bulk_mail_01.jpg" alt="Cloud Outbound Mailer로 대량 메일 발송하는 방법" style="width:800px;align:center">


[대량 발송용 입력양식 파일다운로드]와 [대량 발송용 파일 선택]은 [보내는 메일 주소]와 [제목], [내용] 을 입력해야 활성화 됩니다.

<img src="/images/cloud_outbound_mailer_bulk_mail_02.jpg" alt="Cloud Outbound Mailer로 대량 메일 발송하는 방법" style="width:800px;align:center">

#### 대량메일 발송용 입력양식
Cloud Outbound Mailer에서 제공하는 대량메일 발송용 입력양식을 다운 받아서 보면, [수신자 Email 주소], [NAME] 이렇게 2가지 항목으로 구성되어 있습니다.  

<img src="/images/cloud_outbound_mailer_bulk_mail_03.jpg" alt="Cloud Outbound Mailer로 대량 메일 발송하는 방법" style="width:600px;align:center">


그 외에도 필요한 항목들을 추가해서 사용할 수 있습니다. 여기서는 [GRADE], [GIFT	] 항목을 추가해서 테스트 해보겠습니다. 
추가 항목의 이름은 어떤 것이든 상관없습니다. 실제 메일 내용에서 동일한 이름을 사용하기만 하면 문제 없습니다.

<img src="/images/cloud_outbound_mailer_bulk_mail_04.jpg" alt="Cloud Outbound Mailer로 대량 메일 발송하는 방법" style="width:600px;align:center">


#### 보내는 이름, 보내는 메일주소, 제목 입력
우선, 보내는 이름과 보내는 메일주소, 제목을 입력합니다.  
제목에는 대량 메일 수신자들의 이름이 들어가도록 위에서 확인한 입력 양식에 있던 이름에 해당하는 [NAME] 필드가 들어갈 수 있게 **${NAME} 코드를 입력**합니다.

<img src="/images/cloud_outbound_mailer_bulk_mail_05.jpg" alt="Cloud Outbound Mailer로 대량 메일 발송하는 방법" style="width:800px;align:center">


#### 내용 입력
내용은 직접 입력해도 되고 별도로 제작된 html 파일을 이용할 수도 있는데 Cloud Outbound Mailer에서는 html 파일 업로드는 지원하지 않습니다.  
그러므로 html 소스를 직접 복사해서 화면에 입력해야 합니다.  다음과 같은 샘플용 html을 준비해서 <body>와 </body>태그 사이에 있는 내용을 복사해서 에디터의 html모드에 붙여넣습니다.  

{: .info }
스타일은 css 파일이나 style태그를 사용하지 말고 가급적 태그 안에 inline으로 적용하는 것이 좋습니다. 
메일 서비스 별로 head 태그만 없애는 곳도 있고, style태그를 모두 없애거나, body태그 안쪽의 내용만 남기고 모두 없애는 곳도 있기 때문에 inline 형식으로 적용하는 것이 가장 안전합니다.

<img src="/images/cloud_outbound_mailer_bulk_mail_06.jpg" alt="Cloud Outbound Mailer로 대량 메일 발송하는 방법" style="width:800px;align:center">

복사한 html을 Editor 모드로 바꾸면 다음과 같이 보입니다.  
여기서 위에서 준비한 입력양식 Excel 파일에 있던 ${NAME}, ${GRADE}, ${GIFT} 항목을 적절하게 배치해서 입력해두었습니다.

<img src="/images/cloud_outbound_mailer_bulk_mail_07.jpg" alt="Cloud Outbound Mailer로 대량 메일 발송하는 방법" style="width:800px;align:center">


#### 대량메일 수신자 파일 등록, 확인 후 발송
위에서 준비했던 대량메일 수신자 정보가 들어있는 Excel 파일을 등록하고 [확인 후 발송] 버튼을 클릭합니다.  
바로 발송을 해도 되지만 그것보다는 메일 내용이 제대로 표시되는지, 수신자 목록은 문제 없이 로딩되었는지 확인 한 후에 발송하는 것이 안전합니다.  

<img src="/images/cloud_outbound_mailer_bulk_mail_08.jpg" alt="Cloud Outbound Mailer로 대량 메일 발송하는 방법" style="width:800px;align:center">


## 발송 준비
확인 후 발송을 선택하면 이렇게 [발송 준비] 상태로 리스트에 나타납니다.  
이 상태에서 수신자와 메일 내용이 올바르게 설정되었는지 확인하기 위해 [수신자별 목록] 탭을 선택합니다.

<img src="/images/cloud_outbound_mailer_bulk_mail_09.jpg" alt="Cloud Outbound Mailer로 대량 메일 발송하는 방법" style="width:800px;align:center">


#### 수신자별 목록
수신자별 목록을 살펴보면 다음과 같이 나옵니다.  Excel 파일에 등록해 두었던 이름과 이메일 주소가 문제없이 나타납니다.

<img src="/images/cloud_outbound_mailer_bulk_mail_10.jpg" alt="Cloud Outbound Mailer로 대량 메일 발송하는 방법" style="width:800px;align:center">

#### 메일 내용 확인
수신자별 목록에서 하나를 선택하면 상세 내용이 나오는데, 그 중에서 아래쪽에 있는 [내용보기] 버튼을 클릭해서 메일 본문 내용도 문제가 없는지 확인합니다.

<img src="/images/cloud_outbound_mailer_bulk_mail_11.jpg" alt="Cloud Outbound Mailer로 대량 메일 발송하는 방법" style="width:800px;align:center">

내용 보기를 하시면 아래와 같이 Excel 파일에 등록해 두었던  [NAME], [GRADE], [GIFT] 항목들이 문제없이 설정되어 표시된 것을 확인할 수 있습니다.

<img src="/images/cloud_outbound_mailer_bulk_mail_12.jpg" alt="Cloud Outbound Mailer로 대량 메일 발송하는 방법" style="width:800px;align:center">


## 메일 발송
이제 다시 [요청별 목록]으로 돌아와서 [즉시 발송] 버튼을 클릭해 메일을 발송합니다.

<img src="/images/cloud_outbound_mailer_bulk_mail_13.jpg" alt="Cloud Outbound Mailer로 대량 메일 발송하는 방법" style="width:800px;align:center">

#### 메일 도착 확인
실제 존재하는 메일 주소로 발송해보면 이렇게 메일이 잘 들어온 것을 확인할 수 있습니다.

<img src="/images/cloud_outbound_mailer_bulk_mail_14.jpg" alt="Cloud Outbound Mailer로 대량 메일 발송하는 방법" style="width:800px;align:center">


## 메일 발송 도메인
메일 발송은 잘되었고, 도착도 잘했는데 궁금한 것이 한가지 생깁니다.  
과연 메일 발송 도메인은 어떤 것일까?  그래서 구글 계정으로 보낸 메일에서 발송 도메인을 확인해보았습니다.  
도메인은 **email.ncloud.com** 인 것을 확인할 수 있습니다.

<img src="/images/cloud_outbound_mailer_00.jpg" alt="Cloud Outbound Mailer로 대량 메일 발송하는 방법" style="width:800px;align:center">


## 참고 URL
<a href="https://guide.ncloud-docs.com/docs/email-email-1-2" target="_blank" style="word-break:break-all;">https://guide.ncloud-docs.com/docs/email-email-1-2</a>
