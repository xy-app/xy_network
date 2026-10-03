# P2P 文件共享节点 (P2PHost)

共享本地文件夹并通过点对点直连或服务端中继进行分布式文件分发与传输。

## 运行参数

* **共享本地目录路径 (`folder_path`)**
  * 类型: `FolderPicker`
  * 默认值: `""`
  * 描述: 准备在局域网或公网中对外共享的本地文件夹。
* **允许加密中继回退 (`relay_fallback`)**
  * 类型: `bool`
  * 默认值: `true`
  * 描述: 在无法直连穿透时是否自动回退至服务器加密中继。
* **信令服务器地址 (`server_url`)**
  * 类型: `string`
  * 默认值: `"ws://127.0.0.1:8080/ws"`
  * 描述: 用于节点发现与握手的中央信令服务地址。
* **配对提取码 (`share_code`)**
  * 类型: `string`
  * 默认值: `"default"`
  * 描述: 远端节点下载拉取数据所需的唯一识别提取码。

## 输出

* 类型: `string`
* 描述: 节点启动运行状态提示。

## 使用示例

```json
{
  "tag": "P2PHost",
  "folder_path": "D:/shared_docs",
  "relay_fallback": true,
  "server_url": "ws://signaling.example.com/ws",
  "share_code": "share_98765"
}
```