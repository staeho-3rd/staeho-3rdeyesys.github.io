---
date: 2021-04-13
last_modified_at: 2021-04-13
title: PhpSpreadsheet 설정 샘플 코드
categories:
  - 99.ETC
description: PhpSpreadsheet 설정 샘플 코드를 정리한 문서입니다.
type: Document
set: PhpSpreadsheet
order_number: 1
---

## 개요
PHP에서 Excel 문서를 읽거나 Excel 형태로 문서를 저장해야 할 때 주로 PhpSpreadsheet를 사용하게 됩니다. 
이전에는 PHPExcel이라는 이름이었으나 현재는 해당 버전의 개발-유지보수가 중단되고 새로운 프로젝트로 PhpSpreasheet라는 이름으로 업그레이드 되었습니다.  
또한 PhpSpreadsheet는 Excel 뿐만 아니라 PDF 형식으로도 파일을 저장할 수 있어서 많이 사용되고 있습니다.   
이번에는 이 PhpSpreadsheet로 문서를 저장할 때 사용하는 주요 소스 코드의 샘플을 정리해보겠습니다.

## 클래스 로드

``` php

<?php

	require 'vendor/autoload.php';

	use PhpOffice\PhpSpreadsheet\IOFactory;
	use PhpOffice\PhpSpreadsheet\Spreadsheet;

	$spreadsheet = new Spreadsheet();
?>
```

## 기본 설정

``` php

<? php

 // 문서 가로,세로 방향 설정	
 $spreadsheet->getActiveSheet()->getPageSetup()
 ->setOrientation(\PhpOffice\PhpSpreadsheet\Worksheet\PageSetup::ORIENTATION_PORTRAIT);

 // 문서 사이즈 설정
 $spreadsheet->getActiveSheet()->getPageSetup()
 ->setPaperSize(\PhpOffice\PhpSpreadsheet\Worksheet\PageSetup::PAPERSIZE_A4);

 // 문서 정보 설정
 $spreadsheet->getProperties()->setCreator("써드아이시스템")
 ->setLastModifiedBy("써드아이시스템")
 ->setTitle("문서 타이틀")
 ->setSubject("문서 제목")
 ->setDescription("문서 설명")
 ->setKeywords("키워드")
 ->setCategory("카테고리");


 // 문서 좌우 여백 설정
 $spreadsheet->getActiveSheet()->getPageMargins()->setTop(0.5);	
 $spreadsheet->getActiveSheet()->getPageMargins()->setBottom(0.25);
 $spreadsheet->getActiveSheet()->getPageMargins()->setRight(0.75);
 $spreadsheet->getActiveSheet()->getPageMargins()->setLeft(0.75);
?>
```

## 셀 값 설정

``` php

<? php	
  $spreadsheet->setActiveSheetIndex(0)->setCellValue("C11", $value);
  $spreadsheet->setActiveSheetIndex(0)->setCellValue("B25", "셀 값 설정\r\n다음 줄");
?>
```

## 셀에 이미지 설정

``` php

<? php
  $drawing_logo = new \PhpOffice\PhpSpreadsheet\Worksheet\Drawing();	
  $drawing_logo->setName('Logo');
  $drawing_logo->setDescription('3rdEYESYS Logo');
  $drawing_logo->setPath('/ncp/data/www/img/3rdeyesys_logo.png');
  $drawing_logo->setCoordinates('A1');
  $drawing_logo->setWidth(230);
  $drawing_logo->getShadow()->setVisible(true);
  $drawing_logo->setWorksheet($spreadsheet->getActiveSheet());
?>
```

## 셀 병합

``` php

<? php	
  // 동일한 행에서 병합
  $spreadsheet->getActiveSheet()->mergeCells("A1:D1");

  // 여러 행에서 병합
  $spreadsheet->getActiveSheet()->mergeCells("G5:I7");

  // 변수 사용
  for ($j = 52; $j <= 57; $j++)
  {
    $spreadsheet->getActiveSheet()->mergeCells("B".$j.":C".$j);
  }
?>
```	

## 행 높이 설정	
``` php

<?php

  for ($j = 1; $j <= 5; $j++)
  {
    $spreadsheet->getActiveSheet()->getRowDimension($j)->setRowHeight(12);
  }

  $spreadsheet->getActiveSheet()->getRowDimension(10)->setRowHeight(35);
?>
```

