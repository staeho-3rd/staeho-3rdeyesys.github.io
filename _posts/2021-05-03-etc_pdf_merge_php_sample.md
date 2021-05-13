---
date: 2021-04-13
title: Pdf 문서 합치기 PHP로 구현하는 방법
categories:
  - 99.ETC
description: PHP로 Pdf 문서 합치는 기능을 구현하는 방법입니다.
type: Document
set: pdf merge
order_number: 1
---

## 개요
요즘은 회사 업무에서 파일을 전달할 때 PDF 문서를 활용하는 경우가 매우 많습니다.   
그런데 이때 보내야 하는 PDF 문서가 여러개 일 경우 받는 쪽에서 각각의 파일을 일일이 열어봐야 해서 불편한 경우가 있습니다.  
이럴때 여러 PDF 문서를 합쳐서 하나의 문서로 만들어 보내면 편리한 경우에 사용할 수 있는 PDF 문서 합치기 - merge 기능을 PHP로 구현하는 방법입니다.

## 라이브러리 설치
PDF 문서를 합쳐 주는 기능을 가진 라이브러리는 [ php pdf merger ]라고 검색해보면 여러가지 버전이 존재하는데 여기서는 Jurosh 라는 사람이 만든 것을 사용하겠습니다.  
우선 composer 를 이용해서 라이브러리를 설치합니다.

```  bash
composer require jurosh/pdf-merge
```

## 기본 사용법
아래 기본 사용법은 라이브러리 개발자가 GitHub에 올려둔 가이드에 있는 사용법입니다.
``` php

<?php

  require 'vendor/autoload.php';

  // and now we can use library
  $pdf = new \Jurosh\PDFMerge\PDFMerger;

  // add as many pdfs as you want
  $pdf->addPDF('path/to/source/file.pdf', 'all', 'vertical')
    ->addPDF('path/to/source/file1.pdf', 'all')
    ->addPDF('path/to/source/file2.pdf', 'all', 'horizontal');

  // call merge, output format `file`
  $pdf->merge('file', 'path/to/export/dir/file.pdf');
?>
```

## 응용 사용법
여기서는 파일 업로드 웹페이지를 통해 업로드 된 여러 파일들을 합쳐서 다운로드 받는 방식으로 구현해보겠습니다.

``` php

<? php

  require 'vendor/autoload.php';
  
  $pdf = new \Jurosh\PDFMerge\PDFMerger;


  $source_file_array = Array();
  $source_filename_array = Array();

  $source_file_array = $_FILES["pdf_file"];
  $source_filename_array = $source_file_array["name"];
  $upload_filename_array = $source_file_array["tmp_name"];

  $cnt_file = count($source_filename_array);

  for ($i = 0; $i < $cnt_file; $i++)
  {
    $pdf->addPDF($upload_filename_array[$i], 'all', 'vertical');    
  }

  $output_filename = $source_filename_array[0];

  $pdf->merge('download', $output_filename);
?>
```
<br />
#### 업로드 된 파일명 배열에 저장
``` php

<? php

  $source_file_array = $_FILES["pdf_file"];
  $source_filename_array = $source_file_array["name"];
  $upload_filename_array = $source_file_array["tmp_name"];
?>
```
<br />
#### 합칠 파일들 리스트에 추가
업로드 된 개수 만큼 파일들을 리스트에 추가합니다.  
$pdf->addPDF의 파라미터에는 옵션으로 페이지와 문서방향이 들어갑니다.  
$pdf->addPDF(파일경로, 페이지, 문서방향);

``` php

<? php

  $cnt_file = count($source_filename_array);
  for ($i = 0; $i < $cnt_file; $i++)
  {
    $pdf->addPDF($upload_filename_array[$i], 'all', 'vertical');    
  }

  /* 사용 예시
  $pdf->addPDF($upload_filename_array[$i], 'all', 'vertical');
  $pdf->addPDF($upload_filename_array[$i], '1,3,6', 'horizontal');
  $pdf->addPDF($upload_filename_array[$i], '12-16', 'vertical');
  */
?>
```
<br />
#### 파일 합치기
합치기 옵션은 [download], [browser], [file], [string] 이렇게 4가지가 있습니다.  
보통 로컬PC로 다운로드 받을 때는 [download], 서버에 저장할 때는 [file]를 선택하면 됩니다.
``` php

<? php

  $pdf->merge('download', $output_filename);
  $pdf->merge('browser', $output_filename);
  $pdf->merge('file', $output_filename);
  $pdf->merge('string', $output_filename);
?>
```
<br />
## 한글 깨짐 해결
PDF 파일을 합치면 문서 내의 텍스트는 한글이나 기타 UTF-8 문자들이 문제없이 잘나타납니다.  
그런데 파일명은 UTF-8이 지원되지 않아 한글이 깨지는 현상이 나타납니다.  

이를 해결하기 위해서는 여기서 사용한 pdf merger 소스 파일을 수정해야 합니다.  
소스파일 위치는 ./vendor/jurosh/pdf-merge/src/Jurosh/PDFMerge/PDFMerger.php 입니다.  

