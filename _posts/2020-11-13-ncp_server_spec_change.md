---
date: 2020-11-13
last_modified_at: 2021-07-02
title: 서버 스펙 변경
categories:
  - 1.compute
description: 네이버 클라우드 서버 스펙 변경
type: Document
set: server
order_number: 2
v2_link: /compute/ncloud_compute_server_spec_change.html
---

## 개요
네이버 클라우드에서는 기본적으로 서버 타입 간의 스펙 변경을 지원하지 않고, 동일한 타입 내에서의 스펙 변경만 지원합니다.

{: .warning }
다른 타입의 스펙으로 변경하려면 [내 서버 이미지] 기능을 이용해서 서버 이미지를 생성한 다음, 다른 타입으로 서버를 새로 만들어야 합니다. 이때 IP 주소는 변경됩니다.

<img src="/images/ncp_server_spec_change_01.jpg" alt="서버 스펙 변경 제한" style="width:770px;align:center">

{: .success }
타입 간 스펙 변경이 가능한 경우 : 타입간 스펙 변경은 대부분은 불가능하나 **Classic 1세대의 Compact 타입과 Standard 타입** 간에는 스펙 변경을 할 수 있습니다.

<img src="/images/ncp_server_spec_change_02.jpg" alt="서버 스펙 변경 제한" style="width:770px;align:center">

## 참고 URL
<a href="https://guide.ncloud-docs.com/docs/compute-compute-1-1-v2" target="_blank" style="word-break:break-all;">https://guide.ncloud-docs.com/docs/compute-compute-1-1-v2.html</a>

