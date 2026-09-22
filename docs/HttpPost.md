# HTTP POST 请求 (HttpPost)

## 1. 功能概述
`HttpPost` 算子用于向远程 Web 服务器或 RESTful API 发送携带请求体（Payload）的 HTTP POST 请求，提交业务数据或触发远端任务，支持 `application/json`、`application/x-www-form-urlencoded` 和纯文本格式。

## 2. 核心特性与工业痛点解决
- **多格式载荷原生适配**：支持自由选择 `content_type`（JSON 或 Form 表单），自动设定对应标准请求头并格式化发送。
- **真实生产级实现**：由 `reqwest` 纯 Rust 异步客户端驱动，替代原先无请求能力的日志占位代码。
- **完备错误捕获与状态反馈**：返回状态码、耗时毫秒、响应头与正文文本，并支持跳过自签名 SSL 证书。

## 3. 平台支持与权限
- **支持平台**：`Windows` / `Linux` / `macOS` (跨平台通用)
- **管理员权限**：`否` (标准用户权限)

## 4. 参数说明

| 属性名称 | 参数类型 | 默认值 | 说明 |
| :--- | :--- | :--- | :--- |
| `url` | `String` | `"http://"` | 目标 HTTP POST 请求完整 URL |
| `content_type` | `String` | `"application/json"` | 请求载荷内容格式 (`application/json` / `application/x-www-form-urlencoded` / `text/plain`) |
| `content` | `MixedType` | `""` | 待发送的正文载荷数据 |
| `req_headers` | `MixedType` | `""` | 额外自定义请求头 (JSON 格式或 Key: Value) |
| `timeout` | `Number` | `30` | 请求超时时间（秒） |
| `ignore_ssl` | `Bool` | `false` | 是否忽略 HTTPS 证书校验 |

## 5. 输出变量与上下文
- **返回值**：标准格式 JSON 字符串，包含状态码、响应正文、响应头与耗时：
  ```json
  {
    "status": 200,
    "body": "{\"code\": 0, \"message\": \"Created successfully\"}",
    "headers": { "content-type": "application/json" },
    "elapsed_ms": 48
  }
  ```
- **上下文变量**：
  - `http_response_body`：服务器响应正文字符串。
  - `http_status_code`：HTTP 状态码。

## 6. 典型应用场景
- **业务数据提交入库**：向 ERP 或 CRM 系统提交新增客户资料、采购清单。
- **即时通讯机器人推送**：向企微、飞书、钉钉群机器人发送 Markdown 告警通知。
