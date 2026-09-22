# HTTP 回调监听 (HttpWebhook)

## 1. 功能概述
`HttpWebhook` 算子基于高性能异步网络框架 Axum，在本地或内网临时启动微型 HTTP 服务，等待并接收来自第三方系统（如企业微信、GitHub、GitLab、Stripe、支付网关、工单系统）推送的入站 Webhook 请求，并在捕获到首个合法请求或达到超时后完成返回。

## 2. 核心特性与工业痛点解决
- **逆转传统轮询痛点**：彻底淘汰高资源浪费且有延迟的“每隔 5 秒调一次接口查状态”的低效轮询模式，改用事件驱动的主动推模式（Push Model），毫秒级响应外部事件。
- **内置安全口令鉴权**：支持配置 `secret_token`，自动校验请求头中的 `X-Webhook-Secret`、`X-Hub-Signature` 或 `Authorization`，阻断非法未授权请求伪造。
- **自定义路由与灵活绑定**：可自由指定本地监听网卡和端口（如 `0.0.0.0:8088`）以及路由路径（如 `/webhook/order_pay`）。
- **超时保护与自动释放**：可设定最大等待时间（`timeout` 秒），到期无入站请求优雅退出，避免流程无限死锁。

## 3. 平台支持与权限
- **支持平台**：`Windows` / `Linux` / `macOS` (跨平台通用)
- **管理员权限**：`否` (使用普通非特权端口 >1024 无需管理员权限)

## 4. 参数说明

| 属性名称 | 参数类型 | 默认值 | 说明 |
| :--- | :--- | :--- | :--- |
| `bind_address` | `String` | `"0.0.0.0:8088"` | 本地微服务监听绑定的 IP 与端口号 |
| `route_path` | `String` | `"/webhook"` | 等待接收回调的 HTTP 路径 |
| `secret_token` | `String` | `""` | 可选的安全校验 Token（若提供则强制验证请求头） |
| `timeout` | `Number` | `60` | 最大等待超时时间（秒） |

## 5. 输出变量与上下文
- **返回值**：捕获到的 Webhook 请求完整元数据及 Body 载荷 JSON：
  ```json
  {
    "status": "received",
    "method": "POST",
    "path": "/webhook",
    "headers": { "content-type": "application/json" },
    "body": "{\"event\": \"payment_success\", \"order_id\": \"12345\"}"
  }
  ```
- **上下文变量**：
  - `webhook_body`：接收到的回调正文字符串。
  - `webhook_headers`：接收到的请求头 JSON 字符串。

## 6. 典型应用场景
- **第三方异步事件通知唤醒**：接收支付网关支付成功通知、审批流通过回调。
- **Git / DevOps 自动化触发**：接收 GitHub/GitLab 的 Push 或 Release Webhook，触发本地打包与测试。
- **物联网或硬件传感器事件监听**：接收局域网设备按键或告警推送的 HTTP 报文。
