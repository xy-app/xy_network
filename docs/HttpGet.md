# HTTP GET 请求 (HttpGet)

发送标准 HTTP GET 请求并获取服务器返回的文本响应内容与状态信息。

## 运行参数

* **请求地址 (URL) (`url`)**
  * 类型: `string`
  * 默认值: `"http://localhost:8080"`
  * 描述: 目标接口 URL 地址。
* **请求头 (Headers) (`req_headers`)**
  * 类型: `string`
  * 默认值: `""`
  * 描述: 附加请求头信息 (JSON 对象或每行 Key: Value)。
* **查询参数 (Query) (`query_string`)**
  * 类型: `string`
  * 默认值: `""`
  * 描述: 附加的 URL 查询字符串或参数键值。
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
* 描述: HTTP 响应正文文本。

## 使用示例

```json
{
  "tag": "HttpGet",
  "url": "https://httpbin.org/get",
  "req_headers": "{\"Accept\": \"application/json\"}",
  "query_string": "page=1&limit=20",
  "timeout_secs": 30,
  "ignore_ssl_errors": false
}
```