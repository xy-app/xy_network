# WebSocket 建立长连接 (WebSocketConnect)

## 1. 功能概述
`WebSocketConnect` 算子基于高性能异步 WebSocket 引擎 `tokio-tungstenite`，与远端 WebSocket 服务器（`ws://` 或 `wss://`）建立全双工持久长连接，并将通信句柄登记到全局连接池（`WS_POOL`），供下游 `WebSocketSend`、`WebSocketReceive` 与 `NetworkClose` 持续跨算子复用。

## 2. 核心特性与工业痛点解决
- **跨算子连接池管理**：突破传统 RPA 工具中“单个算子连接后立即断开”的局限，通过唯一 `connection_id` 维持常驻双工长连接，实现事件流的高速收发。
- **安全传输与自定义标头**：原生支持 TLS 加密的 `wss://` 协议，并支持在握手阶段附加自定义认证请求头（如 `Authorization: Bearer <Token>`）。
- **非阻塞连接建立与超时熔断**：支持毫秒级握手超时配置，避免网络异常导致任务无响应挂起。

## 3. 平台支持与权限
- **支持平台**：`Windows` / `Linux` / `macOS` (跨平台通用)
- **管理员权限**：`否` (标准用户权限)

## 4. 参数说明

| 属性名称 | 参数类型 | 默认值 | 说明 |
| :--- | :--- | :--- | :--- |
| `url` | `String` | `"ws://127.0.0.1:8080"` | 目标 WebSocket 服务端完整 URL (`ws://` 或 `wss://`) |
| `connection_id` | `String` | `""` | 连接唯一标识符（若留空则自动生成并存入上下文） |
| `headers` | `MixedType` | `""` | 握手阶段附加 HTTP 请求头 (JSON 格式或 Key: Value) |
| `timeout` | `Number` | `10` | 握手建立连接超时时间（秒） |

## 5. 输出变量与上下文
- **返回值**：成功建立连接的信息 JSON：
  ```json
  {
    "status": "connected",
    "connection_id": "ws_127.0.0.1:8080",
    "url": "ws://127.0.0.1:8080"
  }
  ```
- **上下文变量**：
  - `ws_connection_id`：当前已连接的 WebSocket ID。
  - `ws_connected_url`：连接的服务端 URL。

## 6. 典型应用场景
- **金融行情与实时订单数据流订阅**：订阅高频股票/期货/外汇行情数据推送。
- **实时协作与聊天机器人交互**：对接各类即时通信或大模型流式输出 Gateway。
- **工业设备数据看板双向监控**：监听产线 PLC 或边缘网关主动上报的实时设备状态流。
