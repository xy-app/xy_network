# 关闭网络连接 (NetworkClose)

## 1. 功能概述
`NetworkClose` 算子用于主动安全关闭先前通过 `NetworkConnect`、`NetworkListen` 或 `WebSocketConnect` 建立的 TCP/UDP 套接字或 WebSocket 长连接，释放底层操作系统网络文件句柄与内存资源。

## 2. 核心特性与工业痛点解决
- **资源泄漏防护**：杜绝长时间运行的无人值守 RPA 机器人因频繁创建网络连接而导致句柄耗尽崩溃的顽疾。
- **灵活的按需与一键清理**：支持根据特定的 `connection_id` 精准关闭指定连接，也可指定 `connection_type: "All"` 一键批量清理当前所有存活连接。
- **跨连接类型统一管理**：统一管理 TCP、UDP 套接字连接池与 WebSocket 客户端池。

## 3. 平台支持与权限
- **支持平台**：`Windows` / `Linux` / `macOS` (跨平台通用)
- **管理员权限**：`否` (标准用户权限)

## 4. 参数说明

| 属性名称 | 参数类型 | 默认值 | 说明 |
| :--- | :--- | :--- | :--- |
| `connection_id` | `String` | `""` | 待关闭的套接字或 WebSocket 连接唯一标识符（留空自动使用当前上下文连接） |
| `connection_type` | `String` | `"Socket"` | 连接类别：`"Socket"` (TCP/UDP)、`"WebSocket"` 或 `"All"` (关闭所有) |

## 5. 输出变量与上下文
- **返回值**：关闭结果 JSON 格式字符串：
  ```json
  {
    "status": "closed",
    "connection_id": "tcp_127.0.0.1:8080",
    "connection_type": "Socket"
  }
  ```

## 6. 典型应用场景
- **长连接通信生命周期完结清理**：在流程结束节点关闭网络连接，确保资源干净回收。
- **网络异常重连前置重置**：检测到网络异常断线时，先调用 `NetworkClose` 关闭失效句柄，再重新调用连接算子。
