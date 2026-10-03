# 域名解析查询 (DomainQuery)

调用 DNS 解析服务，将目标域名查询解析为对应的 IP 地址与网络服务信息。

## 运行参数

* **主机节点名/域名 (`p_node_name`)**
  * 类型: `string`
  * 默认值: `"localhost"`
  * 描述: 待解析的目标主机名或域名 (如 `www.baidu.com`)。
* **服务名或端口号 (`p_service_name`)**
  * 类型: `string`
  * 默认值: `"http"`
  * 描述: 网络服务名称或端口号 (如 `http`、`https`、`80`、`443`)。

## 输出

* 类型: `string`
* 描述: 解析出的 IP 地址列表及首选连接地址信息。

## 使用示例

```json
{
  "tag": "DomainQuery",
  "p_node_name": "www.baidu.com",
  "p_service_name": "443"
}
```