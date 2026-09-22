# 通用 HTTP/RESTful 请求 (HttpRequest)

## 1. 功能概述
`HttpRequest` 算子是工业级自动化网络交互的旗舰组件，支持全部主流 HTTP 动词（`GET`、`POST`、`PUT`、`DELETE`、`PATCH`、`HEAD`、`OPTIONS`）、表单及大文件上传（`multipart/form-data`）、网络代理穿透、自签名 SSL 校验跳过、指数退避自动重试、快速 JSON 字段提取，以及直接粘贴 cURL 命令智能解析自动填充。

## 2. 核心特性与工业痛点解决
- **cURL 指令一键解析**：用户在 Chrome/Edge 开发者工具或 Postman 中复制 `Copy as cURL (bash / cmd)`，直接粘贴到 `curl_command` 属性中，算子自动识别提取请求方法、完整 URL、所有 Headers 及 Body 载荷，彻底解决传统 RPA 配参数繁琐易错的痛点。
- **全格式请求体与文件上传**：支持 `JSON`、`Form`（x-www-form-urlencoded）、`Raw`（纯文本/XML）以及工业常用的 `MultipartFile` 文件直传，无需手动构造边界符与二进制流。
- **高韧性失败重试**：可配置 `max_retries`，当遇到网络连接超时、DNS 抖动或服务端 `5xx` 临时故障时，自动进行延时重试，显著提升无人值守 RPA 流程的稳定性。
- **网络代理与证书绕过**：支持配置 HTTP/SOCKS5 企业级代理，支持一键跳过自签名证书或私有内网 CA 校验（`ignore_ssl: true`）。
- **结构化输出与路径提取**：统一返回标准 JSON 结构（包含状态码、耗时毫秒、响应头、正文），并支持通过 `extract_json_path` 直接抽取深层字段存入上下文。

## 3. 平台支持与权限
- **支持平台**：`Windows` / `Linux` / `macOS` (跨平台通用)
- **管理员权限**：`否` (标准用户权限)

## 4. 参数说明

| 属性名称 | 参数类型 | 默认值 | 说明 |
| :--- | :--- | :--- | :--- |
| `url` | `String` | `"https://"` | 目标 API 请求完整 URL 地址 |
| `method` | `String` | `"GET"` | HTTP 方法 (`GET` / `POST` / `PUT` / `DELETE` / `PATCH` / `HEAD` / `OPTIONS`) |
| `headers` | `MixedType` | `""` | 自定义请求头，支持 JSON 对象字符串或多行 `Key: Value` 格式 |
| `query_params` | `MixedType` | `""` | URL 查询参数，支持 JSON 对象或 `key1=val1&key2=val2` 字符串 |
| `body` | `MixedType` | `""` | 发送给服务端的请求载荷内容 |
| `body_type` | `String` | `"JSON"` | 载荷编码类型 (`JSON` / `Form` / `Raw` / `MultipartFile`) |
| `file_path` | `FilePicker` | `""` | 当 `body_type` 为 `MultipartFile` 时待上传的本地文件路径 |
| `file_field_name` | `String` | `"file"` | 上传文件在表单中的参数名 |
| `curl_command` | `String` | `""` | 直接粘贴的 cURL 命令行，填入时将优先自动解析并填充各请求字段 |
| `timeout` | `Number` | `30` | 请求超时时间（秒） |
| `ignore_ssl` | `Bool` | `false` | 是否忽略 HTTPS 证书有效性校验（内网或自签名场景常用） |
| `proxy` | `String` | `""` | 网络代理服务器地址（如 `http://127.0.0.1:7890` 或 `socks5://...`） |
| `max_retries` | `Number` | `0` | 遇网络故障时的最大自动重试次数 |
| `extract_json_path` | `String` | `""` | 快捷提取响应 JSON 中的键值（如 `"data"`、`"token"`）存入上下文 |

## 5. 输出变量与上下文
- **返回值**：标准格式 JSON 字符串：
  ```json
  {
    "status": 200,
    "body": "{\"token\": \"xyz\"}",
    "headers": { "content-type": "application/json" },
    "elapsed_ms": 42
  }
  ```
- **上下文变量**：
  - `http_response_body`：响应正文文本。
  - `http_status_code`：响应 HTTP 状态码（整数）。
  - `http_extracted_value`：若配置了 `extract_json_path`，提取到的目标键值。

## 6. 典型应用场景
- **第三方开放平台对接**：对接飞书、钉钉、企业微信、SAP、ERP 等标准 REST API。
- **大文件报表上传**：定时向后台服务器上传生成的 Excel 审计报表或抓拍图像。
- **浏览器请求快速复制**：直接复制浏览器 DevTools 中的 cURL，快速复刻复杂前端接口调用。
