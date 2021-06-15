---
date: 2021-06-15
title: Video Player 구현하기 샘플 예제
categories:
  - 6.media
description: 네이버 클라우드 Video Player 구현하기 샘플 예제입니다.
type: Document
set: media
order_number: 1
---


## 개요
네이버 클라우드 비디오 플레이어는 최신의 네이버 미디어 플레이어를 기반으로 개발된 클라우드 전용 상품으로 
네이버 동영상 서비스에 적용되어 수 많은 성공사례를 가지고 있는 최신의 네이버 플레이어를 손쉽고 빠르게 고객의 미디어 서비스에 적용할 수 있습니다.

## 플레이어 생성하기
우선은 네이버 클라우드 콘솔에서 js 파일로 구성된 플레이어를 생성하는 과정부터 살펴보겠습니다.

[네이버 클라우드 콘솔] - [Video Player]에서 [플레이어 생성] 버튼을 클릭합니다.

<img src="../../images/ncp_media_video_player_01.jpg" alt="Video Player 구현하기 샘플 예제" style="width:770px;align:center">

플레이어 이름, 유-무료 상품(현재는 무료만 선택 가능), 플레이어 사이즈, 버전, 플레이어가 저장될 Object Storage 위치 등을 선택합니다.

<img src="../../images/ncp_media_video_player_02.jpg" alt="Video Player 구현하기 샘플 예제" style="width:770px;align:center">

재생 속도 조절 등의 기능을 설정하고, 우클릭 메뉴 바로가기를 추가할 수도 있습니다.

<img src="../../images/ncp_media_video_player_03.jpg" alt="Video Player 구현하기 샘플 예제" style="width:770px;align:center">

자동 재성이나 대역폭 등의 재생 관련 설정을 선택합니다.

<img src="../../images/ncp_media_video_player_04.jpg" alt="Video Player 구현하기 샘플 예제" style="width:770px;align:center">

마지막으로 현재까지 선택한 설정을 확인하고, 플레이어 미리보기를 한 후에 [플레이어 생성] 버튼을 클릭합니다.

<img src="../../images/ncp_media_video_player_05.jpg" alt="Video Player 구현하기 샘플 예제" style="width:770px;align:center">

생성된 플레이어 js 파일의 경로를 확인할 수 있습니다.

<img src="../../images/ncp_media_video_player_06.jpg" alt="Video Player 구현하기 샘플 예제" style="width:770px;align:center">

js 파일이 저장된 Object Storage에서도 확인 가능합니다.

<img src="../../images/ncp_media_video_player_07.jpg" alt="Video Player 구현하기 샘플 예제" style="width:770px;align:center">


## 플레이어 구현하기
생성된 플레이어는 자바스크립트 js 파일이므로 html 파일을 만들어서 플레이어를 구현해야 합니다.

#### 플레이어 JavaScript 소스 예시

``` javascript
  var player = new ncplayer('divVideoPlayer', {
    playlist: "http://example.com/myVideo.mp4"
  });

  player.play();
```
<br />
#### 플레이어 html 소스 예시
아래 예시에서는  컨트롤러 표시, 자동 재생 안함, 음소거, 가로 크기 1024로 옵션을 설정했습니다. 상세한 옵션을 아래쪽에서 다시 살펴보겠습니다.

``` html
<!doctype html>
<html lang="kr">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Video Player Sample</title>

    <script src="https://kr.object.ncloudstorage.com/플레이어 파일명.js"></script>
  </head>

  <body>

    <div style="text-align:center">
      <div id="divVideoPlayer" name="divVideoPlayer"></div>
    </div>

     <script type="text/javascript">

      var player = new ncplayer('divVideoPlayer', {
        controls: true,
        autostart: false,
        mute: true,
        width: 1024,
        playlist: [
        {
          file: 'https://kr.object.ncloudstorage.com/재생할 동영상 파일명.mp4',
        },
        ],
      });

      player.play();

     </script>

   </body>
</html>
```

## 플레이어 구현 예시
위 html 소스대로 플레이어를 구현해서 웹브라우져에서 확인해보면 다음과 같이 나타납니다.

<img src="../../images/ncp_media_video_player_08.jpg" alt="Video Player 구현하기 샘플 예제" style="width:770px;align:center">


## 플레이어 상세 옵션
플레이어 상세 옵션을 간단하게 정리해보겠습니다. 좀 더 자세한 설명은 아래 링크에 있는 네이버 클라우드 가이드 문서를 참고하시면 됩니다.

``` javascript

var player = new ncplayer('divVideoPlayer', {
  controls: true,
  autostart: false,
  mute: true,
  width: 1024,
  playlist: [
    { file: 'https://CDN도메인/example_video_01.mp4' },
    { file: 'https://CDN도메인/example_video_02.mp4' },
    { file: 'https://CDN도메인/example_video_03.mp4/index.m3u8' }
  ],
  });

```

| PROPERTY | TYPE | DESCRIPTION | DEFAULT |
| :---: | :---: | :---: | :---: |
| playlist | string/array | 재생하고자 하는 video 정보 (단일 경로 또는, 여러 경로를 전달) | - |
| autostart | boolean | player 시작 시, video를 자동으로 재생 (브라우저의 자동재생 정책을 따름) | false |
| mute | boolean | player 시작 시, video를 자동으로 음소거 상태로 설정 | false |
| controls | boolean | player 제어를 위한 컨트롤의 표시 여부 결정 | true |
| autoPause | boolean | 창이 전환되거나 최소화되었을 때, 자동으로 player를 정지 상태로 만듦 | false |
| aspectRatio | string | player의 가로X세로 비율 지정, "width:height" 형식으로 전달해야 합니다. | - |
| width | number | player의 가로 사이즈 고정 | - |
| height | number | player의 세로 사이즈 고정 | - |
| playbackRate | boolean | video 의 재생속도 조절, 1.0보다 낮으면 느려지고, 1.0보다 높으면 빠르게 재생 | 1.0 |
| repeat | boolean | video의 반복 재생 여부 결정 | false |
| about | object | 마우스 우클릭 시 표시되는 바로가기 정보 수정 | false |


## 참고 URL
1. Video Player 사용 가이드
	- <a href="https://guide.ncloud-docs.com/docs/videoplayer-videoplayerguide" target="_blank" style="word-break:break-all;">https://guide.ncloud-docs.com/docs/videoplayer-videoplayerguide</a>

2. Video Player Demo 가이드
	- <a href="https://guide.ncloud-docs.com/docs/videoplayer-videoplayerdemo" target="_blank" style="word-break:break-all;">https://guide.ncloud-docs.com/docs/videoplayer-videoplayerdemo</a>


> 문서 최종 수정일 : 2021-06-14