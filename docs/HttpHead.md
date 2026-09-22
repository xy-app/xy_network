# HTTP HEAD 请求 (HttpHead)

## 1. 功能概述
`HttpHead` 算子用于向远程 Web 服务器发送标准 HTTP HEAD 请求，仅抓取目标资源的 HTTP 头部元数据（如 `Content-Length`、`Last-Modified`、`ETag`、`Content-Type`），而不下载任何响应正文（Body），极度节省带宽与处理耗时。

## 2. 核心特性与工业痛点解决
- **零带宽损耗资源探测**：在决定是否启动耗费数十分钟下载几 GB 大文件前，先发送 HEAD 请求验证该文件是否存在、实际文件字节大小以及最后修改时间。
- **自定义请求头与 SSL 跳过**：支持自定义 Headers，支持跳过自签名 SSL 证书校验。
- **元数据直接结构化提取**：返回标准 JSON 结构，并自动将关键头信息解析存入工作流上下文变量。

## 3. 平台支持与权限
- **支持平台**：`Windows` / `Linux` / `macOS` (跨平台通用)
- **管理员权限**：`否` (标准用户权限)

## 4. 参数说明

| 属性名称 | 参数类型 | 默认值 | 说明 |
| :--- | :--- | :--- | :--- |
| `url` | `String` | `"http://"` | 目标资源完整 HTTP/HTTPS URL |
| `req_headers` | `MixedType` | `""` | 自定义请求头 (JSON 格式或 Key: Value) |
| `timeout` | `Number` | `15` | 请求超时时间（秒） |
| `ignore_ssl` | `Bool` | `false` | 是否跳过 HTTPS 证书有效性校验 |

## 5. 输出变量与上下文
- **返回值**：HTTP 响应头元数据 JSON 字符串：
  ```json
  {
    "status": 200,
    "headers": {
      "content-length": "104857600",
      "content-type": "application/zip",
      "last-modified": "Wed, 21 Oct 2026 07:28:00 GMT"
    },
    "elapsed_ms": 22
  }
  ```
- **上下文变量**：
  - `http_status_code`：响应状态码（如 `200`）。
  - `http_content_length`：文件大小字节数（字符串格式）。

## 6. 典型应用场景
- **大文件更新预判**：比对本地已有文件修改时间与远程服务器 HEAD 头中的 `Last-Modified`，有更新才触发下载。
- **API 接口可用性心跳监控**：零流量开销持续探测接口是否在线返回 200。
