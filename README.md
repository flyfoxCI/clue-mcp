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

打开 <https://clue-mcp.zhanxiaohui2.workers.dev/login>,用 GitHub 或 Google 登录，token 立刻显示(测试期免费，每天 200 次调用)。
页面上还有一段可以直接贴给 AI 的话，它会自己完成配置。我们只读取账号 id 和显示名，不读邮箱。

Sign in with GitHub or Google at <https://clue-mcp.zhanxiaohui2.workers.dev/login> and your token appears right away
(free during the beta, 200 calls a day), with a prompt you can paste to your AI so it sets itself up. We read only
your account id and display name. Treat the token like a password.

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

| 工具 Tool | 做什么 What it does |
|---|---|
| `feed` | 每日越线的产品和简报(最新一天 / 某天 / 搜索 / 读全文) · daily picks and briefs |
| `early_apps` | 早鸟：近 90 天上架、已在赚钱、还没同款，带跟进判断 · early birds with a verdict |
| `waves` | 赛道：一个跑出来的点子，谁先做的、谁跟上了(晚几个月)、小团队怎么切入 · waves: lead, followers, how to get in |
| `directions` | 方向/赛道：暴涨数、新进场数、赚钱的有几个、领头的；任意 App → 它的赛道和排名、按月进场节奏 · directions, from any app too |
| `search_apps` | 按意思搜(多语言向量)+ 按词搜，名字拼错也能找到，可叠条件 · meaning + word search, typo-tolerant |
| `list_apps` | 不用关键词：按条件筛选，按收入/增长/新增/上架/评分排序 · filter and sort without a search word |
| `get_app` | 一个产品：数字、评论与抱怨原话、各国榜单、版本、广告素材、同开发者、相似产品、走势、简报 · one product in depth |
| `compare_apps` | 2-4 个产品并排，含主要抱怨和原话 · side by side, with complaints |
| `research_app` | 一个 App 的全部材料 + 待你自己查的清单 · everything on one app plus a to-do list |
| `plays` | 增长与变现打法，带案例和反例 · growth playbooks with cases |
| `query_sql` | 只读 SQL,计数和分布(50 行，只读库令牌 + 语句检查) · read-only SQL for counts and cuts |
| `publish_board` | 把挑好的清单发布成可分享的网页 · publish a shortlist as a shareable page |
| `request_research` | 对还没调研过的 App 立即深度调研(约 2–4 分钟出报告页) · live deep report in 2-4 minutes |
| `status` | 数据日期和每个来源的边界 · data date and source limits |

除了 `publish_board` 和 `request_research` 会新建内容外，其余都只读。
All tools are read-only except `publish_board` and `request_research`, which create content.

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
