# TikTok Shop 机构多账号运营平台：开源项目调查报告

> **版本**：v1.0  
> **调查日期**：2026-11-11  
> **项目阶段**：discovery  
> **报告用途**：为后续技术选型、依赖引入和自研边界提供可复查依据。

## 目录

- [一、执行摘要](#一执行摘要)
- [二、项目目标与评估标准](#二项目目标与评估标准)
- [三、候选项目对比](#三候选项目对比)
- [四、官方能力边界](#四官方能力边界)
- [五、推荐组合与自研边界](#五推荐组合与自研边界)
- [六、风险与核验清单](#六风险与核验清单)
- [七、结论](#七结论)
- [八、参考来源](#八参考来源)

## 一、执行摘要

本次调查检索了 GitHub 上与 TikTok Shop API、Partner Center、Seller/Shop 管理、Creator 运营和多平台电商中台相关的开源项目，并对公开 README、项目定位和可见功能进行了对照。

**核心结论：目前没有发现一个成熟开源项目，能够完整覆盖本项目的目标闭环**：

> 机构多账号台账 + Creator/Seller 独立授权 + 带商品视频上传 + 视频预检 + 发布 + 批量编排 + 幂等/重试 + 审计 + Webhook/轮询追踪。

因此，最稳妥的路线是采用“**薄 SDK + 自有编排层**”：复用成熟项目中的 HTTP 签名、OAuth、Token 刷新和基础数据模型；把账号隔离、Creator 视频工作流、任务状态机、权限、审计和密钥治理留在本项目中实现。

## 二、项目目标与评估标准

### 2.1 本项目要解决的问题

平台面向 TikTok Shop TSP 机构，用于集中管理旗下创作者账号和卖家/店铺账号，并在合规授权前提下编排带商品视频的上传、预检、发布和状态追踪。

首版重点包括：

- Creator 与 Seller/Shop 账号分开建模；
- 每个账号独立 OAuth、Token 生命周期和区域/权限状态；
- 商品选择、视频上传、预检、发布和状态查询；
- 批量排程、限流、重试和失败原因记录；
- 幂等键、任务状态机、Webhook/轮询和完整审计。

### 2.2 评估维度

每个候选项目按以下维度判断：

1. **官方 API 相关性**：是否明确对接 TikTok Shop Partner Center/Open API；
2. **Creator 能力**：是否覆盖 Creator 授权和 Affiliate 视频链路；
3. **Seller 能力**：是否覆盖店铺、商品、订单、库存或财务；
4. **多账号能力**：是否支持多店铺、多凭证和租户隔离；
5. **任务可靠性**：是否有重试、幂等、限流、状态和审计设计；
6. **可复用性**：语言、许可证、活跃度、文档和依赖质量；
7. **合规风险**：是否依赖浏览器自动化、抓取、非官方接口或凭证落盘。

## 三、候选项目对比

| 项目 | 定位与可复用内容 | 匹配度 | 主要缺口或风险 |
|---|---|---:|---|
| [EcomPHP/tiktokshop-php](https://github.com/EcomPHP/tiktokshop-php) | PHP SDK；OAuth、授权店铺、Token 刷新、签名和 Seller API 示例 | 中 | 主要面向 Shop/Seller API；没有机构租户、Creator 独立授权和视频预检/发布编排；需核对版本与许可证 |
| [SKIT.FlurlHttpClient.ByteDance](https://github.com/fudiwei/DotNetCore.SKIT.FlurlHttpClient.ByteDance) | .NET 强类型 ByteDance/TikTok Shop SDK，跨平台、异步 | 中 | SDK 不是运营平台；需逐接口核对 Partner Center 版本、区域和 scope |
| [tiktok-shop-mcp](https://github.com/Buer2333/tiktok-shop-mcp) | Python MCP；多店铺配置、Token 刷新、订单/商品/财务/分析、重试、只读默认和写操作闸门 | 中高（架构参考） | 主要是 Seller/Shop 查询和产品写入；不覆盖 Creator OAuth、shoppable video upload/precheck/post；需审查 Token 刷新后的本地落盘行为 |
| [Marketeer](https://github.com/codustry/marketeer) | Apache-2.0 跨平台电商整合层；多 storefront、库存、订单、目录和统一消息模型 | 中（领域模型参考） | 项目明确处于 early days；没有 Creator/TSP 视频工作流和 TikTok 特有授权隔离 |
| [tiktok-shop-docs-skill](https://github.com/dmiller1006/tiktok-shop-docs-skill) | Partner Center 文档镜像、manifest、刷新和缺口检查 | 高（资料工具） | 不是业务系统；需要本地刷新，并以官方页面复核当前区域、scope 和接口版本 |
| [tk-creator-binder](https://github.com/yancyuu/tk-creator-binder) | Chrome 扩展；批量绑定达人、去重、每日配额、诊断和 profile 隔离 | 低到中（流程参考） | UI 自动化而非官方 API；页面改版、账号风控和平台策略变化风险高 |
| [n8n-nodes-tiktok-shop](https://github.com/oneflow-ai/n8n-nodes-tiktok-shop) | 公开仓库 README 更接近 n8n starter/template | 低 | 没有足够证据证明已完成可用的官方 TikTok Shop 节点，不应作为核心依赖 |

### 3.1 最值得优先研究的三个项目

**第一优先：tiktok-shop-mcp。** 它对多店铺、Token 生命周期、重试、只读默认和写操作 allowlist 的处理，最接近本项目的安全运营思路。但它仍然偏 Seller/Shop 查询，不能替代 Creator Affiliate 视频链路。

**第二优先：EcomPHP/tiktokshop-php。** 它适合参考授权、签名、Token 刷新和基础 API 调用方式。即使最终技术栈不是 PHP，也可以把它当作协议实现的交叉校验来源。

**第三优先：tiktok-shop-docs-skill。** 它可以减少手工维护 Partner Center 文档索引的成本，适合放在 discovery 和后续接口变更跟踪流程中。

## 四、官方能力边界

本项目需要的 Creator Affiliate 工作流至少包括：

1. Creator Authorization 与独立 `creator.video.write` scope；
2. 获取店铺商品；
3. 上传带货视频文件；
4. 创建视频预检任务；
5. 获取视频预检结果；
6. 发布带商品视频；
7. 查询带货视频状态；
8. 使用 Webhook 或轮询追踪结果。

候选项目的公开资料主要集中在 Seller/Shop 的订单、商品、库存、财务和分析。没有找到同时实现上述 Creator 视频链路的成熟开源项目。因此，不能因为某个 SDK 宣称“支持 TikTok Shop”，就推断它支持本项目需要的 Affiliate Creator API。

还需要持续核验以下官方限制：

- 注册地区、目标市场和应用类别；
- Creator 与 Seller 所需 scope 是否可同时申请；
- 视频上传文件格式、大小、时长和处理时限；
- 预检与发布的配额、频率限制和错误码；
- Webhook 事件范围、签名校验和重放防护；
- 测试账号与生产账号的差异。

## 五、推荐组合与自研边界

### 5.1 可以复用的部分

- HTTP 请求签名和公共请求封装；
- OAuth 授权 URL、回调和 Token 刷新流程参考；
- Seller/Shop 的商品、订单、库存和财务数据模型；
- 多店铺配置、重试和只读/写操作闸门的设计思路；
- Partner Center 文档镜像和版本差异追踪。

### 5.2 必须自研的部分

- `creator_account` 与 `seller_shop` 的独立数据模型；
- Creator 和 Seller 不共享 Token 的凭证边界；
- 视频上传 → 预检 → 发布 → 状态追踪状态机；
- 批量任务幂等键、重试上限、限流、死信和失败原因；
- Webhook 验签、去重、重放防护和审计日志；
- 机构租户、角色权限、审批和发布闸门；
- 密钥管理、脱敏日志和凭证轮换。

### 5.3 建议的系统边界

```text
官方 API / SDK 适配层
        ↓
账号与授权域（Creator / Seller 分离）
        ↓
商品与视频资产域
        ↓
预检 / 发布任务编排器
        ↓
Webhook、轮询、重试与审计
        ↓
运营后台与报表
```

## 六、风险与核验清单

在引入任何外部项目之前，应完成以下核验：

- [ ] 确认许可证是否允许当前项目使用、修改和再发布；
- [ ] 查看最近提交、Issue、Release 和依赖漏洞；
- [ ] 对照 Partner Center 当前 API 版本和接口路径；
- [ ] 确认是否使用官方 Open API，而不是抓取或逆向接口；
- [ ] 确认 Token、App Secret、授权码不会写入 Git、日志或截图；
- [ ] 确认 Creator/Seller 凭证不会被混用；
- [ ] 用测试账号验证 OAuth、商品查询、视频上传、预检和状态查询；
- [ ] 在真实发布前增加人工审批和明确的生产授权开关。

特别需要避免：

- 用浏览器自动化扩展替代正式 API 发布链路；
- 把 Seller token 当作 Creator token；
- 把机构主账号 Token 当作创作者授权；
- 采用没有官方文档依据的 scraper 或第三方“万能 API”；
- 把刷新后的 Token 直接写入普通 JSON 文件。

## 七、结论

本次调查的结果不是“找到一个可以直接部署的开源平台”，而是确定了合理的技术组合：

- **SDK 层**：优先参考 `tiktokshop-php` 和 `SKIT.FlurlHttpClient.ByteDance`；
- **多店铺与安全策略**：参考 `tiktok-shop-mcp`；
- **跨平台领域模型**：参考 `Marketeer`；
- **官方资料维护**：采用 `tiktok-shop-docs-skill` 的镜像思路；
- **Creator 视频工作流和机构级治理**：由本项目自研。

项目后续应先完成 API-only discovery spike：使用两个测试 Creator 账号分别完成 OAuth、商品查询、视频上传、预检、发布和状态追踪，记录脱敏响应、配额和错误码，再决定 SDK 依赖和实现语言。

## 八、参考来源

### 官方资料

- [TikTok Shop Partner Center Developer Guide / API Reference](https://partner.tiktokshop.com/docv2/page/678e3a2ac0833b03100a4689)
- [Affiliate Creator API overview](https://partner.tiktokshop.com/docv2/page/affiliate-creator-api-overview)
- [Upload shoppable video file](https://partner.tiktokshop.com/docv2/page/upload-shoppable-video-file-202505)
- [Create video precheck task](https://partner.tiktokshop.com/docv2/page/create-video-precheck-task-202511)
- [Get shoppable video precheck result](https://partner.tiktokshop.com/docv2/page/get-shoppable-video-precheck-result-202601)
- [Post shoppable video](https://partner.tiktokshop.com/docv2/page/post-shoppable-video-202603)
- [Get shoppable video status](https://partner.tiktokshop.com/docv2/page/get-shoppable-video-status-202509)

### 开源项目

- [EcomPHP/tiktokshop-php](https://github.com/EcomPHP/tiktokshop-php)
- [fudiwei/DotNetCore.SKIT.FlurlHttpClient.ByteDance](https://github.com/fudiwei/DotNetCore.SKIT.FlurlHttpClient.ByteDance)
- [Buer2333/tiktok-shop-mcp](https://github.com/Buer2333/tiktok-shop-mcp)
- [codustry/marketeer](https://github.com/codustry/marketeer)
- [dmiller1006/tiktok-shop-docs-skill](https://github.com/dmiller1006/tiktok-shop-docs-skill)
- [yancyuu/tk-creator-binder](https://github.com/yancyuu/tk-creator-binder)
- [oneflow-ai/n8n-nodes-tiktok-shop](https://github.com/oneflow-ai/n8n-nodes-tiktok-shop)
