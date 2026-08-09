# h3-reality-sni

H3 REALITY 部署脚本使用的 SNI 候选库。

## 数据文件

[`snis.json`](snis.json) 是唯一机器可读数据源，采用纯 JSON 字符串数组。每个元素都必须是
`domain:port` 格式，例如：

```json
[
  "api-de-prod.icloud.com:443",
  "activity.office.com:443"
]
```

当前列表共 66 条，端口均为 UDP 443。部署脚本会随机选择候选，并在实际使用前从 VPS
依次验证端口可达性、QUIC/HTTP/3 支持及 HTTP 状态码；任何验证失败都不会使用该候选。

## 收录规则

- 域名须能建立 QUIC/HTTP/3 连接。
- HTTP 状态码 `301`、`400` 或 `404` 的候选不收录。
- 每条记录必须包含显式端口，且不得重复。
- 列表只提供候选，最终是否可用以部署 VPS 的实时探测结果为准。

## 更新列表

修改 `snis.json` 后，请至少执行以下校验：

```bash
jq -e 'type == "array" and all(.[]; type == "string" and test("^[A-Za-z0-9.-]+:[0-9]+$"))' snis.json
```

## 许可证

MIT，见 [LICENSE](LICENSE)。
