---
date: 2021-01-04
last_modified_at: 2021-01-04
title: jekyll 설치(윈도 10) with base-theme
categories:
  - 99.ETC
description: 윈도 10 환경에서 base-theme를 기반으로 jekyll 설치하는 방법에 대한 가이드입니다.
type: Document
set: jekyll
order_number: 1
v2_link: /etc/etc_jekyll_install.html
---

## 설치 전체과정 요약

1. Ruby 설치	
2. ridk 설치
3. gem 업데이트
4. jekyll, Bundler 설치


## Ruby 설치
일반적으로 jekyll를 사용한다면 최신 버전을 사용하면 되겠지만 여기서는 base-theme를 기반으로 하기 때문에 호환에 잘되는 2.5를 설치합니다.

Ruby Installer 다운로드 경로
<a href="https://rubyinstaller.org/downloads/" target="_blank" style="word-break:break-all;">https://rubyinstaller.org/downloads/</a>

위 사이트에서 rubyinstaller-devkit-2.5.8-2-x64 를 다운 받아 설치하면 됩니다.

Ruby 설치가 끝나면서 완료화면에 **Run 'ridk install' to setup MSYS2 and development toolchain.** 이라는 옵션이 나타납니다.  
반드시 선택하고 완료를 하시기 바랍니다.

<img src="/images/jekyll_install_ruby.jpg" alt="jekyll 설치화면" style="width:600px;align:center">

### ridk 설치
앞단계인 Ruby 설치 완료에서 ridk install을 선택했다면 ridk 설치 커맨드 창이 나타납니다.  
(혹시 선택하지 않았다면 커맨드 창을 열어서 ridk install 을 입력하면 됩니다)  
이때 설치 옵션을 선택할 수 있는데 1,2,3 을 선택하고 Enter 키를 입력하면 설치가 진행됩니다.

<img src="/images/jekyll_install_ridk.jpg" alt="jekyll ridk 설치화면" style="width:600px;align:center">

## gem 업데이트

``` bash
gem update
```

## Jekyll, Bundler 설치

``` bash
gem install jekyll bundler
```

## base-theme 설치
base-theme는 jekyll의 여러 테마 중에서 CloudCannon에서 제작한 테마입니다.

<img src="/images/jekyll_base_theme.png" alt="jekyll base-theme" style="width:600px;align:center">

다운로드 경로
<a href="https://github.com/CloudCannon/base-jekyll-template" target="_blank" style="word-break:break-all;">https://github.com/CloudCannon/base-jekyll-template</a>


위 다운로드 경로에서 소스를 직접 다운 받거나 GitHub Desktop을 이용해서 가져오면 됩니다.

## 블로그 실행, 접속
작업하면서 결과물을 확인할 때는 다음과 같은 명령어로 입력하고 브라우져에서 **http://127.0.0.1:4000/** 로 접속하시면 되니다.

``` bash
bundle exec jekyll serve
```
<br>  
### 사이트 빌드
작업이 끝난 결과물을 실제 서버나 gitHub에 업로드, 배포할 경우에는 다음 명령어로 빌드 한 후에 **_site**에 생성된 html 등을 사용하시면 됩니다.

``` bash
bundle exec jekyll build
```


## 추가 패키지 설치
혹시 블로그 실행, 접속에서 오류가 발생하면 나타나는 메시지를 보고 처리를 해주면 됩니다.  
혹시 필요한 gem이 설치되지 않았을 경우에는 다음과 같이 설치해주면 됩니다.

``` bash
gem install public_suffix -v 3.0.1
```

## 기본 블로그 생성
위에서 소개한 것처럼 테마를 사용하는 것이 아닌 기본 설정만의 블로그를 새로 만들려면 다음과 같이 입력하면 됩니다.

``` bash
jekyll new PATH
```
기본 블로그를 만들고 접속하면 다음과 같은 화면이 나타납니다.

<img src="/images/jekyll_install_default_blog.jpg" alt="jekyll 기본 블로그" style="width:600px;align:center">
