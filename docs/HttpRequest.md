# 通用 HTTP 请求 (HttpRequest)

综合 HTTP 客户端，支持任意请求方法、文件上传、自动重试、网络代理与状态断言。

## 运行参数

* **请求方式 (Method) (`method`)**
  * 类型: `string`
  * 默认值: `"GET"`
  * 描述: HTTP 谓词方法。可选值：`GET`, `POST`, `PUT`, `DELETE`, `PATCH`, `HEAD`, `OPTIONS`。
* **请求地址 (URL) (`url`)**
  * 类型: `string`
  * 默认值: `"https://"`
  * 描述: 目标接口完整 URL 地址。
* **请求头 (Headers) (`headers`)**
  * 类型: `string`
  * 默认值: `""`
  * 描述: 自定义请求头信息。
* **请求体类型 (Body Type) (`body_type`)**
  * 类型: `string`
  * 默认值: `"None"`
  * 描述: 请求体编码封装类型。可选值：`None`, `JSON`, `Form`, `Multipart`, `Raw`。
* **请求内容 (Payload) (`body`)**
  * 类型: `string`
  * 默认值: `""`
  * 描述: 请求正文载荷字符串。
* **上传文件 (Multipart Files) (`multipart_files`)**
  * 类型: `string`
  * 默认值: `""`
  * 描述: 待上传的本地文件配置 (如 `name=filepath`)。
* **一键粘贴 cURL 命令 (`curl_command`)**
  * 类型: `string`
  * 默认值: `""`
  * 描述: 直接粘贴命令行，自动解析提取 Method、URL、Headers 与 Body。
* **超时时间 (秒) (`timeout_secs`)**
  * 类型: `number`
  * 默认值: `30`
  * 描述: 请求超时最大秒数。
* **忽略 SSL 证书错误 (`ignore_ssl_errors`)**
  * 类型: `bool`
  * 默认值: `false`
  * 描述: 是否跳过证书安全警告。
* **网络代理 (Proxy) (`proxy`)**
  * 类型: `string`
  * 默认值: `""`
  * 描述: HTTP 或 SOCKS5 代理服务器地址。
* **重试次数 (`retry_count`)**
  * 类型: `number`
  * 默认值: `0`
  * 描述: 请求失败时的最大自动重试轮数。
* **断言状态码 (`assert_status`)**
  * 类型: `number`
  * 默认值: `0`
  * 描述: 期望的响应状态码，非 0 时校验不符将触发失败。

## 输出

* 类型: `string`
* 描述: 服务器响应正文内容。

## 使用示例

```json
{
  "tag": "HttpRequest",
  "method": "POST",
  "url": "https://api.example.com/v1/resource",
  "headers": "{\"Authorization\": \"Bearer token_here\"}",
  "body_type": "JSON",
  "body": "{\"name\": \"item_01\"}",
  "multipart_files": "",
  "curl_command": "",
  "timeout_secs": 30,
  "ignore_ssl_errors": false,
  "proxy": "",
  "retry_count": 2,
  "assert_status": 200
}
```