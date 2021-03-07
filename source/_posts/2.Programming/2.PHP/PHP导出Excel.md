---
title: PHP导出Excel
permalink: 
date: 2018-06-07 14:56:00
categories:
  - 2.Programming
  - PHP
tags:
  - PHP
---

## 1、引入相关公共库PHPExcel

## 2、编写公共函数

```php
public function exportExcel($excelTitle,$data,$filename='',$column_width=''){
        ini_set('max_execution_time', '0');
        header("Content-Type:application/force-download");
        header("Content-Type:application/vnd.ms-execl");
        header("Content-Type:application/download");
        $filename=str_replace('.xls', '', $filename).'.xls';
        header('Content-Disposition: attachment;filename='.$filename);
        Vendor('PHPExcel.PHPExcel');
        $phpexcel = new PHPExcel();
        $phpexcel->getActiveSheet()->setTitle('Sheet1');

        $key = 0;  //设置表头
        foreach($excelTitle as $v){
            $colum = \PHPExcel_Cell::stringFromColumnIndex($key);
            $phpexcel->setActiveSheetIndex(0) ->setCellValue($colum.'1', $v);
            $key += 1;
        }
        $column = 2;
        $objActSheet = $phpexcel->getActiveSheet();

        foreach($data as $key => $rows){ //行写入
            $span = 0;
            foreach($rows as $keyName=>$value){// 列写入
                $j = \PHPExcel_Cell::stringFromColumnIndex($span);
                $objActSheet->setCellValue($j.$column, $value);
                $span++;
            }
            $column++;
        }

        //设置字体格式及大小
        $phpexcel->getDefaultStyle()->getFont()->setName( '微软雅黑');
        $phpexcel->getDefaultStyle()->getFont()->setSize(10);

        //第一行设置填充的样式和背景色
        $currentColumn = 'A';
        for ($i = 1; $i <= count($excelTitle); $i++) {
            $a[] = $currentColumn++;
        }
        $last = $a[(count($a) -1)]."1";
        $phpexcel->getActiveSheet()->getStyle( "A1:$last")->getFill()->setFillType(\PHPExcel_Style_Fill::FILL_SOLID);
        $phpexcel->getActiveSheet()->getStyle( "A1:$last")->getFill()->getStartColor()->setARGB('#458B00');

        //设置单元格内容居左显示
        //$phpexcel->getActiveSheet()->getStyle('A')->getAlignment()->setHorizontal(\PHPExcel_Style_Alignment::HORIZONTAL_JUSTIFY);
        $phpexcel->getDefaultStyle()->getAlignment()->setHorizontal(\PHPExcel_Style_Alignment::HORIZONTAL_JUSTIFY);
        $phpexcel->getDefaultStyle()->getAlignment()->setVertical(\PHPExcel_Style_Alignment::HORIZONTAL_JUSTIFY);

        //设置列宽度
        if(!empty($column_width)){
            foreach($column_width as $key => $value){
                $phpexcel->getActiveSheet()->getColumnDimension($key)->setWidth($value);
            }
        }

        $objwriter = PHPExcel_IOFactory::createWriter($phpexcel, 'Excel5');
        $objwriter->save('php://output');
        exit;
    }
```

## 3、调用公共函数

```php
$excelTitle = array("提测单名称","BUG统计周期","BUG_TOTAL","ASSIGNED","REOPENED","RESOLVED","VERIFIED","CLOSED");
foreach ($retSearchStatusList as $excelExport){
    $excelExports['A'] = $excelExport['A'];
    $excelExports['B'] = $dateFrom."至".$dateTo;
    $excelExports['C'] = $excelExport['C'];
    $excelExports['D'] = $excelExport['D'];
    $excelExports['E'] = $excelExport['E'];
    $excelExports['F'] = $excelExport['F'];
    $excelExports['G'] = $excelExport['G'];
    $excelExports['H'] = $excelExport['H'];
    $list[] = $excelExports;
}
$filename = "文件名称-".date('Y-m-d H-i-s');
$column_width = array("A"=>50,"B"=>38);
$exportExcel = $excelUtil->exportExcel($excelTitle,$list,$filename,$column_width);
```

