# WebSocket 建立长连接 (WebSocketConnect)

建立全双工实时长连接通道并注册至全局连接池。

## 运行参数

* **WebSocket 地址 (URL) (`url`)**
  * 类型: `string`
  * 默认值: `"ws://127.0.0.1:8080"`
  * 描述: 目标长连接地址 (`ws://` 或 `wss://`)。
* **连接句柄标识符 (`client_name`)**
  * 类型: `string`
  * 默认值: `"default_ws"`
  * 描述: 长连接注册的唯一命名句柄，供收发消息引用。
* **握手超时时间 (秒) (`timeout_secs`)**
  * 类型: `number`
  * 默认值: `10`
  * 描述: 等待握手建立成功的最大秒数。

## 输出

* 类型: `string`
* 描述: 长连接建立状态提示。

## 使用示例

```json
{
  "tag": "WebSocketConnect",
  "url": "wss://echo.websocket.events",
  "client_name": "echo_client",
  "timeout_secs": 15
}
```