---
name: clue
description: 机会雷达 Opportunity Loop：每天哪些小产品(App、小游戏、SaaS、插件)突然涨起来、为什么、一两个人怎么跟。用户问「现在做什么有机会」「这个点子有没有人赚到钱」「这个 App 怎么起来的、在不在买量」「最近什么在涨」「有没有刚冒头还没人跟的新产品」「我的产品怎么拿到用户」，或要一份每日/每周情报时使用。Use when the user asks what small product to build, whether an idea makes money, how an app grew or whether it buys ads, what is taking off now, or wants a daily or weekly digest.
version: 0.2.0
homepage: https://opportunity-loop.pages.dev/v3/
---

# 机会雷达 Opportunity Loop

网站 https://opportunity-loop.pages.dev/v3/ 每天从十几个公开数据源里找出「越线」的小产品(榜单跳升、评分或安装突然加速、收入、广告),
逐个写简报，再归纳成方向、切口和打法。这个 skill 配合 `clue` MCP 服务器使用。

The site finds small products that crossed a line each day (chart jumps, rating or install surges, revenue, ads), writes
a brief on each, and rolls them up into directions, angles and playbooks. This skill works with the `clue` MCP server.

## 工具 Tools

| 工具 Tool | 用途 What for |
|---|---|
| `feed` | 每日越线：最新一天的入选产品和简报；`day` 看某天，`q` 搜全部简报，`id` 读一篇全文。Daily picks and briefs. |
| `early_apps` | 早鸟：近 90 天上架、已经在赚钱、库里还没同款的新 App,带「可直接跟进 / 有门槛 / 不建议抄」。Early birds with a verdict. |
| `directions` | 方向/赛道：暴涨数、90 天新进场、赚钱的数量、领头的；`key` 打开一个(头部、新进场还在赚钱的、按月进场节奏、切口);`app` 从任意 App 进它的赛道。Directions. |
| `search_apps` | 按意思搜(任何语言，一句话描述它做什么)+ 按词搜，名字拼错也行；可叠 store、min_rev、max_age_days、small_team、has_ads 等。Meaning + word search. |
| `list_apps` | 不用关键词，按条件筛选再排序:sort=revenue / growth / new / newest / rating / size。Filter and sort. |
| `get_app` | 一个产品。默认紧凑；`sections` 可加 reviews(星级分布、抱怨分组和原话)、charts(各国榜单)、versions、ads(广告素材)、developer、similar、history、brief、description、all。One product. |
| `compare_apps` | 2-4 个产品并排：收入、下载、增长、榜单国家、广告、发版节奏、早鸟判断、主要抱怨。Side by side. |
| `research_app` | 一次拿齐一个 App 的全部材料，外加「还需要你自己查什么」清单。Everything on one app. |
| `plays` | 打法：增长和变现做法，带步骤、案例、反例；`case_q` 搜真实案例。Playbooks and cases. |
| `query_sql` | 只读 SQL(DuckDB,一条 SELECT,最多 50 行)。工具描述里有表结构。用于计数、分布、交叉对比。Read-only SQL. |
| `publish_board` | 把挑好的 3-30 个产品发布成可分享网页，返回链接(数字取自我们的数据)。Publish a shortlist page. |
| `request_research` | 对还没有深度报告的 App 申请调研，第二天的每日运行会写出来；每人每天 3 个。Queue a deep report. |
| `status` | 数据日期和每个来源的边界。先看它再引用数字。Data date and each source's limits. |

## 常用配合 Workflows

1. **宽泛找方向 Find a direction.** 并行调 `feed()`、`early_apps()`、`directions()`;挑两三个值得深挖的，
   用 `directions(key=...)` 和 `get_app(...)` 展开，再结合用户自己会什么、有多少时间给建议。
2. **验一个点子 Check an idea.** `search_apps(q="<一句话说它做什么>")` 看谁在做、赚不赚钱、多久前上架;
   用 `compare_apps` 把领头的和最新进场的并排，`get_app(sections="reviews")` 看用户在抱怨什么——抱怨就是切口。
3. **拆一个竞品 Take a competitor apart.** `research_app(app="<商店链接或名字>")`,然后按清单自己联网补齐。
   没有深度报告的，可以 `request_research`。
4. **筛一批 Screen a batch.** `list_apps(store=..., max_age_days=180, min_rev=50000, small_team=true, sort="growth")`;
   要计数或分布用 `query_sql`。挑好了用 `publish_board` 发给用户一个链接。
5. **找打法 Find a growth tactic.** `plays()` 看清单，`plays(id=...)` 读全文，`plays(case_q="<做法关键词>")` 搜案例。
6. **写日报/周报 Write a digest.** `feed()` 拉最新一天，按用户关心的方向补 `directions(key=...)` 和 `early_apps(q=...)`。

## 引用规则 Rules for quoting

- 收入和下载都是模型估算(Appark 模型或我们按畅销榜名次估的),只看量级;每个数字带上来源。
  Revenue and downloads are model estimates: quote them as orders of magnitude, with their source.
- 「没查到广告」≠「没投广告」。An attribution SDK (Adjust, AppsFlyer) means it likely buys ads; it is not proof of spend.
- 回答里给出网站页面链接(工具结果里都有),方便用户点开核对。Link the website pages the tools return.
- 每个 token 每天 200 次调用;先用列表工具筛，再对少数产品深挖。200 calls a day per token: list first, then go deep on a few.
