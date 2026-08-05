# h3-reality-sni

H3 REALITY 伪装 SNI 维护库（Curated H3 REALITY camouflage SNI list）

本仓库维护一份**手工实测可用**的 H3 REALITY 伪装 SNI 列表，供 [h3-reality-deploy](https://github.com/lipeiying032/h3-reality-deploy) 一键部署脚本在用户不填写 SNI 时随机挑选使用。

## 用途

- 机器可读数据：`snis.json`（部署脚本直接拉取解析）
- 部署脚本默认行为：用户直接回车 → 从本库随机挑一个 SNI → 自动走 H3 探测验证
- 避免使用大型泛 CDN / 国内不可达的 SNI 作为伪装目标

## 当前列表

| SNI | 说明 | 测试日期 |
| --- | --- | --- |
| gspe35-ssl.ls.apple.com | Apple GSPE 边缘 | 2026-08-05 |
| tv.apple.com | Apple TV | 2026-08-05 |
| gateway.icloud.com | iCloud 网关 | 2026-08-05 |
| xbox.com | Xbox | 2026-08-05 |

## SNI 准入标准（贡献指南）

新增 SNI 必须**全部**满足以下条件：

1. **不套大型泛 CDN**
   - 排除 akamaized.net 等泛 CDN 域名
   - 排除 google / youtube / facebook 等被墙域名
2. **H3/QUIC 端点可用**：用 `probe-h3-sni` 验证完整握手（任何 HTTP 响应即视为支持）
3. **国内可达**：不被墙，国内网络可正常探测
4. **实测通过后才可加入**：必须真实部署/探测通过，并在条目中注明测试日期

## 如何提交新 SNI

- 开 Issue：附上 `probe-h3-sni -sni <域名>` 的成功探测输出 + 测试日期
- 直接提 PR：修改 `snis.json` 并在 `note` 中注明测试日期
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
