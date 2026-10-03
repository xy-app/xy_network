# 邮件发送 (SendEmail)

通过 SMTP 协议或邮件服务 API 发送邮件，支持抄送、密送、HTML 格式与多附件。

## 运行参数

* **服务类型 (`service_type`)**
  * 类型: `string`
  * 默认值: `"SMTP"`
  * 描述: 邮件发送协议通道类型。可选值：`SMTP`, `Resend (API)`。
* **快捷服务商预设 (`provider_preset`)**
  * 类型: `string`
  * 默认值: `"Custom"`
  * 描述: 快速预填服务器配置的厂商模版。可选值：`Custom`, `Gmail`, `Resend (SMTP)`, `Resend (API)`, `QQ`, `163`, `Outlook`。
* **SMTP 服务器地址 (`smtp_host`)**
  * 类型: `string`
  * 默认值: `"smtp.qq.com"`
  * 描述: 发件服务器主机名或 IP。
* **SMTP 端口 (`smtp_port`)**
  * 类型: `number`
  * 默认值: `465`
  * 描述: 发件服务器通信端口号。
* **传输加密方式 (`encryption`)**
  * 类型: `string`
  * 默认值: `"SSL/TLS"`
  * 描述: 连接发信服务器的安全加密模式。可选值：`SSL/TLS`, `STARTTLS`, `None`, `Auto`。
* **API 密钥 (用于云服务通道) (`api_key`)**
  * 类型: `string`
  * 默认值: `""`
  * 描述: 使用第三方云邮件 API 时填写的通信密钥。
* **发信账号 (Username) (`username`)**
  * 类型: `string`
  * 默认值: `""`
  * 描述: 发信邮箱的登录账号。
* **密码/应用授权码 (`password`)**
  * 类型: `string`
  * 默认值: `""`
  * 描述: 邮箱密码或生成的第三方应用专用授权码。
* **发件人邮箱 (From) (`from`)**
  * 类型: `string`
  * 默认值: `""`
  * 描述: 显示的完整发件人邮箱地址。
* **收件人邮箱 (To) (`to`)**
  * 类型: `string`
  * 默认值: `""`
  * 描述: 收件人邮箱列表 (多个用逗号或分号分隔)。
* **抄送邮箱 (Cc) (`cc`)**
  * 类型: `string`
  * 默认值: `""`
  * 描述: 抄送收件人邮箱列表。
* **密送邮箱 (Bcc) (`bcc`)**
  * 类型: `string`
  * 默认值: `""`
  * 描述: 密送收件人邮箱列表。
* **回复地址 (Reply-To) (`reply_to`)**
  * 类型: `string`
  * 默认值: `""`
  * 描述: 收件人回复邮件时的接收地址。
* **邮件主题 (`subject`)**
  * 类型: `string`
  * 默认值: `""`
  * 描述: 邮件标题摘要。
* **邮件正文 (`body`)**
  * 类型: `string`
  * 默认值: `""`
  * 描述: 邮件正文文本内容或 HTML 源代码。
* **正文格式 (`body_type`)**
  * 类型: `string`
  * 默认值: `"Plain"`
  * 描述: 邮件内容排版格式：纯文本 (`Plain`) 或 HTML 富文本 (`HTML`)。
* **附件文件路径 (`attachments`)**
  * 类型: `string`
  * 默认值: `""`
  * 描述: 本地附件文件完整路径列表 (多个用逗号或换行分隔)。
* **超时时间 (秒) (`timeout_secs`)**
  * 类型: `number`
  * 默认值: `30`
  * 描述: 发信网络交互的最大等待秒数。
* **忽略 SSL 证书错误 (`ignore_ssl_errors`)**
  * 类型: `bool`
  * 默认值: `false`
  * 描述: 是否跳过服务器证书安全验证。

## 输出

* 类型: `string`
* 描述: 邮件发送结果提示或回执 ID。

## 使用示例

```json
{
  "tag": "SendEmail",
  "service_type": "SMTP",
  "provider_preset": "QQ",
  "smtp_host": "smtp.qq.com",
  "smtp_port": 465,
  "encryption": "SSL/TLS",
  "username": "user@qq.com",
  "password": "authorization_code",
  "from": "user@qq.com",
  "to": "target@example.com",
  "subject": "自动化任务通知",
  "body": "<h1>任务执行成功</h1>",
  "body_type": "HTML",
  "attachments": "",
  "timeout_secs": 30,
  "ignore_ssl_errors": false
}
```