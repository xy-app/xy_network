# P2P 信令与中继服务 (P2PServer)

启动 P2P 网络信令发现服务与加密流量中继中心。

## 运行参数

* **服务绑定地址与端口 (`bind_address`)**
  * 类型: `string`
  * 默认值: `"0.0.0.0:8080"`
  * 描述: 信令中继服务监听绑定的 IP 地址与端口号。
* **开启加密中继转发 (`enable_relay`)**
  * 类型: `bool`
  * 默认值: `true`
  * 描述: 是否允许受限环境下的客户端通过本节点进行流量中继。
* **静态资源目录 (可选) (`optional_root_folder`)**
  * 类型: `FolderPicker`
  * 默认值: `""`
  * 描述: 可选的本地文件资源管理器根目录。

## 输出

* 类型: `string`
* 描述: 信令中继服务监听状态提示。

## 使用示例

```json
{
  "tag": "P2PServer",
  "bind_address": "0.0.0.0:8080",
  "enable_relay": true,
  "optional_root_folder": ""
}
```