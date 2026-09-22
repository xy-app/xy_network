# HTTP GET 请求 (HttpGet)

## 1. 功能概述
`HttpGet` 算子用于向远程服务器发送标准 HTTP/HTTPS GET 请求，安全拉取远程 API 数据或网页内容，支持自定义请求头、查询参数解析与自签名 SSL 证书校验跳过。

## 2. 核心特性与工业痛点解决
- **真实工业引擎驱动**：基于 `reqwest` 纯 Rust 网络客户端，彻底替代旧版日志桩，具备生产级高性能与内存安全。
- **查询参数智能合并**：支持以 JSON 对象（如 `{"name": "test", "id": 1}`）或传统查询串方式传入 `query_string`，自动进行 URL 编码。
- **SSL 校验跳过**：支持 `ignore_ssl: true`，在内网测试或未配置有效 CA 的服务器环境中稳定请求。

## 3. 平台支持与权限
- **支持平台**：`Windows` / `Linux` / `macOS` (跨平台通用)
- **管理员权限**：`否` (标准用户权限)

## 4. 参数说明

| 属性名称 | 参数类型 | 默认值 | 说明 |
| :--- | :--- | :--- | :--- |
| `url` | `String` | `"http://"` | 目标 HTTP/HTTPS 请求 URL |
| `req_headers` | `MixedType` | `""` | 请求头 (JSON 格式或多行 `Key: Value`) |
| `query_string` | `MixedType` | `""` | URL 查询参数 (JSON 对象或 URL 编码串) |
| `timeout` | `Number` | `30` | 请求超时时间（秒） |
| `ignore_ssl` | `Bool` | `false` | 是否忽略 HTTPS 证书有效性校验 |

## 5. 输出变量与上下文
- **返回值**：标准格式 JSON 字符串，包含状态码、响应体、响应头与耗时：
  ```json
  {
    "status": 200,
    "body": "{\"code\": 0, \"data\": \"ok\"}",
    "headers": { "content-type": "application/json" },
    "elapsed_ms": 35
  }
  ```
- **上下文变量**：
  - `http_response_body`：响应正文文本。
  - `http_status_code`：HTTP 状态码。

## 6. 典型应用场景
- **业务系统数据查询**：调用企业内部 OA、CRM 或 ERP 的数据查询 API。
- **健康检查与网关探测**：定时探测关键业务接口是否返回 HTTP 200。
