# clash-rules-lite

精简版 Clash 规则集，专为**内存受限路由器**（≤1 GB RAM）优化。

基于 [Loyalsoldier/clash-rules](https://github.com/Loyalsoldier/clash-rules) 的数据源，激进过滤冗余条目，总规则数从 **32 万条 → 约 2.5 万条**，路由器 mihomo 内存基线从 ~280 MB 降至 ~60 MB。

## 精简策略

| 文件 | 原版条数 | 精简后 | 策略 |
|------|---------|--------|------|
| `reject.yaml` | 167,851 | ~5,000 | 替换为 [anti-AD](https://anti-ad.net) 精选中文广告列表 |
| `proxy.yaml` | 26,784 | ~4,000 | 替换为 GFW 封锁列表（真正被墙的域名）|
| `direct.yaml` | 116,609 | ~15,000 | 只保留根域名（`baidu.com`），去除冗余子域名条目（`cdn.baidu.com` 已被 `+.baidu.com` 覆盖）|
| `cncidr.yaml` | 同原版 | 同原版 | IP 段列表本身已经很小 |
| 其余小文件 | 同原版 | 同原版 | apple/google/icloud/private/telegram 本身条数少 |

## 订阅地址（rule-provider YAML 格式）

将你的订阅配置中的 `rule-providers` URL 替换为以下地址：

```yaml
rule-providers:
  reject:
    type: http
    behavior: domain
    url: "https://github.com/wujun4code/clash-rules-lite/releases/latest/download/reject.yaml"
    path: ./rule_provider/reject.yaml
    interval: 86400

  proxy:
    type: http
    behavior: domain
    url: "https://github.com/wujun4code/clash-rules-lite/releases/latest/download/proxy.yaml"
    path: ./rule_provider/proxy.yaml
    interval: 86400

  direct:
    type: http
    behavior: domain
    url: "https://github.com/wujun4code/clash-rules-lite/releases/latest/download/direct.yaml"
    path: ./rule_provider/direct.yaml
    interval: 86400

  cncidr:
    type: http
    behavior: ipcidr
    url: "https://github.com/wujun4code/clash-rules-lite/releases/latest/download/cncidr.yaml"
    path: ./rule_provider/cncidr.yaml
    interval: 86400

  lancidr:
    type: http
    behavior: ipcidr
    url: "https://github.com/wujun4code/clash-rules-lite/releases/latest/download/lancidr.yaml"
    path: ./rule_provider/lancidr.yaml
    interval: 86400

  telegramcidr:
    type: http
    behavior: ipcidr
    url: "https://github.com/wujun4code/clash-rules-lite/releases/latest/download/telegramcidr.yaml"
    path: ./rule_provider/telegramcidr.yaml
    interval: 86400

  apple:
    type: http
    behavior: domain
    url: "https://github.com/wujun4code/clash-rules-lite/releases/latest/download/apple.yaml"
    path: ./rule_provider/apple.yaml
    interval: 86400

  google:
    type: http
    behavior: domain
    url: "https://github.com/wujun4code/clash-rules-lite/releases/latest/download/google.yaml"
    path: ./rule_provider/google.yaml
    interval: 86400

  icloud:
    type: http
    behavior: domain
    url: "https://github.com/wujun4code/clash-rules-lite/releases/latest/download/icloud.yaml"
    path: ./rule_provider/icloud.yaml
    interval: 86400

  private:
    type: http
    behavior: domain
    url: "https://github.com/wujun4code/clash-rules-lite/releases/latest/download/private.yaml"
    path: ./rule_provider/private.yaml
    interval: 86400
```

## 更新频率

GitHub Actions 每天 UTC 02:00（北京时间 10:00）自动拉取上游最新数据重新生成。

## 数据来源

- 广告拦截：[anti-AD](https://anti-ad.net) — 专注中文互联网，更新及时，质量高
- GFW 列表：[Loyalsoldier/v2ray-rules-dat](https://github.com/Loyalsoldier/v2ray-rules-dat)
- 直连/IP 段：[Loyalsoldier/v2ray-rules-dat](https://github.com/Loyalsoldier/v2ray-rules-dat) + [Loyalsoldier/geoip](https://github.com/Loyalsoldier/geoip)
- Apple/Google：[felixonmars/dnsmasq-china-list](https://github.com/felixonmars/dnsmasq-china-list)
- iCloud/Private：[Loyalsoldier/domain-list-custom](https://github.com/Loyalsoldier/domain-list-custom)