## 열 너비 설정

``` php

<?php

  // 특정 열 너비 설정
  $spreadsheet->getActiveSheet()->getColumnDimension("A")->setWidth(3);

  // 범위 내 여러 열 너비 설정
  foreach(range("B","J") as $columnID) 
  {
    $spreadsheet->getActiveSheet()->getColumnDimension($columnID)->setWidth(15);
  }
?>
```


## 셀 스타일 지정

``` php

<?php

  // 셀 스타일을 배열 형식으로 저장
  // 순서대로 border, font, 배경색 지정하는 스타일
  $styleArray_Cell = [
    'borders' => [
      'allBorders' => [
        'borderStyle' => \PhpOffice\PhpSpreadsheet\Style\Border::BORDER_THIN,
        'color' => ['argb' => 'FF20AFA5']
      ]
    ],
    'font' => [
      'bold' => true,
      'size' => 9,
      'color'    => ['argb' => 'FFFFFFFF'],
    ],
    'fill' =>[
      'fillType' => \PhpOffice\PhpSpreadsheet\Style\Fill::FILL_SOLID,
      'color' => ['argb' => 'FF20AFA5']
    ]
  ];

  // 특정 셀에 스타일 적용
  $spreadsheet->getActiveSheet()
  ->getStyle("B40")->applyFromArray($styleArray_Cell);
  
  // 특정 범위의 여러 셀에 스타일 동시 적용
  $spreadsheet->getActiveSheet()
  ->getStyle("G45:G46")->applyFromArray($styleArray_Cell);

  // 특정 셀에 폰트 스타일 직접 적용
  $spreadsheet->getActiveSheet()
  ->getStyle("B80")->getFont()->setSize(13)->setBold(true);
	
?>
```

## PDF 로 저장

``` php

<?php
  $spreadsheet->setActiveSheetIndex(0);

  // Mpdf 클래스 별로 설치해야 함
  IOFactory::registerWriter('Pdf', \PhpOffice\PhpSpreadsheet\Writer\Pdf\Mpdf::class);

  $out_put_file_full_name = "sample.pdf";

  // Redirect output to a client’s web browser (PDF)
  header('Content-Type: application/pdf; charset=utf-8' );
  header('Content-Disposition: attachment;filename="'.$out_put_file_full_name.'"');
  header('Cache-Control: max-age=0');

  $writer = IOFactory::createWriter($spreadsheet, 'Pdf');
  $writer->save('php://output');
  exit;
?>
```
PhpSpreadsheet를 사용해서 pdf 파일을 저장하려면 Mpdf 를 추가로 설치해야 합니다.   
이와 관련된 내용은 다음 문서에서 다시 정리하겠습니다.

## Excel로 저장

``` php

<?php
  $spreadsheet->setActiveSheetIndex(0);

  // Redirect output to a client’s web browser (Excel2007)
  $out_put_file_full_name = "sample.xlsx";

  header('Content-Type: application/vnd.openxmlformats-officedocument.spreadsheetml.sheet');
  header('Content-Disposition: attachment;filename="'.$out_put_file_full_name.'"');
  header('Cache-Control: max-age=0');

  // If you're serving to IE over SSL, then the following may be needed
  header ('Expires: Mon, 26 Jul 2022 05:00:00 GMT'); // Date in the past
  header ('Last-Modified: '.gmdate('D, d M Y H:i:s').' GMT'); // always modified
  header ('Cache-Control: cache, must-revalidate'); // HTTP/1.1
  header ('Pragma: public'); // HTTP/1.0

  $writer = IOFactory::createWriter($spreadsheet, 'Xlsx');	
  $writer->save('php://output');
  exit;
?>
```

## 참고 URL
1.  PhpSpreadsheet Documentation
	- <a href="https://phpspreadsheet.readthedocs.io/" target="_blank" style="word-break:break-all;">https://phpspreadsheet.readthedocs.io/</a>
2.  PhpSpreadsheet GitHub Source and Download
	- <a href="https://github.com/PHPOffice/PhpSpreadsheet" target="_blank" style="word-break:break-all;">https://github.com/PHPOffice/PhpSpreadsheet</a>
