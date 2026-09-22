# 网络连接 (NetworkConnect)

## 1. 功能概述
`NetworkConnect` 算子用于创建底层 TCP 或 UDP 网络套接字并连接到指定的远端主机与端口，成功建立后将套接字句柄注册到全局对象池（`SOCKET_POOL`），供下游 `NetworkSend`、`NetworkReceive` 与 `NetworkClose` 算子持续跨步骤复用。

## 2. 核心特性与工业痛点解决
- **全局连接池常驻管理**：彻底解决传统脚本中“连接一次只能发一句数据便被销毁”的严重架构缺陷，建立长寿命套接字对象。
- **TCP 与 UDP 双协议支持**：TCP 模式下执行标准握手连接，UDP 模式下自动绑定本地端口并锁定目标通信地址。
- **自定义句柄名称 (handle_name)**：支持指定句柄名称（如 `"plc_client_1"`），可在同一流程中并发管理连接多个不同工控设备或服务器。

## 3. 平台支持与权限
- **支持平台**：`Windows` / `Linux` / `macOS` (跨平台通用)
- **管理员权限**：`否` (标准用户权限)

## 4. 参数说明

| 属性名称 | 参数类型 | 默认值 | 说明 |
| :--- | :--- | :--- | :--- |
| `host_address` | `String` | `"127.0.0.1"` | 远程目标主机 IP 地址或域名 |
| `port_number` | `Number` | `8080` | 远程目标服务端口号 |
| `protocol` | `String` | `"TCP"` | 传输层协议：`"TCP"` 或 `"UDP"` |
| `handle_name` | `MixedType` | `"default_socket"` | 套接字唯一实例句柄标识符 |
| `timeout_ms` | `Number` | `3000` | 连接建立超时时间（毫秒） |

## 5. 输出变量与上下文
- **返回值**：成功建立连接的信息 JSON：
  ```json
  {
    "status": "connected",
    "handle": "default_socket",
    "protocol": "TCP",
    "remote": "127.0.0.1:8080"
  }
  ```
- **上下文变量**：
  - `network_connected_socket`：当前激活的套接字 handle 名称。

## 6. 典型应用场景
- **工业 PLC / 单片机工控通信**：与现场三菱、西门子、欧姆龙 PLC 建立 TCP 原始套接字长连接。
- **硬件外设串口转网络模块交互**：连接各类条码扫描枪、电子秤、传感器的串口服务器 IP:Port。
