# WebSocket 接收消息 (WebSocketReceive)

从已建立的 WebSocket 长连接通道中等待并读取单条消息。

## 运行参数

* **连接句柄标识符 (`client_name`)**
  * 类型: `string`
  * 默认值: `"default_ws"`
  * 描述: 已建立连接的长连接句柄标识符。
* **等待超时时间 (秒，0为无限) (`timeout_secs`)**
  * 类型: `number`
  * 默认值: `30`
  * 描述: 等待入站消息帧的最大等待时长。

## 输出

* 类型: `string`
* 描述: 读取接收到的文本消息或二进制数据。

## 使用示例

```json
{
  "tag": "WebSocketReceive",
  "client_name": "echo_client",
  "timeout_secs": 30
}
```