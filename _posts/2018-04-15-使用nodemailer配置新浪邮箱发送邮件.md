---
layout: post
title: 使用nodemailer配置新浪邮箱发送邮件
publised: false
categroy: [nodejs, nodemailer]
---

## 安装nodemailer
>npm install nodemailer


## 新浪邮箱配置
>var Mailder = require('nodemailer')
// 开启一个 SMTP 连接池
var transport = Mailder.createTransport({
    host: "smtp.sina.cn", // 主机
    port: 25, // SMTP 端口
    auth: {
        user: "myemail@sina.cn", // 账号，以下用myemail代替
        pass: "******" // 密码
    }
});
// 设置邮件内容
var mailOptions = {
    from: "myemail<myemail@sina.cn>", // 发件地址
    to: "youremail@sina.cn", // 收件列表
    subject: "Hello world", // 标题
    text:"hello",
    html: "<b>thanks a for visiting!</b>" // html 内容
}
class Mail {
    static sendMail () {
        // 发送邮件
        transport.sendMail(mailOptions, function(error, response) {
            if (error) {
                console.error(error);
                console.log("send mail fail")
            } else {
                console.log(response);
                console.log("send mail ok")
            }
            transport.close(); // 如果没用，关闭连接池
        });
    }
}
module.exports = Mail

## nodejs中使用

>var Mail = require('../src/services/mail_service')
>Mail.sendMail()

## 新浪邮箱的坑
配置好以后一直报错，smtp验证失败，报错信息如下
>code: 'EAUTH',
  response: '535 5.7.12 SMTP access disabled',
  responseCode: 535,
  command: 'AUTH PLAIN'

>535	1. Error: Authentication Failed 2. Authentication Unsuccessful	1. 错误讯息：验证失败 2. 验证不成功
寄信端邮件服务器为了要防止垃圾信做出传递邮件的限制。	可请邮递员设定SMTP AUTH的认证或是限定某个IP地址才可寄信的方式。

解决方法: 登陆新浪邮箱->设置区->客户端pop/imap/smtp->将POP3/SMTP服务和IMAP4服务/SMTP服务的服务状态都修改为“开启”，DONE
