---
date: 2021-08-12
last_modified_at: 2021-09-16
title: AWS S3 수명 주기 (LifeCycle) 설정하기
categories:
  - 91.AWS
description: AWS S3 수명 주기 (LifeCycle) 설정하기 가이드입니다
type: Document
set: storage
order_number: 10
creator: elcidme
v2_link: /aws/aws_s3_lifecycle_management.html
---

## 개요
AWS S3에서는 버킷내 특정 디렉토리 하위 파일들에 대해 지정된 날짜가 지난 후 만료(삭제)가 되도록 설정할 수 있습니다. 
수명 주기 관리(LifeCycle Management)라고 불리는 이 방법은 파일관리 뿐만 아니라 비용절감에도 도움이 됩니다.

## 수명주기 설정
AWS 콘솔에 접속 후 수명 주기를 적용할 S3 버킷으로 이동해 [관리] 메뉴 클릭합니다.

<img src="/images/aws_s3_lifecycle_01.jpg" alt="AWS S3 수명주기 (LifeCycle) 설정하기 가이드" style="width:770px;align:center">

[수명 주기 규칙 생성] 버튼을 클릭합니다.

<img src="/images/aws_s3_lifecycle_02.jpg" alt="AWS S3 수명주기 (LifeCycle) 설정하기 가이드" style="width:770px;align:center">

수명 주기 규칙 이름을 입력하고, 규칙 범위는 [하나 이상의 필터를 사용하여 이 규칙의 범위 제한]을 선택합니다.  
접두사에는 수명 주기를 적용할 디렉토리나 파일 등의 필터를 작성합니다. 예를 들어 images 디렉토리가 포함된 모든 파일에 적용하려면 **images/** 라고 입력합니다. 이때 버킷명은 제외하고 입력해야 합니다.

<img src="/images/aws_s3_lifecycle_03.jpg" alt="AWS S3 수명주기 (LifeCycle) 설정하기 가이드" style="width:770px;align:center">

수명 주기 규칙 작업에서 [객체의 현재버전 만료],  [객체의 이전버전 영구 삭제] 두가지 항목을 선택하면, 몇일 후에 삭제할 것인지를 설정할 수 있는 [객체 생성 후 경과 일수], [객체가 이전 버전이 된 후 경과 일수] 항목이 나타납니다. 
여기에 원하는 날짜를 각각 입력하고, [규칙 생성] 버튼을 클릭합니다.

<img src="/images/aws_s3_lifecycle_06.jpg" alt="AWS S3 수명주기 (LifeCycle) 설정하기 가이드" style="width:770px;align:center">

## 주의사항
AWS 수명 주기 관련해서 주의해야 할 사항이 몇가지 있습니다.

#### 수명 주기 규칙 작동 시점
수명 주기 규칙이 생각한 것보다 늦게 작동하는 경우가 있습니다. 이는 **Amazon S3가 객체의 전환 또는 만료(삭제) 날짜를 익일 자정(UTC)부터 계산하기 때문**입니다.  
예를 들어 2020년 1월 1일 10:30(UTC)에 객체를 생성하고 3일 후 객체가 만료(삭제)되도록 수명 주기 규칙을 설정할 경우 객체의 만료(삭제) 날짜는 2020년 1월 5일 00:00(UTC)이 됩니다. 
따라서 수명 주기 규칙이 충족되었는지 확인하기 전에 충분한 시간이 경과했는지 확인해야 합니다.

#### 수명 주기 규칙 접두사 필터 설정
수명 주기 규칙에서 접두사 필터에 디렉토리를 지정할 경우 접두사 필터의 끝에 / 문자를 지정해야 합니다. 접두사 필터의 처음에 / 문자가 있으면 수명 주기 규칙이 올바르게 평가되지 않습니다.  
즉, images 디렉토리가 포함된 모든 파일에 적용하려면 **images/** 라고 입력해야 합니다.

<img src="/images/aws_s3_lifecycle_05.jpg" alt="AWS S3 수명주기 (LifeCycle) 설정하기 가이드" style="width:770px;align:center">


## 참고 URL
1.  AWS S3 수명 주기 관리 가이드
	- <a href="https://docs.aws.amazon.com/ko_kr/AmazonS3/latest/userguide/object-lifecycle-mgmt.html" target="_blank" style="word-break:break-all;">https://docs.aws.amazon.com/ko_kr/AmazonS3/latest/userguide/object-lifecycle-mgmt.html</a>

1.  수명 주기 규칙 작동 지연 관련 FAQ
	- <a href="https://aws.amazon.com/ko/premiumsupport/knowledge-center/s3-lifecycle-rule-delay/" target="_blank" style="word-break:break-all;">https://aws.amazon.com/ko/premiumsupport/knowledge-center/s3-lifecycle-rule-delay/</a>
