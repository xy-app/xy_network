# 正向代理服务 (ForwardProxy)

## 1. 功能概述
`ForwardProxy` 算子在本地指定的网络接口与端口上启动工业级正向代理与隧道中继服务。支持标准 **HTTP / HTTPS CONNECT 隧道代理 (RFC 7231 / RFC 9110)** 与 **SOCKS5 代理协议 (RFC 1928 / RFC 1929)**，并独创支持**单端口双协议自适应感知 (Mixed-Port Auto-Sensing)**。代理服务启动后在后台异步静默守护常驻，立即放行后续工作流，并无缝支持通过 [`NetworkClose`](file:///c:/Users/ff/Documents/Rider/xy-app/src/plugins/xy_network/src/network_close.rs) 算子实现精准停止与端口释放。

## 2. 核心特性与工业痛点解决
- **单端口双协议自适应探测 (Mixed-Port)**：
  - 传统代理需要为 HTTP 和 SOCKS5 分别占用两个独立端口，管理繁琐且容易引发端口冲突；
  - `ForwardProxy` 默认提供 `ALL` 模式，在单端口上通过首字节动态嗅探（`0x05` 分流 SOCKS5，ASCII 字符分流 HTTP/CONNECT），使得同一个监听端口可同时满足浏览器（HTTP 代理）与专业网络客户端（SOCKS5 代理）的并发接入。
- **高安全账号密码鉴权**：
  - 支持可选的用户名与密码保护；
  - SOCKS5 执行 RFC 1929 握手凭据校验，HTTP 执行 `Proxy-Authorization: Basic <base64>` 报头校验；鉴权不通过时即刻拦截，返回 `407 Proxy Authentication Required` 响应，保护代理端口不被未授权盗用。
- **全双工高性能异步流中继**：
  - 基于 Tokio 异步非阻塞 I/O 与 `copy_bidirectional` 双工零拷贝管道，支撑超高并发 TCP 数据流并发穿透，低 CPU 与内存开销。
- **零学习成本与工作流闭环联动**：
  - 启动后自动后台托管，绝不卡死当前工作流，秒级流转至下一步；
  - 联动 `NetworkClose`：在流程执行完毕或需要释放端口时，仅需在 `NetworkClose` 中填入该句柄（如 `default_proxy` 或 `all`），即可安全关闭监听与活跃连接。

## 3. 平台支持与权限
- **支持平台**：`Windows` / `Linux` / `macOS` (跨平台全支持)
- **管理员权限**：`否` (绑定常规端口 1024~65535 无需管理员权限)

## 4. 参数说明

| 属性名称 (Field) | 参数类型 | 默认值 | 预设推荐值 | 说明 |
| :--- | :--- | :--- | :--- | :--- |
| `port` | `Number` | `1080` | `1080`, `8080`, `7890`, `10808`, `8888` | 本地 TCP 监听端口号 |
| `bind_address` | `String` | `"0.0.0.0"` | `"0.0.0.0"`, `"127.0.0.1"` | 本地绑定的网络接口 IP（`0.0.0.0` 允许局域网/外部访问，`127.0.0.1` 仅本机） |
| `protocol` | `String` | `"ALL"` | `"ALL"`, `"HTTP"`, `"SOCKS5"` | 代理协议模式（`ALL` 自适应双协议、`HTTP` 仅HTTP隧道、`SOCKS5` 仅SOCKS5） |
| `auth_username` | `String` | `""` | - | 代理认证用户名（留空表示免密匿名连接） |
| `auth_password` | `String` | `""` | - | 代理认证密码 |
| `handle_name` | `String` | `"default_proxy"` | `"default_proxy"` | 代理服务唯一标识句柄，用于多实例隔离及 `NetworkClose` 联动 |
| `timeout_secs` | `Number` | `60` | `30`, `60`, `300`, `0` | 隧道无数据通信时的空闲超时时间（秒，0 表示不设超时） |

## 5. 输出变量与上下文
- **返回值**：启动结果 JSON 字符串：
  ```json
  {
    "status": "running",
    "handle": "default_proxy",
    "bind_address": "0.0.0.0:1080",
    "port": 1080,
    "protocol": "ALL",
    "auth_required": false
  }
  ```
- **上下文变量**：
  - `proxy_handle`：启动成功的句柄标识符（如 `default_proxy`）。
  - `proxy_address`：绑定的完整 Socket 地址（如 `0.0.0.0:1080`）。
  - `proxy_port`：绑定的端口号（如 `1080`）。
  - `proxy_status`：当前服务状态（`"running"` / `"stopped"`）。

## 6. 典型应用场景

### 场景 1：网络爬虫与自动化测试的前置代理与用完即关
- **步骤 1**：拖入 `ForwardProxy`，设置 `port: 10808`，`handle_name: "crawler_proxy"`；
- **步骤 2**：拖入 `HttpRequest` 或执行 Python/浏览器自动化，配置代理为 `http://127.0.0.1:10808` 完成目标网站数据抓取；
- **步骤 3**：拖入 `NetworkClose`，配置 `socket: "crawler_proxy"`，完成测试后释放 10808 端口。

### 场景 2：局域网设备统一流量网关与安全鉴权
- 设置 `bind_address: "0.0.0.0"`, `port: 8888`, `auth_username: "corp_user"`, `auth_password: "SecurePassword123!"`；
- 局域网内的移动端、测试机或工控终端可通过该代理安全访问外网，未提供正确账号密码的连接将被直接 407 拦截。