PDFMerger.php 파일에서 파일 합치기 함수 merge를 찾습니다.
``` php

<? php

  /* ./vendor/jurosh/pdf-merge/src/Jurosh/PDFMerge/PDFMerger.php */
  
  /*== 기존 ==*/ 
  public function merge($outputmode = 'browser', $outputpath = 'newfile.pdf') {

    /* ====*/
    /* 중략 */
    /* ====*/

    if ($mode == 'S') {
        return $fpdi->Output($outputpath, 'S');
    } else {
        if ($fpdi->Output($outputpath, $mode) == '') {
      return true;
        } else {
      throw new Exception("Error outputting PDF to '$outputmode'.");
      return false;
        }
    }
  }

  /*== 수정 ==*/ 
  public function merge($outputmode = 'browser', $outputpath = 'newfile.pdf') {

    /* ====*/
    /* 중략 */
    /* ====*/

    if ($mode == 'S') {
        return $fpdi->Output($outputpath, 'S', true);
    } else {
        if ($fpdi->Output($outputpath, $mode, true) == '') {
      return true;
        } else {
      throw new Exception("Error outputting PDF to '$outputmode'.");
      return false;
        }
    }
  }
?>
```

<br />
위에서 바뀐 부분은 Output 함수의 파라미터 하나입니다.
``` php

<? php
  // 기존
  $fpdi->Output($outputpath, 'S');
  $fpdi->Output($outputpath, $mode);
  // 수정
  $fpdi->Output($outputpath, 'S', true);
  $fpdi->Output($outputpath, $mode, true);
?>
```

<br />
왜 그런지 실제 Output 함수가 선언된 곳을 찾아가보겠습니다.
파일 위치는 ./vendor/setasign/fpdf/fpdf.php 입니다.  
아래와 같이 Output 함수의 파라미터는 3개로 마지막이 UTF-8 여부를 설정하는 것이었습니다.  
그러므로 마지막 파라미터를 true 로 설정만 해도 한글, UTF-8 문제가 해결됩니다.

``` php

<? php

  /* ./vendor/setasign/fpdf/fpdf.php */

  function Output($dest='', $name='', $isUTF8=false)
  {
	/* 중략 */
  }
?>
```

## 문서 속성 추가
또 하나의 문제가 제목, 작성자, 주제 등의 문서 속성 정보가 추가되지 않는다는 점입니다.  
이 또한 파일 2개를 수정해야 합니다.


#### 문서 합치기 파일 (ex: pdf_merge.php)
``` php

<? php
  // 문서 속성 정보를 추가합니다.
  $author = "써드아이시스템";
  $creator = "써드아이시스템";
  $title = "문서 제목";
  $subject = "문서 주제";
  $keywords = "키워드";

  // merge 함수에 문서 속성 옵션을 추가합니다.
  $pdf->merge('download', $output_filename, $author, $creator, $title, $subject, $keywords);
?>
```
<br />
####  merge 함수 정의 파일 (PDFMerger.php)
위치 : ./vendor/jurosh/pdf-merge/src/Jurosh/PDFMerge/PDFMerger.php

``` php

<? php

  /* ./vendor/jurosh/pdf-merge/src/Jurosh/PDFMerge/PDFMerger.php */

  // 파라미터 추가
  public function merge($outputmode = 'browser', $outputpath = 'newfile.pdf', $author = '3rdEYESYS', $creator = '3rdEYESYS', $title = 'title', $subject = 'subject', $keywords = 'keywords') {
    
    /* 중략 */

    // 속성 항목 설정
    $fpdi->SetAuthor($author, true);
    $fpdi->setCreator($creator, true);
    $fpdi->setTitle($title, true);
    $fpdi->setSubject($subject, true);
    $fpdi->setKeywords($keywords, true);
  }
?>
```

문서 속성을 저렇게 설정하면 되는 이유는 해당 함수가 정의된 곳을 찾아보면 알 수 있습니다.

파일 위치는 ./vendor/setasign/fpdf/fpdf.php 입니다.  

``` php

<? php

  /* ./vendor/setasign/fpdf/fpdf.php */

  function SetTitle($title, $isUTF8=false)
  {
    // Title of document
    $this->metadata['Title'] = $isUTF8 ? $title : utf8_encode($title);
  }

  function SetAuthor($author, $isUTF8=false)
  {
    // Author of document
    $this->metadata['Author'] = $isUTF8 ? $author : utf8_encode($author);
  }

  function SetSubject($subject, $isUTF8=false)
  {
    // Subject of document
    $this->metadata['Subject'] = $isUTF8 ? $subject : utf8_encode($subject);
  }

  function SetKeywords($keywords, $isUTF8=false)
  {
    // Keywords of document
    $this->metadata['Keywords'] = $isUTF8 ? $keywords : utf8_encode($keywords);
  }

  function SetCreator($creator, $isUTF8=false)
  {
    // Creator of document
    $this->metadata['Creator'] = $isUTF8 ? $creator : utf8_encode($creator);
  }
?>
```

## 참고 URL
1.  PDF Merge GitHub Source and Download
	- <a href="https://github.com/jurosh/php-pdf-merge" target="_blank" style="word-break:break-all;">https://github.com/jurosh/php-pdf-merge</a>


> 문서 최종 수정일 : 2021-05-03
