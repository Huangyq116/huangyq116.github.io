---
title: PHP之SMTP发送邮件
permalink: 
date:   2018-06-07 15:24:00
categories:
  - 2.Programming
  - PHP
tags:
  - PHP
---

## 1、下载相关库

class.phpmailer.php和class.smtp.php至公共库

## 2、编写公共函数

```php
function sendMail($param) {
        $config = C('THINK_EMAIL');
        vendor('PHPMailer.class#phpmailer'); //从PHPMailer目录导class.phpmailer.php类文件
        $mail = new PHPMailer(); //PHPMailer对象
        $mail->CharSet = $config['EMAIL_CHARSET']; //设定邮件编码，默认ISO-8859-1，如果发中文此项必须设置，否则乱码
        $mail->IsSMTP();  // 设定使用SMTP服务
        $mail->SMTPDebug = 0;   // 关闭SMTP调试功能
        // 1 = errors and messages
        // 2 = messages only
        $mail->SMTPAuth = $config['EMAIL_SMTPAUTH'];   // 启用 SMTP 验证功能
        $mail->Host = $config['SMTP_HOST'];  // SMTP 服务器
        $mail->Port = $config['SMTP_PORT'];  // SMTP服务器的端口号
        $mail->Username = $config['SMTP_USER'];  // SMTP服务器用户名
        $mail->Password = $config['SMTP_PASS'];  // SMTP服务器密码
        //$mail->SetFrom($config['FROM_EMAIL'], $config['FROM_NAME']);
        $mail->SetFrom($param['mail_from'], $param['mail_name']);
        $replyEmail = $config['REPLY_EMAIL'] ? $config['REPLY_EMAIL'] : $param['mail_from'];
        $replyName = $config['REPLY_NAME'] ? $config['REPLY_NAME'] : $param['mail_name'];
        $mail->AddReplyTo($replyEmail, $replyName);

        if (!empty($param['to'])) {
            foreach ($param['to'] as $to) {
                $mail->AddAddress($to['address'], $to['name']);
            }
        }
        if (!empty($param['cc'])) {
            foreach ($param['cc'] as $cc) {
                $mail->addCC($cc['address'], $cc['name']);
            }
        }
//        if (!empty($param['bcc'])) {
//            foreach ($param['bcc'] as $bcc) {
//                $mail->addBCC($bcc['address'], $bcc['name']);
//            }
//        }

        $param['body'] = $mail->WrapText($param['body'], 900);
        $mail->Subject = $param['subject'];
        if (!empty($param['body'])) {
            $mail->MsgHTML($param['body']);
            $mail->IsHTML($config['EMAIL_ISHTML']);
            $mail->Body = $param['body'];
        }

//        if (!empty($param['attachment'])) { // 添加附件
//            foreach ($param['attachment'] as $file) {
//                if (is_file($file['path'])) {
//                    $mail->AddAttachment($file['path'], $file['name']);
//                }
//            }
//        }

        for($i=0;$i<(count($param['attachment']));$i++){
            $img=substr($param['attachment'][$i], strpos($param['attachment'][$i], ","));
            $mail->AddStringAttachment(base64_decode($img),"attach".$i.".png","base64","image/png");

        }

        //重发机制
        $ret['errno'] = 0;
        $ret['msg'] = '';
        if ($mail->Send()) {
            return $ret;
        } else {
            if ($mail->Send()) {
                return $ret;
            } else {
                $ret['errno'] = 1;
                $ret['msg'] = $mail->ErrorInfo;
                return $ret;
            }
        }
        // return $mail->Send() ? true : $mail->ErrorInfo;
    }
```

## 3、SMTP配置函数

```php
// 配置邮件发送服务器
'THINK_EMAIL'=>array(
    'SMTP_HOST'   =>  'localhost',  //邮件发送SMTP服务器
    'SMTP_PORT'   => '25',//SMTP服务器端口  
    'SMTP_USER'   =>  'admin', //SMTP服务器登陆用户名
    'SMTP_PASS'   =>  'admin', //SMTP服务器登陆密码 
    'FROM_EMAIL'  =>'发件箱@XX.com',
    'FROM_NAME'  =>'发件人姓名',
    'REPLY_EMAIL' =>'',
    'REPLY_NAME'  =>'',
    'EMAIL_CHARSET' =>'utf-8',
    'EMAIL_ISHTML' => 'TRUE',
    'EMAIL_SMTPAUTH' => '0',
    ),
```

## 4、根据库中存取的base64获取图片信息，实际调用为一个url

```php
public function getImage() {
    $reportId = I('request.id');
    $imgInfos = I('request.img');
    header('Content-Type: image/png');
    $repotModel = M('XXX');
    $report = $repotModel->where(['id'=>$reportId])->find();
    $base = explode(',', $report[$imgInfos])[1];
    $base = base64_decode($base);
    echo $base;
    die();
}
```

