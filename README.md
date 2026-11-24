# clue-mcp：把「机会雷达」接进你的 AI · Connect Opportunity Loop to your AI

[机会雷达 Opportunity Loop](https://opportunity-loop.pages.dev/v3/) 每天从十几个公开数据源里找出突然涨起来的小产品
(App、小游戏、SaaS、Chrome 插件、Shopify 应用),逐个写简报：做什么、为什么是现在、谁做的、怎么赚钱、能不能持续、
一两个人该学什么。这个仓库是接入层：MCP 地址、Claude Code 插件、skill。服务器和数据采集不在这里。

Opportunity Loop finds small products that took off each day from a dozen public sources and writes a brief on each:
what it is, why now, who made it, how it earns, whether it lasts, and what a 1-2 person team should copy. This repo is
the connection layer: the MCP address, a Claude Code plugin and a skill. The server and data collection live elsewhere.

## 能问什么 What you can ask

- 最近一周宠物方向有哪些产品在涨？有没有个人开发者做的？ Which pet apps surged this week, any by a solo maker?
- 有没有刚上架就在赚钱、还没人跟的新 App? Any new apps that already earn and have no look-alike yet?
- 「AI 帮你排一周晚饭」这类 App 有多少个、谁赚到钱了、谁在买量？ Who makes "plan my week's dinners" apps, who earns, who buys ads?
- 这个 App(贴商店链接)怎么起来的？ How did this app (store link) take off?
- 我的工具 App 没人知道，有哪些能照着做的起量打法？ My tool app has no users: which growth tactics have real cases?

## 1. 拿 token · Get a token

每个人一个个人 token(`ol_…`),测试期每天 200 次调用。申请方式见
<https://opportunity-loop.pages.dev/v3/connect.html>。token 就是你的钥匙，别发到公开的地方。

Each person gets a personal token (`ol_…`), 200 calls a day during the beta. How to get one:
<https://opportunity-loop.pages.dev/v3/connect.html>. Treat it like a password.

## 2. 接上 · Connect

MCP 地址 Address (streamable HTTP):

```
https://clue-mcp.zhanxiaohui2.workers.dev/mcp/<your token>
```

**Claude Code**

```bash
claude mcp add --transport http --scope user clue https://clue-mcp.zhanxiaohui2.workers.dev/mcp/<token>
```

或者装插件(MCP + skill 一起装，安装时会问 token) · or install the plugin (asks for the token):

```
/plugin marketplace add flyfoxCI/clue-mcp
/plugin install clue@clue
```

**Cursor** `~/.cursor/mcp.json`

```json
{ "mcpServers": { "clue": { "url": "https://clue-mcp.zhanxiaohui2.workers.dev/mcp/<token>" } } }
```

**Codex** `~/.codex/config.toml`

```toml
[mcp_servers.clue]
url = "https://clue-mcp.zhanxiaohui2.workers.dev/mcp/<token>"
```

**ChatGPT 深度研究 / 只支持 `search` + `fetch` 的连接器** · ChatGPT deep research and other search/fetch-only connectors:

```
https://clue-mcp.zhanxiaohui2.workers.dev/mcp/oai/<token>
```

## 工具 Tools

全部只读 · All read-only.

| 工具 Tool | 做什么 What it does |
|---|---|
| `feed` | 每日越线的产品和简报(最新一天 / 某天 / 搜索 / 读全文) · daily picks and briefs |
| `early_apps` | 早鸟：近 90 天上架、已在赚钱、还没同款，带跟进判断 · early birds with a verdict |
| `directions` | 方向与切口、进场节奏 · directions, angles, launch pace |
| `search_apps` | 约 3.7 万个产品按词搜，可叠条件 · search ~37K products with filters |
| `get_app` | 一个产品的数字、早鸟判断、简报、走势 · one product in depth |
| `research_app` | 一个 App 的全部材料 + 待你自己查的清单 · everything on one app plus a to-do list |
| `plays` | 增长与变现打法，带案例和反例 · growth playbooks with cases |
| `status` | 数据日期和每个来源的边界 · data date and source limits |

回答是 Markdown:中文在前、英文在后，每个数字带来源，每个产品带网站页面链接。
Answers are Markdown, Chinese first then English, with a source on every number and a link to the website page.

## 数据边界 Data limits

- 收入和下载是模型估算(Appark 模型或按畅销榜名次估),只看量级 · revenue and downloads are model estimates
- 苹果不公开下载和收入;没上过榜的 App 不在库里 · Apple publishes no downloads or revenue; apps that never charted are not in the data
- 没查到广告 ≠ 没投广告 · no ads found is not proof of no ads
- 每个来源的详细边界：调用 `status`,或看 <https://opportunity-loop.pages.dev/v3/about.html>

## 许可 License

本仓库代码 MIT。网站上的文章、判断和数据版权归机会雷达所有，引用请注明出处并附链接。
Code in this repo: MIT. The site's briefs, judgments and data belong to Opportunity Loop; please cite with a link.
