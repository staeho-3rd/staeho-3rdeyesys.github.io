---
date: 2021-04-13
title: PhpSpreadsheet 설정 샘플 코드
categories:
  - 99.ETC
description: PhpSpreadsheet 설정 샘플 코드를 정리한 문서입니다.
type: Document
set: PhpSpreadsheet
order_number: 1
---

## 개요


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


## 스타일 지정

``` php

<?php

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
  $spreadsheet->getActiveSheet()->getStyle("B40")->applyFromArray($styleArray_Cell);

  $spreadsheet->getActiveSheet()->getStyle("G45:G46")->applyFromArray($styleArray_Cell);

  $spreadsheet->getActiveSheet()->getStyle("B80")->getFont()->setSize(13)->setBold(true);
	
?>
```
	$spreadsheet->setActiveSheetIndex(0);
	
	// PDF로 저장
	IOFactory::registerWriter('Pdf', \PhpOffice\PhpSpreadsheet\Writer\Pdf\Mpdf::class);

	$out_put_file_full_name = "sample.pdf";

	// Redirect output to a client’s web browser (PDF)
	header('Content-Type: application/pdf; charset=utf-8' );
	header('Content-Disposition: attachment;filename="'.$out_put_file_full_name.'"');
	header('Cache-Control: max-age=0');

	$writer = IOFactory::createWriter($spreadsheet, 'Pdf');
	$writer->save('php://output');
	exit;

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