# self-rule

个人 Loon 分流规则仓库。参考 [blackmatrix7/ios_rule_script](https://github.com/blackmatrix7/ios_rule_script) 的规则格式及原 [fintech-rule](https://github.com/nintutu1015/fintech-rule) 的维护方式。一个服务一个纯文本 `.list`，不内置出口策略，不需要构建程序。

## 分类与订阅

| 分类 | 规则 | 建议策略 | Raw 订阅 |
| --- | --- | --- | --- |
| ai-rule | [ChatGPT](ai-rule/ChatGPT.list) | ChatGPT | [订阅](https://raw.githubusercontent.com/nintutu1015/self-rule/main/ai-rule/ChatGPT.list) |
| ai-rule | [Claude](ai-rule/Claude.list) | Claude | [订阅](https://raw.githubusercontent.com/nintutu1015/self-rule/main/ai-rule/Claude.list) |
| ai-rule | [Grok](ai-rule/Grok.list) | Grok | [订阅](https://raw.githubusercontent.com/nintutu1015/self-rule/main/ai-rule/Grok.list) |
| ai-rule | [Gemini](ai-rule/Gemini.list) | Gemini | [订阅](https://raw.githubusercontent.com/nintutu1015/self-rule/main/ai-rule/Gemini.list) |
| fin-rule | [Bybit_DE](fin-rule/Bybit_DE.list) | DE | [订阅](https://raw.githubusercontent.com/nintutu1015/self-rule/main/fin-rule/Bybit_DE.list) |
| fin-rule | [Bitkeep](fin-rule/Bitkeep.list) | HK | [订阅](https://raw.githubusercontent.com/nintutu1015/self-rule/main/fin-rule/Bitkeep.list) |
| fin-rule | [Bonify](fin-rule/Bonify.list) | DE | [订阅](https://raw.githubusercontent.com/nintutu1015/self-rule/main/fin-rule/Bonify.list) |
| fin-rule | [Polymarket](fin-rule/Polymarket.list) | TW | [订阅](https://raw.githubusercontent.com/nintutu1015/self-rule/main/fin-rule/Polymarket.list) |
| fin-rule | [AntBankHK](fin-rule/AntBankHK.list) | HK | [订阅](https://raw.githubusercontent.com/nintutu1015/self-rule/main/fin-rule/AntBankHK.list) |
| other-rule | [CathayPacific](other-rule/CathayPacific.list) | Proxy | [订阅](https://raw.githubusercontent.com/nintutu1015/self-rule/main/other-rule/CathayPacific.list) |
| other-rule | [Mosavi](other-rule/Mosavi.list) | Proxy | [订阅](https://raw.githubusercontent.com/nintutu1015/self-rule/main/other-rule/Mosavi.list) |
| other-rule | [UKCarrier](other-rule/UKCarrier.list) | UK | [订阅](https://raw.githubusercontent.com/nintutu1015/self-rule/main/other-rule/UKCarrier.list) |

## Loon 使用方法

1. 在 Loon 中建立 ChatGPT、Claude、Grok、Gemini 四个独立的 SELECT 策略组，各自选择已有节点或地区策略组；名称与 `policy=` 一致。金融及其他规则沿用旧策略名称（DE、HK、TW、Proxy、UK）。
2. 将 [loon/subscriptions.conf](loon/subscriptions.conf) 中的订阅行加到原配置的 `[Remote Rule]`，也可逐条订阅 Raw URL。该文件只包含本仓库订阅，并不是完整 Loon 配置。
3. 将四个 AI 规则排在第三方 AI、OpenAI、Claude、Google、Twitter、Proxy 等集合之前。Bybit_DE 排在通用 Bybit 之前。同一来源先匹配先执行；本地规则及插件也可能优先命中。
4. 替换原 fintech-rule 对应订阅，禁用重复的旧 OpenAI / Claude / Grok 等专属订阅；通用 AI 集合可保留作为其他 AI 的兜底，放在专属规则之后。原有第三方规则与插件不在本仓库自动迁移，也不需要删除。
5. 刷新远程规则，使用 Loon 请求日志确认四个服务分别命中对应策略。今后只需编辑对应 list 文件。

## 规则取舍

- ChatGPT：网页、API、静态资源、文件域名和部分明确专属的第三方主机。参考 [OpenAI 网络建议](https://help.openai.com/en/articles/9247338-network-recommendations-for-chatgpt-errors-on-web-and-apps)。不接管整个 Stripe、Auth0、Sentry 或 Cloudflare，也不接管共享 IP/ASN。语音的 UDP/IP 连接可能需单独验证。
- Claude：覆盖 Anthropic API、Claude 网页及 claudeusercontent。参考 [Claude 网络端点说明](https://support.claude.com/en/articles/13065128-claude-in-chrome-admin-controls)。
- Grok：覆盖 grok.com 和 x.ai（包含子域名与 API）；不接管整个 x.com 或 Twitter。X App 内嵌 Grok 的共享域名可能仍走 Twitter。
- Gemini：覆盖 Gemini、AI Studio、生成式语言 API 和相关专属后端。参考 [Gemini API 文档](https://ai.google.dev/gemini-api/docs)。不接管整个 google.com、googleapis.com、gstatic.com 或 apis.google.com。Google 登录、共享 CDN 等仍由原 Google 策略负责。
- 四套规则优先服务隔离，不保证覆盖所有第三方登录、支付、语音或共享资源；这些域名需要根据实际请求日志决定。规则未经过 Loon 实机验证。
- 原有金融规则保持原内容和策略建议。航空、Mosavi 和英国运营商归入 other-rule；旧仓库保留不变，旧订阅不会因此失效。

## 维护与来源

AI 域名按公开文档、原项目对应服务和实际服务边界选择，未整包复制上游规则。上游项目见 [blackmatrix7/ios_rule_script](https://github.com/blackmatrix7/ios_rule_script)，其原始文件遵循自身许可。旧自建规则来源于原 fintech-rule。域名会变化，本仓库目前手动维护，不设置自动同步任务。
