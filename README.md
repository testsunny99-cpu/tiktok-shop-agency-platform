# TikTok Shop 机构多账号运营平台

这是一个为 TikTok Shop TSP 机构设计的多账号运营平台。它的核心用途是：让机构在合规授权的前提下，集中管理大量创作者账号和卖家/店铺账号，并批量编排带货视频发布任务。

## 项目目的

机构不是只运营一个创作者，而是要持续接入、管理和运营一批账号。因此系统的核心对象是“账号池 + 授权关系 + 批量任务”，而不是单次 API 调用。平台需要让运营人员能够：

1. 绑定和查看机构旗下的创作者账号、卖家/店铺账号；
2. 独立维护每个账号的授权、Token 生命周期、区域和权限状态；
3. 为账号选择商品，上传视频，执行视频预检，并发布带商品视频；
4. 批量排程、限流、重试和追踪任务，避免重复发布；
5. 查看失败原因、账号风险、配额和发布结果，并保留完整审计记录。

## 当前范围

首个版本聚焦美国区 TSP 场景和 Affiliate Creator API：账号授权与台账、Token 管理、商品与视频任务、带商品视频发布、状态查询、批量编排、Webhook/轮询、审计与基础权限。

暂不包含：自动完成创作者入驻审核、绕过 TikTok 风控或配额、内容生成、结算分账、生产部署和真实账号发布。具体市场、账号规模、角色权限、数据保留期限和上线目标仍需确认，见 `docs/project/intake.md`。

## 文档入口

- [项目立项与范围](docs/project/intake.md)
- [项目概览](docs/project/overview.md)
- [系统架构](docs/project/architecture.md)
- [关键决策](docs/project/decisions.md)
- [里程碑](docs/project/milestones.md)
- [API 与授权资料](docs/project/api-reference.md)
- [开源项目调查报告](docs/project/open-source-landscape.md)
- [许可证](LICENSE)
- [AI 交接记录](AI_PROJECT_HANDOFF.md)

## OAuth 回调配置（重要）

TikTok Shop 后台的 **Redirect URL** 必须填写由本项目控制的 HTTPS 地址，例如：

```text
https://api.example.com/tiktok/oauth/callback
```

截图中出现的 `https://tts-callback.shop-oauth.workers.dev/callback` 是一个外部
Cloudflare `workers.dev` 子域，不属于本项目或本账号。除非已经确认该 Worker 的
所有者、代码和数据处理方式，否则不要把授权码或店铺授权交给它；应替换为自有
服务器或自有 Cloudflare Worker 的回调地址，并点击 **Publish changes**。

授权链接中的 `state` 由本项目在发起授权时随机生成并短期保存，回调时必须比较
返回值，匹配后才能用 `code` 换取 Token。`state` 不能固定写死，也不能只依赖
后台复制的授权链接模板。Creator 与 Seller 使用各自的授权入口和独立 Token。

回调地址和授权流程的操作清单见 [`docs/project/api-reference.md`](docs/project/api-reference.md)。

## 当前状态

处于 discovery。当前只完成文档初始化，尚无可运行服务或生产集成。所有运行命令和测试矩阵标记为 TBD，待技术选型和接口验证后补齐。
