# 📡 网络请求与文件下载套件 (Network & Downloader Suite)

[![Plugin Version](https://img.shields.io/badge/version-0.50.5-blue.svg)](manifest.json)
[![Group](https://img.shields.io/badge/group-Network-indigo.svg)](#)
[![Platform](https://img.shields.io/badge/platform-All-green.svg)](#)
[![License](https://img.shields.io/badge/license-Freeware-brightgreen.svg)](#)

企业级跨平台全栈网络通信、流媒体抓取与分布式数据传输套件。涵盖工业级 RESTful API 请求客户端、HTTP 回调监听服务 (Webhook)、TCP/UDP 网络连接池与监听收发、WebSocket 全双工长连接、高性能分布式数据下载与动态做种分发、流媒体视频高清下载、点对点加密穿透与中继共享，以及网络往返延迟探测 (Ping) 与端口连通性检测。

---

## 📖 简介 (Overview)

`xy_network` 是桌面自动化与分布式协同系统的核心网络通信枢纽，集成了多项企业级能力：
- **HTTP / RESTful 通信矩阵**：
  - **旗舰请求客户端 (`HttpRequest`)**：支持全量 HTTP 谓词、`multipart/form-data` 文件上传、命令行指令一键解析导入、网络代理配置、网络故障自动重试与响应状态码断言。
  - **基础请求算子 (`HttpGet`, `HttpPost`, `HttpHead`)** 与流式大文件下载器 (`HttpDownload`)，支持断点覆盖与安全校验配置。
  - **临时 Webhook 监听服务 (`HttpWebhook`)**：内置轻量级监听微服务，支持 Token 安全鉴权并捕获远程系统推送回调。
- **全双工 WebSocket 长连接**：
  - 提供高可靠的全双工长连接客户端：`WebSocketConnect`、`WebSocketSend` 与 `WebSocketReceive`，支持全局连接池管理与文本/二进制帧传输。
- **网络连接池与数据通道**：
  - 提供 `NetworkConnect`（建立连接）、`NetworkListen`（服务监听）、`NetworkSend` 与 `NetworkReceive`（支持定界符分包、定长分帧、纯文本与 Hex 工业报文解析），以及统一的连接资源安全释放机制 (`NetworkClose`)。
- **分布式文件共享与传输**：
  - **分布式下载与做种 (`BtDownload`, `BtSeed`)**：内嵌高性能数据分发服务，支持磁力链接与 Torrent 文件下载，支持单种子及动态监听目录自动做种共享。
  - **点对点穿透与中继 (`P2PHost`, `P2PServer`)**：实现网络穿透直连与受限环境加密中继回退，支持秒级配对共享本地文件夹。
- **流媒体视频解析下载 (`VideoDownload`)**：
  - 进程级内嵌流媒体视频解析引擎，支持从主流浏览器读取已登录 Cookie，实现免登录态解析与高清格式下载。
- **网络诊断与远程输入协同**：
  - 跨平台 RTT 往返延迟探测 (`NetworkPing`)、域名 DNS 解析 (`DomainQuery`)、端口连通性开放检测 (`PortCheck`)，以及基于 Token 鉴权的键鼠控制远程透传 (`SendInput`, `ReceiveInput`)。
- **正向与隧道代理服务 (`ForwardProxy`)**：
  - 启动高性能本地代理服务，支持多协议中继与用户身份认证。

---

## ✨ 核心特性 (Features)

- **全协议栈覆盖**：支持 HTTP/1.1 & HTTP/2、WebSocket、TCP/UDP 通信、分布式 P2P 与种子传输。
- **指令一键逆向解析**：在 `HttpRequest` 中可直接粘贴外部复制的命令行请求语句，自动拆解填充 Method、URL、Headers 与 Payload。
- **内嵌分布式引擎**：无需依赖外部下载器进程，兼具高速数据下载与目录变动监听做种。
- **免子进程流媒体下载**：直接在安全沙箱内调度解析流媒体音视频管线，支持浏览器凭据联动。
- **连接池复用与工业报文**：网络连接全局复用托管，支持断线重连、自签名证书忽略、Hex 工业报文收发与定界符自动分包粘包处理。

---

## 🛠️ 动作指令全景速查 (Action Catalog)

| 动作标识 (Tag) | 功能名称 | 描述 | 输出类型 |
| :--- | :--- | :--- | :--- |
| `HttpRequest` | 通用 HTTP 请求 | 综合 HTTP 客户端，支持任意请求方法、文件上传、自动重试、网络代理与状态断言 | `string` |
| `HttpGet` | HTTP GET 请求 | 发送标准 HTTP GET 请求并获取服务器返回的文本响应内容与状态信息 | `string` |
| `HttpPost` | HTTP POST 请求 | 发送携带数据载荷的 HTTP POST 请求，支持 JSON、表单与纯文本等多种编码格式 | `string` |
| `HttpHead` | HTTP HEAD 请求 | 发送标准 HTTP HEAD 请求，仅快速获取目标资源的响应头部元数据 | `string` |
| `HttpDownload` | HTTP 文件下载 | 通过 HTTP 网址下载文件，支持覆盖判断、超时设置与忽略证书校验 | `string` |
| `HttpWebhook` | HTTP 回调监听 | 在本地启动轻量级微服务，等待并捕获远程系统推送的 Webhook 回调通知 | `string` |
| `SendEmail` | 邮件发送 | 通过 SMTP 协议或邮件服务 API 发送邮件，支持抄送、密送、HTML 格式与多附件 | `string` |
| `WebSocketConnect` | WebSocket 建立长连接 | 建立全双工实时长连接通道并注册至全局连接池 | `string` |
| `WebSocketSend` | WebSocket 发送消息 | 通过指定的 WebSocket 长连接通道向远端发送文本或二进制数据帧 | `string` |
| `WebSocketReceive` | WebSocket 接收消息 | 从已建立的 WebSocket 长连接通道中等待并读取单条消息 | `string` |
| `NetworkConnect` | 网络建立连接 | 创建 TCP 或 UDP 客户端网络连接并注册到全局连接池 | `string` |
| `NetworkListen` | 网络服务监听 | 本地绑定并监听指定端口上的网络连接或入站数据报文 | `string` |
| `NetworkSend` | 网络数据发送 | 向指定的网络连接发送纯文本内容或十六进制字节报文 | `string` |
| `NetworkReceive` | 网络数据接收 | 从已连接的网络通道中按定界符、固定长度或全部可用流读取数据 | `string` |
| `NetworkClose` | 关闭网络连接 | 安全释放已打开的网络连接句柄或代理服务资源 | `string` |
| `BtDownload` | BT/种子下载 | 通过 BT 种子文件或磁力链接高速下载网络资源 | `string` |
| `BtSeed` | BT/种子做种分发 | 将本地完整文件或目录作为种子进行持续做种与分发共享 | `string` |
| `VideoDownload` | 流媒体视频下载 | 高速解析并下载网页流媒体音视频，支持浏览器登录态 Cookie 读取与格式转码配置 | `string` |
| `P2PHost` | P2P 文件共享节点 | 共享本地文件夹并通过点对点直连或服务端中继进行分布式文件分发与传输 | `string` |
| `P2PServer` | P2P 信令与中继服务 | 启动 P2P 网络信令发现服务与加密流量中继中心 | `string` |
| `NetworkPing` | 网络连通性探测 | 跨平台探测目标主机的网络往返延迟 (RTT) 与连通可用性 | `string` |
| `PortCheck` | 端口可用性检测 | 探测目标主机指定网络端口的连通开放状态与握手耗时 | `string` |
| `DomainQuery` | 域名解析查询 | 调用 DNS 解析服务，将目标域名查询解析为对应的 IP 地址与网络服务信息 | `string` |
| `SendInput` | 发送远程输入事件 | 将本地操作捕获的键鼠动作与控制指令向远端受控设备传输 | `string` |
| `ReceiveInput` | 接收远程输入事件 | 本地监听接收来自远端设备的输入动作与协同控制指令 | `string` |
| `ForwardProxy` | 正向代理服务 | 启动本地正向代理或隧道代理服务，支持多协议中继与用户鉴权 | `string` |

---

## 📚 详细参数说明与使用参考 (Detailed Reference)

### 1. HTTP 客户端与 Webhook 监听 (HTTP & RESTful Suite)

#### `HttpRequest` - 通用 HTTP 请求
- **输入参数：**
  - `method` (*string*, 默认 `GET`): HTTP 请求方式（GET, POST, PUT, DELETE, PATCH, HEAD, OPTIONS）。
  - `url` (*string*): 目标接口完整 URL 地址。
  - `headers` (*string*): 自定义请求头信息（JSON 对象或每行 Key: Value）。
  - `body_type` (*string*, 默认 `None`): 载荷类型（None, JSON, Form, Multipart, Raw）。
  - `body` (*string*): 请求正文内容。
  - `multipart_files` (*string*): 文件上传配置。
  - `curl_command` (*string*): 一键粘贴命令行自动解析配置。
  - `timeout_secs` (*number*, 默认 `30`): 超时秒数。
  - `ignore_ssl_errors` (*bool*, 默认 `false`): 是否跳过证书安全验证。
  - `proxy` (*string*): 网络代理服务器地址（HTTP/SOCKS5）。
  - `retry_count` (*number*, 默认 `0`): 失败自动重试轮数。
  - `assert_status` (*number*, 默认 `0`): 期望状态码（非 0 时校验）。
- **输出类型：** `string`（响应正文）

#### `HttpGet`, `HttpPost`, `HttpHead` 与 `HttpDownload`
- **`HttpGet`**：参数包含 `url`、`req_headers`、`query_string`、`timeout_secs`、`ignore_ssl_errors`。
- **`HttpPost`**：参数包含 `url`、`content_type`（application/json 等）、`content`、`req_headers`、`timeout_secs`、`ignore_ssl_errors`。
- **`HttpHead`**：参数包含 `url`、`req_headers`、`timeout_secs`、`ignore_ssl_errors`。
- **`HttpDownload`**：参数包含 `url`、`folder`（保存目录）、`filename`（保存文件名）、`overwrite`（覆盖已有）、`timeout_secs`、`ignore_ssl_errors`。

#### `HttpWebhook` - HTTP 回调监听
- **输入参数：**
  - `bind_address`: 本地监听绑定地址与端口（如 `0.0.0.0:8088`）。
  - `path`: 路由路径（如 `/webhook`）。
  - `timeout_secs`: 等待超时秒数（0 为无限等待）。
  - `expected_secret`: 鉴权密钥 Token（可选）。

---

### 2. 长连接与通信连接池 (WebSocket & Connections)

#### WebSocket 模块
- **`WebSocketConnect`**：建立长连接。参数：`url`（长连接地址）、`client_name`（句柄名称）、`timeout_secs`。
- **`WebSocketSend`**：向指定连接发送消息。参数：`client_name`、`message`、`is_binary`（是否以二进制帧发送）。
- **`WebSocketReceive`**：从指定连接等待读取消息。参数：`client_name`、`timeout_secs`。

#### 网络连接与数据收发
- **`NetworkConnect`**：创建客户端连接。参数：`host_address`、`port_number`、`protocol`（TCP/UDP）、`handle_name`、`timeout_ms`。
- **`NetworkListen`**：绑定服务端口。参数：`socket_address`、`protocol`、`handle_name`、`timeout_secs`。
- **`NetworkSend`**：发送数据。参数：`socket`（句柄名称）、`value`（发送内容）、`data_mode`（Text/Hex）、`append_crlf`。
- **`NetworkReceive`**：接收数据。参数：`socket`、`read_mode`（AllAvailable, UntilDelimiter, FixedLength）、`delimiter`、`length`、`data_mode`、`timeout_ms`。
- **`NetworkClose`**：安全释放句柄。参数：`socket`（句柄标识，填 `'all'` 关闭全部）。

---

### 3. 分布式传输与流媒体下载 (Distributed Transfer & Video)

- **`BtDownload`**：通过种子或磁力链接下载。参数：`file`（种子路径或磁力链接）、`save_path`（保存目录）、`wait_finish`（是否等待全部完成）、`timeout_secs`。
- **`BtSeed`**：本地做种共享。参数：`torrent`（种子文件或监听目录）、`resource_dir`（资源文件夹）、`watch_folder`（自动监听变动）、`seed_duration`（做种时长）。
- **`VideoDownload`**：流媒体高清下载。参数：`url`、`output`（保存目录）、`options`（高级配置 JSON）、`cookiefile`、`browser`（从浏览器自动提取 Cookie）、`profile_directory`。
- **`P2PHost`**：共享本地文件夹。参数：`folder_path`、`relay_fallback`（中继回退）、`server_url`（信令服务）、`share_code`（提取码）。
- **`P2PServer`**：启动信令与中继。参数：`bind_address`、`enable_relay`、`optional_root_folder`。

---

### 4. 辅助服务与网络诊断 (Services & Diagnostics)

- **`ForwardProxy`**：本地正向代理。参数：`port`、`bind_address`、`protocol`（ALL, HTTP, SOCKS5）、`auth_username`、`auth_password`、`handle_name`、`timeout_secs`。
- **`SendEmail`**：发送邮件。参数：`service_type`、`provider_preset`、`smtp_host`、`smtp_port`、`encryption`、`api_key`、`username`、`password`、`from`、`to`、`cc`、`bcc`、`reply_to`、`subject`、`body`、`body_type`、`attachments`、`timeout_secs`、`ignore_ssl_errors`。
- **`NetworkPing`**：网络往返延迟探测。参数：`host`、`timeout_ms`。
- **`PortCheck`**：服务端口连通性检测。参数：`host`、`port`、`timeout_ms`。
- **`DomainQuery`**：DNS 域名解析。参数：`p_node_name`、`p_service_name`。
- **`SendInput` / `ReceiveInput`**：远程输入指令透传与接收协同。

---

## 📦 插件清单定义 (Manifest Reference)

```json
{
  "plugin_id": "xy_network",
  "name": "网络请求与文件下载 (Network & Downloader)",
  "version": "0.50.5",
  "group": "Network",
  "group_icon": "📡",
  "description": "提供发送 HTTP 接口请求 (API)、大文件高速下载、WebSocket 实时长连接、网络状态检测与局域网文件互传功能",
  "actions": [...]
}
```

---

## 📄 许可证 (License)

本项目遵循开源发布协议。
