# 发送邮件 (SendEmail)

## 1. 功能概述
`SendEmail` 算子是工业级自动化流程中的邮件通信组件，基于成熟稳定的底层协议引擎构建，提供标准 **SMTP 协议**与 **Resend 官方 HTTPS REST API** 双通道投递能力。原生支持 Google (Gmail)、Resend、QQ 邮箱、网易 163 邮箱、Microsoft Outlook/Office 365 及各类企业自建邮件系统，支持 SSL/TLS 与 STARTTLS 加密、多收件人/抄送/密送、HTML 富文本渲染以及多个本地文件附件的自动识别与打包发送。

## 2. 核心特性与工业痛点解决
- **Google (Gmail) 原生适配**：
  - 完美支持 Google 官方推荐的 `smtp.gmail.com:465` (SSL/TLS) 或 `587` (STARTTLS) 加密传输通道。
  - 针对 Google 强制开启的两步验证机制，原生适配 **应用专用密码 (App Password)** 授权，免去繁琐的 OAuth2 授权拦截，实现无人值守自动化投递。
- **Resend 双通道保障（彻底解决云服务器端口封锁痛点）**：
  - **SMTP 通道**：`smtp.resend.com:465`，用户名填 `resend`，密码填入 Resend API Key (`re_...`)。
  - **HTTPS REST API 通道**：直接调用官方 `POST https://api.resend.com/emails`，走标准 **443 端口**。完美解决腾讯云、阿里云、AWS 等云主机或受限企业内网默认封禁出站 25/465/587 端口的行业痛点，连通率 100%。
- **多附件智能流式封包**：
  - 支持在 `attachments` 中指定多个本地文件路径（用英文逗号 `,`、分号 `;` 或换行分隔）。
  - 引擎基于 `mime_guess` 自动解析文件扩展名（如 `.pdf`, `.xlsx`, `.zip`, `.png`, `.log` 等）并匹配对应的标准 MIME Content-Type，自动组装为 RFC 兼容的标准 MultiPart 邮件数据流。
- **HTML 富文本与纯文本双格式**：
  - 支持 `Plain`（纯文本）与 `HTML`（富文本格式），在自动化运维或业务监控中可直接发送带有表格、色块高亮及超链接的精美排版邮件。
- **自建邮箱与内网证书穿透**：
  - 提供 `ignore_ssl_errors` 选项，可在使用企业私有部署的自签名或内部 CA 证书的邮件服务器时跳过阻断，确保内网业务稳定执行。
- **架构解耦与跨模块复用**：
  - 核心通信能力下沉至 `rust_binding::model::email`，除 RPA 流程动作外，系统级用户反馈（`feedback_controller`）、接口服务（`xy_https`）与账号验证码系统均可直接复用底层引擎。

## 3. 平台支持与权限
- **支持平台**：`Windows` / `Linux` / `macOS` (跨平台通用，基于纯 Rust `rustls-tls`，无平台 C 库依赖)
- **管理员权限**：`否` (标准普通用户权限即可)

## 4. 参数说明

| 属性名称 | 参数类型 | 默认值 | 说明 |
| :--- | :--- | :--- | :--- |
| `service_type` | `String` | `"SMTP"` | 传输通道类型：`"SMTP"`（通用协议）或 `"Resend (API)"`（443 端口 REST API） |
| `provider_preset`| `String` | `"Custom"` | 快捷服务商预设模板：`"Custom"`, `"Gmail"`, `"Resend (SMTP)"`, `"Resend (API)"`, `"QQ"`, `"163"`, `"Outlook"`，选择后自动匹配对应服务器与加密参数 |
| `smtp_host` | `String` | `"smtp.qq.com"` | SMTP 服务器主机地址（如 `smtp.gmail.com` 或 `smtp.resend.com`） |
| `smtp_port` | `Number` | `465` | SMTP 端口号：`465` (SSL/TLS 隐式加密), `587` (STARTTLS), `25` (明文) |
| `encryption` | `String` | `"SSL/TLS"` | 传输层加密模式：`"SSL/TLS"`, `"STARTTLS"`, `"None"`, `"Auto"` |
| `api_key` | `String` | `""` | Resend API Key（以 `re_` 开头，仅在 `service_type` 为 `Resend (API)` 时生效） |
| `username` | `String` | `""` | 登录邮箱账号（Gmail/QQ/163 填完整邮箱；Resend SMTP 固定填 `resend`） |
| `password` | `String` | `""` | 邮箱密码、SMTP 授权码或 Google 应用专用密码 |
| `from` | `String` | `""` | 发件人名称与地址（如 `DevOps <notice@company.com>`，留空默认使用 `username`） |
| `to` | `String` | `""` | 收件人邮箱地址，支持逗号 `,`、分号 `;` 或换行分隔多个收件人 |
| `cc` | `String` | `""` | 抄送人邮箱地址（可选，支持多个） |
| `bcc` | `String` | `""` | 密送人邮箱地址（可选，支持多个） |
| `reply_to` | `String` | `""` | 对方点击回复时的目标邮箱地址（可选） |
| `subject` | `String` | `""` | 邮件主题/标题 |
| `body` | `String` | `""` | 邮件正文（纯文本内容或完整 HTML 源码） |
| `body_type` | `String` | `"Plain"` | 正文格式：`"Plain"`（纯文本）或 `"HTML"`（富文本） |
| `attachments` | `String` | `""` | 本地附件文件路径，支持逗号、分号或换行分隔多个文件 |
| `timeout_secs` | `Number` | `30` | 网络连接与投递超时时间（秒） |
| `ignore_ssl_errors` | `Bool` | `false` | 是否忽略自签名或无效 SSL/TLS 证书校验 |

