# HTTP 回调监听 (HttpWebhook)

在本地启动轻量级微服务，等待并捕获远程系统推送的 Webhook 回调通知。

## 运行参数

* **监听绑定地址与端口 (`bind_address`)**
  * 类型: `string`
  * 默认值: `"0.0.0.0:8088"`
  * 描述: 本地监听的服务绑定地址与端口。
* **监听路由路径 (`path`)**
  * 类型: `string`
  * 默认值: `"/webhook"`
  * 描述: 接收回调请求的 HTTP 路由路径。
* **等待超时时间 (秒，0为一直等待) (`timeout_secs`)**
  * 类型: `number`
  * 默认值: `60`
  * 描述: 等待远程请求的最大超时秒数，0 为无限阻塞。
* **鉴权密钥 Token (可选) (`expected_secret`)**
  * 类型: `string`
  * 默认值: `""`
  * 描述: 用于验证远程请求合法性的安全签名或密钥 Token。

## 输出

* 类型: `string`
* 描述: 接收到的远程请求报文内容（JSON 或文本）。

## 使用示例

```json
{
  "tag": "HttpWebhook",
  "bind_address": "0.0.0.0:8088",
  "path": "/webhook/notice",
  "timeout_secs": 120,
  "expected_secret": "my_secret_token"
}
```