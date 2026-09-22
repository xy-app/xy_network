# 📡 官方高级网络与流媒体套件 (Network & Downloader Suite)

[![Plugin Version](https://img.shields.io/badge/version-0.49.1-blue.svg)](manifest_12.json)
[![Group](https://img.shields.io/badge/group-Network-indigo.svg)](#)
[![Platform](https://img.shields.io/badge/platform-All-green.svg)](#)
[![License](https://img.shields.io/badge/license-MIT-lightgrey.svg)](#)

工业级跨平台全栈网络通信、流媒体抓取与分布式数据传输套件。涵盖工业级 RESTful API 旗舰客户端、HTTP 回调监听服务 (Webhook)、底层 TCP/UDP 套接字连接池与监听收发、WebSocket 全双工长连接、基于纯 Rust `librqbit v9` 的 BitTorrent 高速下载与动态做种分发、基于 PyO3 零进程开销的 `yt-dlp` 高清流媒体视频下载、WebRTC P2P 穿透与加密中继共享，以及网络 RTT 延迟探测 (Ping) 与端口可用性检测。

---

## 📖 简介 (Overview)

`xy_network` 是桌面自动化与分布式协同系统的核心网络通信枢纽，集成了多项企业级能力：
- **工业级 HTTP/RESTful 通信**：
  - **旗舰请求器 (`HttpRequest`)**：支持全量 HTTP 谓词、`multipart/form-data` 文件上传、浏览器/Postman cURL 指令一键解析、多级代理穿透、5xx/网络故障自动重试与 `extract_json_path` 字段快速提取。
  - **基础方法算子 (`HttpGet`, `HttpPost`, `HttpHead`)** 与流式大文件下载器 (`HttpDownload`)，支持进度追踪与自动文件名推导。
  - **临时 Webhook 监听服务 (`HttpWebhook`)**：内置基于 Axum 的轻量级 HTTP 监听器，支持 Token 安全鉴权并捕获远程推送回调。
- **全双工 WebSocket 长连接**：
  - 基于 `tokio-tungstenite` 实现 `WebSocketConnect`、`WebSocketSend` 与 `WebSocketReceive`，支持心跳维系与全局连接池托管。
- **底层 TCP/UDP 套接字引擎**：
  - 提供 `NetworkConnect`（连接）、`NetworkListen`（监听绑定）、`NetworkSend` 与 `NetworkReceive`（支持定长/定界符分帧、纯文本与工业 Hex 报文解析），以及统一的连接池句柄释放机制 (`NetworkClose`)。
- **P2P 分布式传输与 BitTorrent 做种/下载**：
  - **纯 Rust BT 引擎 (`BtDownload`, `BtSeed`)**：基于 `librqbit v9`，支持磁力链接与 Torrent 文件下载，支持单种子及动态监听文件夹自动做种分发。
  - **WebRTC P2P 穿透与中继 (`P2PHost`, `P2PServer`)**：实现跨 NAT 内网穿透直连与受限环境加密中继回退，支持文件夹秒级点对点配对共享。
- **流媒体视频抓取 (`VideoDownload`)**：
  - 通过 `pypm` 沙箱集成 `yt-dlp` 原生库，利用 PyO3 进程内无额外进程开销执行抓取，支持读取 Chrome/Edge/Firefox 登录态 Cookie 与自定义配置。
- **网络诊断与远程输入协同**：
  - 免提权的跨平台 RTT 延迟与丢包率探测 (`NetworkPing`)、域名 DNS 解析 (`DomainQuery`)、TCP 握手端口开放检测 (`PortCheck`)，以及基于 Token 鉴权的键鼠输入远程透传 (`SendInput`, `ReceiveInput`)。

---

## ✨ 核心特性 (Features)

- **全协议栈覆盖**：HTTP/1.1 & HTTP/2、WebSocket、原始 TCP/UDP Socket、WebRTC P2P 与 BitTorrent。
- **cURL 命令行一键逆向解析**：在 `HttpRequest` 中直接粘贴浏览器网络控制台或 Postman 复制的 cURL 命令，即可自动拆解并覆盖 URL、Headers、Body 与 Method。
- **纯 Rust BitTorrent 引擎**：内嵌 `librqbit v9`，不依赖外部 qBittorrent 或 Aria2 进程，兼具极速下载与文件夹监听自动做种。
- **PyO3 进程内嵌入式视频下载**：免除外部 `subprocess` 繁重开销与命令行参数转义风险，直接在沙箱内调用 `yt-dlp` 高清流媒体解析管线。
- **高工业级可靠性**：套接字连接池全局复用、断网重试、自签名证书忽略、Hex 工业报文收发与定界符自动分包粘包处理。

---

## 🛠️ 动作指令全景速查 (Action Catalog)

| 动作标识 (Tag) | 功能名称 | 描述 | 输出类型 |
| :--- | :--- | :--- | :--- |
| `HttpRequest` | 通用 HTTP/RESTful 请求 | 旗舰级 RESTful 组件，支持 cURL 解析、文件上传、代理、重试与 JSONPath | `string` |
| `HttpGet` | HTTP GET 请求 | 发送标准 HTTP GET 请求，获取响应体、状态码及耗时 | `string` |
| `HttpPost` | HTTP POST 请求 | 发送携带 JSON、Form 或纯文本载荷的 POST 请求 | `string` |
| `HttpHead` | HTTP HEAD 请求 | 仅拉取目标资源的 HTTP 头部元数据（不下载 Body） | `string` |
| `HttpDownload` | HTTP 文件下载 | 流式下载大文件，支持进度跟踪与文件名智能推导 | `string` |
| `HttpWebhook` | HTTP 回调监听 | 基于 Axum 启动微服务，等待并接收远程系统 Webhook 回调 | `string` |
| `SendEmail` | 邮件发送 | 通过 SMTP 协议或 Resend API 发送邮件，支持 Google (Gmail)、Resend、QQ、163 等，支持 HTML 及附件 | `string` |
| `WebSocketConnect` | WebSocket 建立长连接 | 建立异步全双工 WS/WSS 客户端连接并注册到全局连接池 | `string` |
| `WebSocketSend` | WebSocket 发送消息 | 向指定 WebSocket 连接发送文本或二进制数据帧 | `string` |
| `WebSocketReceive` | WebSocket 接收消息 | 从已建立的 WebSocket 连接中等待并接收单条消息 | `string` |
| `NetworkConnect` | 网络连接 (Socket) | 创建 TCP/UDP 客户端套接字并注册到连接池 | `string` |
| `NetworkListen` | 网络监听 (Socket) | 本地绑定监听 TCP 入站连接或 UDP 报文 | `string` |
| `NetworkSend` | 网络发送 (Socket) | 发送文本字符串或 Hex 工业十六进制原始报文 | `string` |
| `NetworkReceive` | 网络接收 (Socket) | 按定界符、定长或全部可用流读取入站套接字数据 | `string` |
| `NetworkClose` | 关闭网络连接 | 安全释放已打开的 TCP/UDP 套接字或 WebSocket 句柄 | `string` |
| `BtDownload` | BT/种子下载 | 基于 librqbit 纯 Rust 引擎，通过 Torrent/Magnet 高速下载 | `string` |
| `BtSeed` | BT/种子做种分发 | 基于 librqbit 做种服务，支持动态监听文件夹持续分发做种 | `string` |
| `VideoDownload` | 流媒体视频下载 | 通过 PyO3 在进程内调用 yt-dlp，支持浏览器 Cookie 提取与高清下载 | `string` |
| `P2PHost` | P2P 文件分发节点 | 通过 WebRTC/Direct 穿透并与信令服务器共享本地文件夹 | `string` |
| `P2PServer` | P2P 信令与中继服务 | 启动 P2P 网络信令发现与 WebRTC 握手及加密流量中继中心 | `string` |
| `NetworkPing` | 网络连通性探测 | 跨平台多轮 RTT 延迟与丢包率探测（免 Root/管理员权限） | `string` |
| `PortCheck` | 端口可用性检测 | TCP 三次握手探测目标 IP 与端口的开放状态及 RTT 延迟 | `string` |
| `DomainQuery` | 域名查询 (DNS) | 通过系统 DNS 解析域名对应的 IPv4/IPv6 与套接字地址 | `string` |
| `SendInput` | 发送远程输入事件 | 通过网络套接字将本地捕获的输入事件发送到远端设备 | `string` |
| `ReceiveInput` | 接收远程输入事件 | 监听接收远端设备发送的输入动作并进行校准回放 | `string` |
| `ForwardProxy` | 正向代理服务 | 启动高性能正向代理/隧道代理服务，支持 HTTP/HTTPS 与 SOCKS5 双协议自适应中继与鉴权 | `string` |

---

## 📚 详细参数说明与使用参考 (Detailed Reference)

### 1. HTTP 客户端与 Webhook 监听 (HTTP & RESTful Suite)

#### `HttpRequest` - 通用 HTTP/RESTful 请求 (旗舰组件)
- **输入参数：**
  | 参数名 (Field) | 显示名称 | 类型 | 默认值 | 预设推荐值 | 说明 |
  | :--- | :--- | :--- | :--- | :--- | :--- |
  | `url` | 请求地址 (URL) | `string` | `https://` | - | 目标请求 URL |
  | `method` | 请求方法 | `string` | `GET` | `GET`, `POST`, `PUT`, `DELETE`, `PATCH`, `HEAD`, `OPTIONS` | 标准 HTTP 谓词 |
  | `headers` | 请求头 (Headers) | `string` | `""` | - | 支持 JSON 字典或多行 `Key: Value` 格式 |
  | `query_params` | URL 查询参数 | `string` | `""` | - | 支持 JSON 对象或 `a=1&b=2` 形式 |
  | `body` | 请求体 (Body) | `string` | `""` | - | 请求载荷内容 |
  | `body_type` | 请求体类型 | `string` | `JSON` | `JSON`, `Form`, `Raw`, `MultipartFile` | 数据封装编码方式 |
  | `file_path` | 上传文件路径 | `string` | `""` | - | `MultipartFile` 模式下的本地文件绝对/相对路径 |
  | `file_field_name` | 文件表单字段名 | `string` | `file` | `file`, `upload` | 表单提交的文件 Key 键名 |
  | `curl_command` | cURL 指令解析 | `string` | `""` | - | 直接粘贴 cURL 命令，自动覆盖解析 Method、URL、Headers 与 Body |
  | `timeout` | 超时时间(秒) | `number` | `30` | `10`, `30`, `60` | 请求超时秒数 |
  | `ignore_ssl` | 忽略 SSL 证书校验 | `bool` | `false` | `false`, `true` | 是否跳过自签名或无效 SSL 证书阻断 |
  | `proxy` | 网络代理地址 | `string` | `""` | `http://127.0.0.1:7890`, `socks5://127.0.0.1:1080` | HTTP/SOCKS5 代理 |
  | `max_retries` | 失败自动重试次数 | `number` | `0` | `0`, `1`, `3` | 遇到网络中断或 5xx 时的最大自动重试轮数 |
  | `extract_json_path`| JSON 字段快速提取 | `string` | `""` | `data`, `code`, `token` | 自动从 JSON 响应体中直接提取目标键值 |
- **输出类型：** `string`（响应正文或提取出的字段内容）

#### `HttpDownload` - HTTP 文件下载
- **输入参数：**
  - `url` (*string*): 远程下载地址。
  - `folder` (*string*): 保存目录。
  - `filename` (*string*): 自定义保存文件名（留空则自动从响应头 `Content-Disposition` 或 URL 推导）。
  - `overwrite` (*bool*, 默认 `true`): 已存在时是否覆盖。
  - `headers` (*string*): 携带的请求头。
  - `timeout` (*number*, 默认 `60`): 总下载超时秒数。
- **输出类型：** `string`（最终落盘文件完整绝对路径）

#### `HttpWebhook` - HTTP 回调监听
- **输入参数：**
  | 参数名 (Field) | 显示名称 | 类型 | 默认值 | 预设推荐值 | 说明 |
  | :--- | :--- | :--- | :--- | :--- | :--- |
  | `bind_address` | 监听绑定地址 | `string` | `0.0.0.0:8088` | `0.0.0.0:8088`, `127.0.0.1:8088` | 本地绑定网卡 IP 与端口 |
  | `route_path` | 回调路由路径 | `string` | `/webhook` | `/webhook`, `/callback`, `/notify` | 匹配的 HTTP 路由 Path |
  | `secret_token` | 鉴权令牌 (Token) | `string` | `""` | - | 匹配 `X-Webhook-Secret` 或 `Authorization` 请求头 |
  | `timeout` | 最大等待时间(秒) | `number` | `60` | `30`, `60`, `300` | 阻塞等待入站回调的最长时限 |
- **输出类型：** `string`（接收到的回调请求体 JSON/Text）

#### `SendEmail` - 邮件发送 (SMTP & Resend API)
- **核心特性：**
  - **全服务商支持**：内置 `Gmail` (Google)、`Resend (SMTP)`、`Resend (API)`、`QQ`、`163`、`Outlook` 快捷预设。
  - **Google (Gmail) 接入**：选择 `Gmail` 预设，填入 Gmail 完整邮箱并在密码栏输入 Google 账号生成的 16 位 **应用专用密码 (App Password)** 即可稳定发送。
  - **Resend 双通道接入**：支持标准 SMTP (`smtp.resend.com:465`) 与 HTTPS REST API (`POST https://api.resend.com/emails`)。在云服务或企业受限网络封锁邮件端口时，启用 Resend (API) 走 443 端口直连，连通率 100%。
  - **多附件与富文本**：支持多个本地文件自动识别 MIME 打包作为附件，支持纯文本与 HTML 源码渲染。
- **输入参数：**
  | 参数名 (Field) | 显示名称 | 类型 | 默认值 | 预设推荐值 | 说明 |
  | :--- | :--- | :--- | :--- | :--- | :--- |
  | `service_type` | 服务类型 | `string` | `SMTP` | `SMTP`, `Resend (API)` | 协议通道：SMTP 通用协议或 Resend API 直连 |
  | `provider_preset`| 快捷服务商预设 | `string` | `Custom` | `Custom`, `Gmail`, `Resend (SMTP)`, `Resend (API)`, `QQ`, `163`, `Outlook` | 选择后自动配置对应服务商 Host/Port 与传输模式 |
  | `smtp_host` | SMTP 服务器地址 | `string` | `smtp.qq.com` | `smtp.gmail.com`, `smtp.resend.com`, `smtp.qq.com`, `smtp.163.com` | SMTP 邮件服务器主机名 |
  | `smtp_port` | SMTP 端口 | `number` | `465` | `465`, `587`, `25` | 465 (SSL/TLS), 587 (STARTTLS), 25 (明文) |
  | `encryption` | 传输加密 | `string` | `SSL/TLS` | `SSL/TLS`, `STARTTLS`, `None`, `Auto` | 传输层加密协议 |
  | `api_key` | Resend API Key | `string` | `""` | `re_...` | Resend API Key（仅在 Resend API 模式生效） |
  | `username` | 邮箱账号 | `string` | `""` | - | Gmail: 完整邮箱；Resend SMTP: 固定填 `resend`；QQ/163: 邮箱账号 |
  | `password` | 密码/授权码 | `string` | `""` | - | 邮箱密码、客户端授权码或 Gmail 应用专用密码 |
  | `from` | 发件人 (From) | `string` | `""` | - | 发件人名称与地址（如 `Support <support@domain.com>`） |
  | `to` | 收件人 (To) | `string` | `""` | - | 收件人邮箱，支持逗号、分号或换行分隔 |
  | `cc` / `bcc` | 抄送 / 密送 | `string` | `""` | - | 抄送与密送收件人地址 |
  | `reply_to` | 回复地址 | `string` | `""` | - | 对方点击回复时的目标地址 |
  | `subject` | 邮件主题 | `string` | `""` | - | 邮件标题 |
  | `body` | 邮件正文 | `string` | `""` | - | 邮件正文内容 |
  | `body_type` | 正文格式 | `string` | `Plain` | `Plain`, `HTML` | 纯文本或 HTML 富文本格式 |
  | `attachments` | 附件文件路径 | `string` | `""` | - | 本地文件路径，多个文件用逗号、分号或换行分隔 |
  | `timeout_secs` | 超时时间(秒) | `number` | `30` | `10`, `30`, `60` | 网络发送超时时长 |
  | `ignore_ssl_errors`| 忽略证书错误 | `bool` | `false` | `false`, `true` | 是否跳过自签名 SSL 证书校验 |
- **输出类型：** `string`（执行结果 JSON，包含 message_id、elapsed_ms 与 status）

---

### 2. WebSocket 全双工长连接 (WebSocket Suite)

- **`WebSocketConnect`**：
  - `url`: WebSocket 服务端地址（如 `ws://127.0.0.1:8080` 或 `wss://echo.websocket.org`）。
  - `connection_id`: 连接池唯一标识（留空自动基于 URL 分配）。
  - `headers`: 握手附加自定义 Header。
  - `timeout`: 握手超时秒数（默认 `10` 秒）。
- **`WebSocketSend`**：
  - `connection_id`: 目标连接句柄 ID。
  - `message`: 发送的消息正文（支持纯文本或 Hex）。
  - `message_type`: `Text` 或 `Binary`。
- **`WebSocketReceive`**：
  - `connection_id`: 目标连接句柄 ID。
  - `timeout`: 等待单条消息接收的超时时间（秒）。
- **`NetworkClose`**：
  - 传入 `connection_id`，将 `connection_type` 设为 `WebSocket` 即可优雅关闭连接。

---

### 3. 底层 Socket 网络连接与监听 (Socket Engine)

#### `NetworkConnect` 与 `NetworkListen`
- **`NetworkConnect`**：建立 TCP/UDP 客户端套接字并注册到全局连接池。
  - 参数：`host_address`、`port_number`、`protocol` (`TCP` / `UDP`)、`connection_id`、`timeout`。
- **`NetworkListen`**：绑定本地接口与端口监听入站连接。
  - 参数：`socket_address`（如 `0.0.0.0:8080`）、`protocol`、`connection_id`、`timeout`。

#### `NetworkSend` 与 `NetworkReceive`
- **`NetworkSend`**：
  - `socket`: 已连接套接字的 `connection_id`。
  - `value`: 发送数据（文本字符串或十六进制字节码如 `01 03 00 00 00 01 84 0A`）。
  - `format`: `Text` 或 `Hex`。
- **`NetworkReceive`**：
  - `mode`:
    - `AllAvailable`: 读取当前缓冲区内所有已达字节。
    - `FixedLength`: 严格等待读取达到 `fixed_length` 字节数。
    - `UntilDelimiter`: 持续读取直至遇到 `delimiter` 定界字符（如 `\n`、`\r\n`），彻底解决粘包问题。
  - `output_format`: `Text`（自动解码为字符串）或 `Hex`（转为十六进制编码）。

---

### 4. BitTorrent 与流媒体视频下载 (P2P & Media Downloads)

#### `BtDownload` - BT/种子下载
- **输入参数：**
  - `file`: `.torrent` 种子文件路径或 `magnet:?xt=...` 磁力链接。
  - `save_path`: 下载落盘的目标文件夹。
- **引擎特性：** 基于 `librqbit v9` 纯 Rust 打造，零外部依赖，极速并行分片调度。

#### `BtSeed` - BT/种子做种分发
- **输入参数：**
  - `torrent_path`: 单个 `.torrent` 种子文件路径。
  - `watch_folder`: 种子监听文件夹（新放入 `.torrent` 自动加入做种，移除自动卸载）。
  - `resource_folder`: 做种关联的本地物理数据文件根目录。
  - `port`: Peer 服务监听端口（默认 `4240`，推荐 `6881`）。
  - `seed_duration`: 持续做种秒数（`0` 为后台常驻做种不退出）。

#### `VideoDownload` - 流媒体视频下载 (yt-dlp)
- **输入参数：**
  | 参数名 (Field) | 显示名称 | 类型 | 默认值 | 预设推荐值 | 说明 |
  | :--- | :--- | :--- | :--- | :--- | :--- |
  | `url` | 视频网页地址 (URL) | `string` | `https://` | - | 在线视频网页 URL |
  | `output` | 输出保存目录 | `string` | `""` | - | 视频及字幕保存文件夹路径 |
  | `options` | 附加下载参数 | `string` | `""` | - | 额外的 JSON 配置参数 |
  | `cookiefile` | Cookie 文件路径 | `string` | `""` | - | 本地 Netscape 格式 Cookie 文件 |
  | `browser` | 读取 Cookie 的浏览器 | `string` | `chrome` | `chrome`, `edge`, `firefox`, `none` | 直接从本地已登录浏览器抓取会话 Cookie |
  | `profile_directory`| 浏览器配置目录 | `string` | `""` | - | 指定特定浏览器用户 Profile 目录 |
  | `cookies_from_browser`| 浏览器 Cookie 提取模式 | `string` | `""` | - | 高级浏览器提取扩展配置 |
- **实现架构：** 在 `pypm` 沙箱内加载 `yt-dlp` 原生模块，通过 PyO3 进程内无缝调用，无子进程创建损耗。

---

### 5. WebRTC P2P 穿透共享与网络探测 (WebRTC P2P & Diagnostics)

#### `P2PHost` 与 `P2PServer`
- **`P2PServer`**：一键拉起轻量信令服务器与 WebRTC 握手协助中心，提供加密流量中继转发 (`enable_relay = true`)。
- **`P2PHost`**：输入本地共享目录 `folder_path` 与信令地址 `server_url`，生成 `share_code` 共享提取码，支持穿透失败自动降级到服务器中继。

#### 网络健康自检算子
- **`NetworkPing`**：传入目标 IP/域名 `host` 与探测轮数 `count`，免管理员高权限输出多轮最小/最大/平均 RTT 延迟与丢包率。
- **`PortCheck`**：高速发起 TCP 三次握手探测目标主机端口（如 `80`, `443`, `3306`, `6379`）的开放状态与连通耗时。
- **`DomainQuery`**：调用系统底层 DNS 解析服务查询主机的主机名、IPv4、IPv6 与服务端口映射。
- **`SendInput` / `ReceiveInput`**：通过 TCP/UDP 套接字传输键盘鼠标事件，支持通过 `auth_token` 安全校验防止未授权接管。


---

### 6. 正向代理服务 (Forward Proxy Suite)

#### `ForwardProxy` - 启动正向代理 / 隧道代理服务
- **双协议自适应中继**：单个监听端口自动嗅探客户端连接协议，自适应支持 HTTP CONNECT 隧道代理（支持 HTTPS 全量穿透与纯 HTTP 请求中继）以及 SOCKS5 (RFC 1928) 代理。
- **用户安全鉴权**：支持可选设置用户名与密码。SOCKS5 采用 RFC 1929 握手校验，HTTP CONNECT 代理采用标准 `Proxy-Authenticate: Basic` 与 407 Proxy Authentication Required 认证流程。
- **后台常驻与生命周期联动**：代理服务在后台异步常驻运行，支持多实例并发（如分别监听 1080、8080 等不同端口），并在生命周期管理器与 `NetworkClose`（传入 `handle_name` 或留空全部停止）中实现平滑优雅下线。

---

## 📦 插件清单定义 (Manifest Reference)

```json
{
  "plugin_id": "xy_network",
  "name": "官方高级网络与流媒体套件 (Network & Downloader Suite)",
  "version": "0.49.1",
  "group": "Network",
  "group_icon": "📡",
  "description": "提供工业级 HTTP/RESTful、WebSocket、Socket 通信、BT 下载与做种、yt-dlp 视频下载、网络连通性探测及 P2P 穿透共享能力",
  "actions": [...]
}
```

---

## 📄 许可证 (License)

本项目遵循 [MIT License](LICENSE) 开源协议。
