[TOC]

# nodemailer

nodemailer 是一个简单易用的 Node.JS 邮件发送模块（通过 SMTP，sendmail，或者 Amazon SES）

[官方文档](https://nodemailer.com/about/)



## 第一步、修改qq邮箱设置

登录qq邮箱，点击进入 `设置-账户-POP3/IMAP/SMTP/Exchange/CardDAV/CalDAV` 服务，开启SMTP服务。发送短信到指定号码后，获取授权码。



## 第二步、编写发送邮件调用函数

**注意：`nodemailer.createTransport` 中的 `host` 修改成 `smtp.qq.email` ，然后使用第一步获得的授权码，填入到 `auth.pass` 中。**

修改 `transporter.sendMail` 中的参数，即可修改邮件的发送者、接收者、主题、正文等。

代码示例：

```js
"use strict";
const nodemailer = require("nodemailer");

// async..await is not allowed in global scope, must use a wrapper
async function main() {
  // Generate test SMTP service account from ethereal.email
  // Only needed if you don't have a real mail account for testing
  // let testAccount = await nodemailer.createTestAccount();

  // create reusable transporter object using the default SMTP transport
  let transporter = nodemailer.createTransport({
    host: "smtp.qq.email",
    port: 587,
    secure: false, // true for 465, false for other ports
    auth: {
      // user: testAccount.user, // generated ethereal user
      // pass: testAccount.pass, // generated ethereal password
      user: '872990547@qq.com', // 邮箱
      pass: 'ytseggkpephvbegh', // 非密码，而是授权码
    },
  });

  // send mail with defined transport object
  let info = await transporter.sendMail({
    from: '"Fred Foo 👻" <foo@example.com>', // sender address
    to: "bar@example.com, baz@example.com", // list of receivers
    subject: "Hello ✔", // Subject line
    text: "Hello world?", // plain text body
    html: "<b>Hello world?</b>", // html body
  });

  console.log("Message sent: %s", info.messageId);
  // Message sent: <b658f8ca-6296-ccf4-8306-87d57a0b4321@example.com>

  // Preview only available when sending through an Ethereal account
  console.log("Preview URL: %s", nodemailer.getTestMessageUrl(info));
  // Preview URL: https://ethereal.email/message/WaQKMgKddxQDoou...
}

main().catch(console.error);
```

