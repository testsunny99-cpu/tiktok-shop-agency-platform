# Project Intake

## Mandatory to start

- Project name: `TikTok Shop 机构多账号运营平台`
- Project code: `tiktokshop-agency-multi-account`
- Goal in one sentence: 为 TikTok Shop TSP 机构提供一个可审计的平台，批量管理旗下创作者与卖家/店铺账号，并批量发布带商品视频。
- Target user or internal consumer: TSP 机构的运营人员、账号管理员、内容/发布团队和技术管理员。
- Initial scope: 美国区优先；账号接入与台账、创作者 OAuth、卖家/店铺授权记录、Token 生命周期、商品选择、视频上传/预检/发布、批量任务、状态追踪、限流重试、Webhook/轮询、审计记录。
- Explicit exclusions: 自动创作者入驻审核；绕过平台风控、配额或地区限制；内容生成；结算和佣金分账；未授权账号操作；生产部署。
- First acceptance criteria: 能在测试环境绑定多个创作者账号；每个账号有独立授权状态；创建批量视频任务后可逐账号上传、发布、查询终态；重复任务不会造成重复发布；失败任务有可读原因和审计记录；真实 Secret 不进入仓库。
- Current stage: `discovery`
- Reference objects: 用户提供的 `tiktokshop_api.txt`、美国区 TSP 服务入口、TikTok Shop Partner Center Developer Guide、Affiliate Creator API 文档、Webhook 文档。

## Recommended

- Repository setup mode: `new`, initialized locally on 2026-11-11
- Remote URL: `TBD`
- Deadline or review date: `TBD`
- Target platforms and runtime versions: `TBD`；建议先做 Web 管理台 + 后端任务服务
- Technical preferences and constraints: TikTok Shop OpenAPI/OAuth；需支持批量任务、幂等、限流、审计和密钥隔离
- Dependencies and external services: TikTok Shop Partner Center、TikTok Shop OpenAPI、OAuth、Webhook；数据库、队列和对象存储 `TBD`
- Design or competitor references: TSP Partner Center；具体竞品 `TBD`
- Environment matrix: `local | staging | production`，当前仅 local 文档阶段
- Deployment target and rollback expectation: `TBD`；发布接口必须具备人工开关和可暂停队列
- Test strategy: 先用测试店铺/测试账号验证授权、签名、上传、预检、发布和状态机；真实账号发布前需要单独批准
- Budget/cost limit: `TBD`
- Known risks and open questions: 区域/应用类别可用性、创作者授权规模、Token 刷新策略、API 配额、视频存储成本、Webhook 可靠性、账号风控和数据合规均需验证。

## Assumptions recorded for initialization

1. 初始目标市场按用户提供的美国区服务链接暂定为 US；需要机构确认是否同时支持 UK/SEA/其他市场。
2. “账号”至少包含 Creator 和 Seller/Shop 两类；若机构实际只需要 Creator，Seller 模块可在架构上保留、MVP 中关闭。
3. 系统面向机构内部运营，不默认做成对外 SaaS；多租户和客户隔离是否需要，标记为 TBD。
