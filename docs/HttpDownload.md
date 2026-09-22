# HTTP 文件下载 (HttpDownload)

## 1. 功能概述
`HttpDownload` 算子通过标准 HTTP/HTTPS 协议流式下载远程网络资源并安全写入本地磁盘，支持大文件下载、`Content-Disposition` 远程文件名智能提取、覆盖保护控制与自定义请求头。

## 2. 核心特性与工业痛点解决
- **流式大文件下载**：采用内存流式分块写入，不把整个文件一次性载入内存，轻松下载几个 GB 的大体积镜像与压缩包。
- **智能文件名推导**：若未指定 `filename`，算子将优先从 HTTP 响应头的 `Content-Disposition: attachment; filename="..."` 自动解析出真实文件名；若头部未提供，则自动从 URL 末尾提取有效文件名。
- **覆盖保护机制**：支持 `overwrite: false`，当本地目标文件已存在时跳过下载，节省带宽并保护已有数据。

## 3. 平台支持与权限
- **支持平台**：`Windows` / `Linux` / `macOS` (跨平台通用)
- **管理员权限**：`否` (标准用户权限)

## 4. 参数说明

| 属性名称 | 参数类型 | 默认值 | 说明 |
| :--- | :--- | :--- | :--- |
| `url` | `String` | `"https://"` | 远程文件下载的完整 HTTP/HTTPS 链接 |
| `folder` | `DirectoryPicker` | `""` | 下载文件存储的本地文件夹路径 |
| `filename` | `String` | `""` | 保存的文件名（留空则自动根据头部或 URL 推导） |
| `overwrite` | `Bool` | `true` | 若目标文件已存在是否强制覆盖 |
| `headers` | `MixedType` | `""` | 下载时携带的自定义请求头 (JSON 格式或 Key: Value) |
| `timeout` | `Number` | `60` | 下载总超时时间（秒） |

## 5. 输出变量与上下文
- **返回值**：下载结果 JSON 字符串：
  ```json
  {
    "status": "success",
    "saved_path": "C:/Downloads/package.zip",
    "file_size": 10485760,
    "elapsed_ms": 1200
  }
  ```
- **上下文变量**：
  - `download_saved_path`：实际保存的本地文件绝对路径。
  - `download_file_size`：下载文件总字节数。

## 6. 典型应用场景
- **定时批量下载网络报表**：从业务后台下载每日对账单或销售明细。
- **自动化软件补丁与固件获取**：无人值守拉取最新更新安装包。
