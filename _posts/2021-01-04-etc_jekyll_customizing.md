---
date: 2021-01-04
title: jekyll base-theme 커스터마이징
categories:
  - 99.ETC
description: jekyll base-theme에서 블로그 특성이 맞게 수정하는 방법을 정리한 문서입니다.
type: Document
set: jekyll
order_number: 2
---

## 폰트 교체
base-theme는 기본 폰트가 Merriweather로 되어 있습니다.  
영어 표기에는 적당하지만, 한글 표기에는 좋지 않아 **나눔고딕** 폰트로 변경하였습니다.

그리고 폰트 파일을 별도로 추가하기 보다는 구글에서 제공하는 폰트 경로를 설정하는 방식으로 교체했습니다.  
구글에서 제공하는 폰트 리스트와 적용 방법은 아래 사이트에서 확인 가능합니다.
<a href="https://fonts.google.com/" target="_blank">https://fonts.google.com/</a>


### html 수정

파일 위치 : \_layouts\default.html
``` html
<link rel="preconnect" href="https://fonts.gstatic.com">
<link href="https://fonts.googleapis.com/css2?family=Nanum+Gothic&display=swap" rel="stylesheet"> 
```
<br>
### css 수정

파일 위치: \_sass\typography.scss
``` css
body {
  height: 100%;
  max-height: 100%;
  font-family: 'Nanum Gothic', sans-serif;
  }
``` 

## 한글 검색 오류 수정
base-theme 기본 설정으로는 한글 등 utf-8 언어 검색이 되지 않습니다.  
그에 따라 몇가지 수정을 하면 한글 검색이 가능해집니다.

js 파일 위치: \js\search.js

``` js
기존:
window.index = lunr(function () {
		this.field("id");
		this.field("title", {boost: 10});
		this.field("categories");
		this.field("url");
		this.field("content");
	});

수정: 
window.index = new lunr.Index;
window.index.field('id');
window.index.field('title', { boost: 10 });
window.index.field('author');
window.index.field('category');
window.index.field('content');
```
<br>
다음으로 charset 을 설정합니다.  
html 파일 위치 : \search.html
``` html
기존: 
<script src="{{ site.baseurl }}/js/lunr.min.js"></script>
<script src="{{ site.baseurl }}/js/search.js"></script>

수정: 
<script src="{{ site.baseurl }}/js/lunr.min.js" charset="utf-8"></script>
<script src="{{ site.baseurl }}/js/search.js" charset="utf-8"></script>
```

## 상단 header bg 교체

파일 위치: \_sass\header.scss

``` css
기존:
header {
	background: $header-color;
}

수정: 
header {
	background: url("/images/ncp-header-bg.png");
	background-size: 2200px;
}
```

## logo 파일 교체

파일 위치: \_includes\logo.html

``` html

기존:
<svg version="1.1" id="Layer_1" ..중략.. xml:space="preserve">
	 ..중략..
</svg>

수정:
<img src="/images/title_logo_white.png" style="width:160px;height:33px;margin-top:5px">
```

## 상단 메뉴 수정

파일 위치: \_data\navigation.yml

``` yml
- name: Docs
  link: /
  target: _self
- name: FAQ
  link: /faq/
  target: _self
- name: Company
  link: https://3rdeyesys.com
  target: _blank
```

## 상단 header에 검색 박스 추가

파일 위치: \_includes\navigation.html

``` liquid
{% raw %}{% if page.url != "/" %}
	<span style="width:100px">\{% include search.html %}</span>
{% endif %}	{% endraw %}
</nav>
```

## 하단 footer social 아이콘 수정

파일 위치: \_data\footer.yml

``` yml
- name:
  link: https://www.facebook.com/3rdeyesys
  social_icon: Facebook
  target: _blank
- name:
  link: mailto:biz@3rdeyesys.com
  social_icon: Email
  target: _blank
```

> "문서 최종 수정일 : 2021-01-04"