## 5. 输出变量与上下文
- **返回值**：标准格式 JSON 字符串，包含投递状态、分配的 Message-ID、收件人总数及耗时：
  ```json
  {
    "status": "success",
    "message_id": "<a1b2c3d4-e5f6@smtp.gmail.com>",
    "recipients_count": 2,
    "elapsed_ms": 320,
    "response_info": "250 2.0.0 OK 1741944883 - gsmtp"
  }
  ```
- **上下文变量**：
  - `email_status`：投递结果状态，成功为 `"success"`，失败为 `"failed"`。
  - `email_message_id`：分配的邮件消息唯一标识。
  - `email_elapsed_ms`：投递网络往返耗时（毫秒）。
  - `last_email_result`：包含完整结果的 JSON 字符串。
  - `last_email_error`：若投递失败，保存具体错误异常信息。

## 6. 主流邮箱服务商配置指引

### 6.1 Google (Gmail)
1. 登录 Google 账号，进入「管理您的 Google 账号」->「安全性」。
2. 确保已开启「两步验证 (2-Step Verification)」。
3. 在安全性页面搜索或进入「应用专用密码 (App Passwords)」。
4. 应用名称填写（例如 `XY-App`），系统将生成一组 16 位字符密码。
5. 在算子中配置：
   - `provider_preset` 选择 `Gmail`（或手动配置 `smtp_host: smtp.gmail.com`, `smtp_port: 465`, `encryption: SSL/TLS`）。
   - `username`：您的完整 Gmail 邮箱（如 `myname@gmail.com`）。
   - `password`：粘贴生成的 16 位应用专用密码。

### 6.2 Resend
- **方式 A：Resend (API) 模式（推荐，走 443 端口，免受防火墙阻断）**
  1. 登录 [resend.com](https://resend.com) 控制台，在 API Keys 页面创建并复制 API Key（形如 `re_123456789...`）。
  2. 在算子中将 `service_type` 设为 `Resend (API)`。
  3. `api_key` 填入您的 Key。
  4. `from`：填入已在 Resend 验证的域名邮箱（测试阶段可填 `onboarding@resend.dev`）。
- **方式 B：Resend (SMTP) 模式**
  - `smtp_host`: `smtp.resend.com`
  - `smtp_port`: `465` (SSL/TLS) 或 `587` (STARTTLS)
  - `username`: `resend`
  - `password`: 您的 Resend API Key

### 6.3 QQ 邮箱 / 163 网易邮箱
1. 登录网页版邮箱，进入「设置」->「账户」。
2. 开启 `POP3/SMTP 服务`。
3. 按照页面短信验证生成「授权码」（非邮箱网页登录密码）。
4. 在算子中配置：
   - QQ 邮箱：`smtp_host: smtp.qq.com`，`smtp_port: 465`，`encryption: SSL/TLS`。
   - 163 邮箱：`smtp_host: smtp.163.com`，`smtp_port: 465`，`encryption: SSL/TLS`。
   - `password`：填入生成的授权码。

### 6.4 Microsoft Outlook / Office 365
- `smtp_host`: `smtp.office365.com`
- `smtp_port`: `587`
- `encryption`: `STARTTLS`
- `username`: 微软账号邮箱
- `password`: 微软账号密码（如开启两步验证需使用应用专用密码）

## 7. 典型应用场景
- **自动化报表与日志定时发送**：自动化任务在夜间运行完成后，抓取生成的 Excel 汇总表或压缩日志，通过附件形式发送给运维负责人。
- **任务异常熔断实时报警**：在 RPA 流程或数据抓取发生严重异常时，自动构造包含错误堆栈与排查指南的 HTML 高亮告警邮件推送到指定组。
- **企业用户注册验证码投递**：在用户注册或敏感操作鉴权时，通过 Resend API 或企业内网 SMTP 毫秒级分发动态验证码。
- **桌面软件诊断报告回传**：将客户端崩溃日志、截屏附件静默打包回传至技术支持邮箱。
