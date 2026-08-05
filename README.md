# h3-reality-sni

H3 REALITY 伪装 SNI 维护库（Curated H3 REALITY camouflage SNI list）

本仓库维护一份**全部经真实 Chrome --force-http3 验证可用**的 H3 REALITY 伪装 SNI 列表，供 [h3-reality-deploy](https://github.com/lipeiying032/h3-reality-deploy) 一键部署脚本在用户不填写 SNI 时随机挑选使用。

## 用途

- 机器可读数据：`snis.json`（部署脚本直接拉取解析）
- 部署脚本默认行为：用户直接回车 → 从本库随机挑一个 SNI → 自动走 H3 探测验证
- 避免使用大型泛 CDN / 国内不可达的 SNI 作为伪装目标

## 当前列表

完整列表以 [`snis.json`](snis.json) 为唯一数据源（机器可读，部署脚本直接拉取解析）。
当前共 3 条已验证 SNI，全部经真实 Chrome --force-http3 验证。

## SNI 准入标准（贡献指南）

新增 SNI 必须**全部**满足以下条件：

1. **不套大型泛 CDN**
   - 排除 akamaized.net 等泛 CDN 域名
   - 排除 google / youtube / facebook 等被墙域名
2. **必须真实 Chrome --force-http3 + netlog 确认 QUIC 会话建立才算支持 H3**
   - 以 `--headless=new --force-http3 --origin-to-force-quic-on=<host>:443 --log-net-log` 启动真实 Chrome，分析 netlog 中 `QUIC_SESSION_VERSION_NEGOTIATED`（会话建立）与 `HTTP_TRANSACTION_QUIC_SEND_REQUEST_HEADERS`（H3 请求发出）
3. **HTTP/HTTPS 通不代表 H3 通**
   - gspe35-ssl.ls.apple.com / xbox.com 教训：TCP/HTTPS 有响应（如 TCP 404）但无 `Alt-Svc: h3` 即无 H3，Chrome 强制 H3 时表现为 0x128 服务器拒绝或 ERR_QUIC_PROTOCOL_ERROR，此类 SNI 一律移除
4. **国内可达**：不被墙，国内网络可正常探测
5. **实测通过后才可加入**：必须真实 Chrome 验证通过，并在条目中注明测试日期

## 如何提交新 SNI

- 开 Issue：附上真实 Chrome `--force-http3` + netlog 中 `QUIC_SESSION_VERSION_NEGOTIATED` / `HTTP_TRANSACTION_QUIC_SEND_REQUEST_HEADERS` 的证据 + 测试日期
- 直接提 PR：修改 `snis.json` 并在 `note` 中注明测试日期与验证方式
- 或直接联系作者 lipeiying032

## 数据格式

`snis.json` 顶层字段：

- `version`：列表格式版本号
- `updated`：最近更新日期
- `description`：列表说明
- `maintainer`：维护者
- `snis`：条目数组，每条包含 `sni` / `note` / `tested`

## 许可证

MIT，见 [LICENSE](LICENSE)。
