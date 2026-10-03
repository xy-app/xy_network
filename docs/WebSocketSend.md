# WebSocket 发送消息 (WebSocketSend)

通过指定的 WebSocket 长连接通道向远端发送文本或二进制数据帧。

## 运行参数

* **连接句柄标识符 (`client_name`)**
  * 类型: `string`
  * 默认值: `"default_ws"`
  * 描述: 已建立连接的长连接句柄标识符。
* **待发送消息内容 (`message`)**
  * 类型: `string`
  * 默认值: `""`
  * 描述: 准备向长连接推送的文本或数据内容。
* **以二进制帧发送 (`is_binary`)**
  * 类型: `bool`
  * 默认值: `false`
  * 描述: `false` 表示发送 UTF-8 文本帧，`true` 表示发送二进制帧。

## 输出

* 类型: `string`
* 描述: 数据帧发送状态提示。

## 使用示例

```json
{
  "tag": "WebSocketSend",
  "client_name": "echo_client",
  "message": "{\"type\": \"ping\"}",
  "is_binary": false
}
```