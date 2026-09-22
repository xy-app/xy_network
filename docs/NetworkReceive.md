# 网络接收 (NetworkReceive)

## 1. 功能概述
`NetworkReceive` 算子从已连接的 TCP/UDP 网络套接字中读取入站数据，支持**按特定定界符分帧**（`UntilDelimiter`）、**固定字节长度读取**（`FixedLength`）或**非阻塞拉取当前全部可用字节**（`AllAvailable`），并支持将字节流解码为纯文本（`Text`）或十六进制报文（`Hex`）。

## 2. 核心特性与工业痛点解决
- **三大工业分帧模式**：
  - `UntilDelimiter`：按常见换行符（`\n`、`\r\n`）或自定义帧尾标记精准切分帧，彻底解决流式 TCP 粘包/半包问题。
  - `FixedLength`：精准读取指定字节数（如 Modbus 响应、固定协议头），不多读也不少读。
  - `AllAvailable`：在指定超时时间内读取所有到达的字节流。
- **Text / Hex 双输出模式**：工控场景自动将二进制字节解码为标准空格分隔的 Hex 字符串（如 `"01 03 04 00 01 00 02 2B B9"`），避免乱码。
- **毫秒级超时控制**：可精确控制读取超时时间（`timeout_ms`）。

## 3. 平台支持与权限
- **支持平台**：`Windows` / `Linux` / `macOS` (跨平台通用)
- **管理员权限**：`否` (标准用户权限)

## 4. 参数说明

| 属性名称 | 参数类型 | 默认值 | 说明 |
| :--- | :--- | :--- | :--- |
| `socket` | `MixedType` | `"default_socket"` | 已连接的套接字句柄名称 |
| `read_mode` | `MixedType` | `"AllAvailable"` | 读取模式：`"UntilDelimiter"` / `"FixedLength"` / `"AllAvailable"` |
| `delimiter` | `MixedType` | `"\n"` | `UntilDelimiter` 模式下的帧结束分隔符 |
| `length` | `MixedType` | `1024` | `FixedLength` 模式下的期望读取字节数 |
| `data_mode` | `MixedType` | `"Text"` | 数据输出解码格式：`"Text"` (UTF-8 文本) 或 `"Hex"` (十六进制字符串) |
| `timeout_ms` | `MixedType` | `3000` | 读取超时时间（毫秒） |

## 5. 输出变量与上下文
- **返回值**：接收到的正文或 Hex 字符串。
- **上下文变量**：
  - `network_received_data`：接收到的数据字符串。
  - `network_received_bytes_count`：接收字节总数。

## 6. 典型应用场景
- **Modbus / 工业总线协议应答解析**：以 Hex 模式读取 PLC 返回的状态报文。
- **条码枪 / 测量仪器串口转网口数据提取**：以 `\r\n` 为界按行读取扫描到的标签编号。
