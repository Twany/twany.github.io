---
layout: post
title: "phpexcel的上传和下载"
subtitle: "tp5.1使用phpexcel——上传与下载"
author: "Twany"
header-img: "img/bg-scene/psot-bg-ten.jpg"
header-mask: 0.4
catalog: true
tags:
  - php
  - phpExcel
  - thinkPHP
---

## 将上传的phpexcel数据传到数据库

```
	//处理上传的文件数据
	public function doUpload()
	{
		
		$data = input();
		$file = request()->file('excel');
		//dump($data);
		
		//考试批次
		$batch = $data['batch'];
		
		//班级
		$class = $data['class'];
		
		//科目
		$subject = $data['subject'];
		
		$info =  $file->validate(['size'=>1048576,'ext'=>'xlsx,xls'])->move('/public/upload/excel');
		if($info){
			//再batch表创建对应批次
			$db = Db::table('batch')->insert(['batch'=>$batch]);
			
			//加载指定文件
			$fileName = '/public/upload/excel/'.$info->getSaveName();

			$phpExcelObj = \PHPExcel_IOFactory::load($fileName);
			$sheet = array();
			$sheet[] = $phpExcelObj->getSheet(0)->toArray();
			
			$name = '';
			//dump($sheet[0]);
			//把表格数据遍历为数组
			foreach($sheet[0] as $key=>$value){
				
				if($key!=0){
					foreach($value as $k=>$val){
						if($k == 0){
							$name = $val;
						}
						if($k == 1){
							$score = $val;
						}
						
					}
					
					//上传到数据库
					GradeModel::create(['name'=>$name,'score'=>$score,'batch'=>$batch,'class'=>$class,'subject'=>$subject]);
					//echo $name.$score.$batch.$class.'<br>';
				}
				
			}
			$this->view->assign('class',$class);
			$this->view->assign('batch',$batch);
			$this->view->assign('subject',$subject);
			return $this->view->fetch();

		}else{
			// 上传失败获取错误信息
			//echo $file->getError();
			return ['status'=>0,'msg'=>'上传失败'];
		}	
	}
```


下载

```
	//下载Excel模板
	public function product()
	{
		$excel = new \PHPExcel();
		

		//2.指定当前的sheet
		$objSheet = $excel->setActiveSheetIndex(0);
		
		//写入值
		$objSheet->setCellValue('A1', '姓名');
		$objSheet->setCellValue('B1', '分数');


		//4.保存文件
		$objWriter = \PHPExcel_IOFactory::createWriter($excel, 'Excel2007');
		//告诉浏览器以xlsx文件输出
		
		header('Content-Type: application/vnd.openxmlformats-officedocument.spreadsheetml.sheet');
		//Content-Disposition 消息头指示输出内容该以何种形式展示， attachment：消息体应该被下载到本地
		//filename的值预填为下载后的文件名
		
		header('Content-Disposition: attachment;filename="01simple.xlsx"');
		//设置缓存存储的最大周期，超过这个时间缓存被认为过期(单位秒),
		//max-age=0 即不缓存
		
		header('Cache-Control: max-age=0');
		
		$objWriter->save('php://output');
	}
```

 ### 总结
 文件的上传主要是上传文件->移动文件->获取文件信息，处理文件数据

 文件的下载重点在于浏览器header的更改。

 教程学习：[51cto](http://edu.51cto.com/center/course/lesson/index?id=259283)