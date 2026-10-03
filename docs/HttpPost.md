# HTTP POST 请求 (HttpPost)

发送携带数据载荷的 HTTP POST 请求，支持 JSON、表单与纯文本等多种编码格式。

## 运行参数

* **请求地址 (URL) (`url`)**
  * 类型: `string`
  * 默认值: `"http://localhost:8080"`
  * 描述: 目标接口 URL 地址。
* **内容格式 (Content-Type) (`content_type`)**
  * 类型: `string`
  * 默认值: `"application/json"`
  * 描述: 请求体 MIME 类型。可选值：`application/json`, `application/x-www-form-urlencoded`, `text/plain`。
* **发送内容 (Payload) (`content`)**
  * 类型: `string`
  * 默认值: `""`
  * 描述: POST 请求正文内容（如 JSON 字符串或表单参数）。
* **请求头 (Headers) (`req_headers`)**
  * 类型: `string`
  * 默认值: `""`
  * 描述: 附加请求头信息。
* **超时时间 (秒) (`timeout_secs`)**
  * 类型: `number`
  * 默认值: `30`
  * 描述: 请求等待响应的最大秒数。
* **忽略 SSL 证书错误 (`ignore_ssl_errors`)**
  * 类型: `bool`
  * 默认值: `false`
  * 描述: 是否允许连接非受信任的 HTTPS 站点。

## 输出

* 类型: `string`
* 描述: 服务器返回的响应正文。

## 使用示例

```json
{
  "tag": "HttpPost",
  "url": "https://httpbin.org/post",
  "content_type": "application/json",
  "content": "{\"username\": \"admin\", \"action\": \"login\"}",
  "req_headers": "",
  "timeout_secs": 30,
  "ignore_ssl_errors": false
}
```