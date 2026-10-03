# HTTP HEAD 请求 (HttpHead)

发送标准 HTTP HEAD 请求，仅快速获取目标资源的响应头部元数据。

## 运行参数

* **请求地址 (URL) (`url`)**
  * 类型: `string`
  * 默认值: `"http://localhost:8080"`
  * 描述: 目标接口 URL 地址。
* **请求头 (Headers) (`req_headers`)**
  * 类型: `string`
  * 默认值: `""`
  * 描述: 附加请求头信息。
* **超时时间 (秒) (`timeout_secs`)**
  * 类型: `number`
  * 默认值: `15`
  * 描述: 请求等待响应的最大秒数。
* **忽略 SSL 证书错误 (`ignore_ssl_errors`)**
  * 类型: `bool`
  * 默认值: `false`
  * 描述: 是否允许连接非受信任的 HTTPS 站点。

## 输出

* 类型: `string`
* 描述: 包含状态码与响应头的 JSON 格式元数据。

## 使用示例

```json
{
  "tag": "HttpHead",
  "url": "https://example.com/resource",
  "req_headers": "",
  "timeout_secs": 15,
  "ignore_ssl_errors": false
}
```