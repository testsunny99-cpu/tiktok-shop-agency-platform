# 开源项目调查：TikTok Shop 机构多账号运营平台

调查日期：2026-11-11。目标是寻找可复用的开源项目，而不是把第三方项目当作官方能力证明。TikTok Shop 的区域、应用类别、scope、配额和接口版本仍以 Partner Center 当前页面为准。

## 结论

目前没有发现一个成熟开源项目完整覆盖本项目的目标闭环：机构多账号台账、Creator 与 Seller/Shop 独立授权、带商品视频上传、视频预检、发布、批量编排、幂等/重试/审计和 Webhook/轮询追踪。

最有价值的组合是：

1. 用 `EcomPHP/tiktokshop-php` 或 `SKIT.FlurlHttpClient.ByteDance` 作为签名、OAuth、Token 刷新和基础 Seller API 的 SDK 参考；
2. 用 `Buer2333/tiktok-shop-mcp` 参考多店铺配置、Token 生命周期、默认只读和写操作闸门；
3. 用 `codustry/marketeer` 参考多店铺抽象和跨平台电商领域模型，但它仍处于早期设计阶段；
4. 用 `dmiller1006/tiktok-shop-docs-skill` 做官方 Partner Center 文档镜像与差异追踪；
5. 对 Creator Affiliate 视频工作流、账号隔离、任务状态机和审计仍需自研。

## 候选项目

| 项目 | 定位与可复用内容 | 与本项目的匹配度 | 主要缺口/风险 |
|---|---|---:|---|
| [EcomPHP/tiktokshop-php](https://github.com/EcomPHP/tiktokshop-php) | PHP SDK；OAuth、授权店铺、Token 刷新、签名和大量 Seller API 示例 | 中 | 面向 Shop/Seller API；没有机构租户、Creator 独立授权、视频预检/发布编排；需核对 API 版本与许可证 |
| [fudiwei/DotNetCore.SKIT.FlurlHttpClient.ByteDance](https://github.com/fudiwei/DotNetCore.SKIT.FlurlHttpClient.ByteDance) | .NET 强类型 ByteDance/TikTok Shop Open Platform SDK，跨平台异步 | 中 | SDK 不是运营平台；需逐接口核对当前 Partner Center 版本、区域和 scope |
| [Buer2333/tiktok-shop-mcp](https://github.com/Buer2333/tiktok-shop-mcp) | Python MCP；多店铺配置、Token 刷新、订单/商品/财务/分析；写操作默认关闭且有店铺 allowlist/dry-run | 中高（架构参考） | 主要是 Seller/Shop 查询和产品写入；不覆盖 Creator OAuth、shoppable video upload/precheck/post；凭证刷新会写本地 shops.json，需按本项目密钥政策改造 |
| [codustry/marketeer](https://github.com/codustry/marketeer) | Apache-2.0 的跨平台电商整合层；多 storefront、订单、库存、目录和统一消息模型 | 中（领域模型参考） | 明确处于 early days；没有 Creator/TSP 工作流、视频链路和 TikTok 特有授权隔离 |
| [oneflow-ai/n8n-nodes-tiktok-shop](https://github.com/oneflow-ai/n8n-nodes-tiktok-shop) | 结果更像 n8n starter/template，不能视为完成的 TikTok Shop 节点 | 低 | README 未证明已实现官方节点；不应作为核心依赖 |
| [dmiller1006/tiktok-shop-docs-skill](https://github.com/dmiller1006/tiktok-shop-docs-skill) | Partner Center 文档镜像、manifest、刷新和缺口检查 | 高（资料工具） | 不是业务系统；内容由 TikTok 文档组成，需本地刷新并按官方页面复核 |
| [yancyuu/tk-creator-binder](https://github.com/yancyuu/tk-creator-binder) | Chrome 扩展，批量绑定达人，去重、每日配额、诊断和本地 profile 隔离 | 低到中（运营流程参考） | UI 自动化而非官方 API；只覆盖绑定，页面改版和平台风控风险高；不应替代 API |
| [Buer2333/tiktok-shop-mcp](https://github.com/Buer2333/tiktok-shop-mcp) | 多店铺 AI 查询接口，支持 32 个工具和重试 | 中（任务/安全参考） | 与视频创作者发布目标不同；需审查依赖、许可证和 token 落盘行为 |

## 官方能力边界对照

本项目要实现的 Creator Affiliate 工作流至少涉及：

- Creator Authorization 与独立 `creator.video.write` scope；
- 获取店铺商品；
- 上传带货视频文件；
- 创建/查询视频预检任务；
- 发布带商品视频；
- 查询视频状态；
- Webhook 或轮询、幂等键、重试上限和审计记录。

候选开源项目的 README 和公开目录主要集中在 Seller/Shop 的订单、商品、库存、财务和分析；没有找到同时实现上述 Creator 视频链路的成熟项目。因此不能因为某个 SDK “支持 TikTok Shop” 就推断它支持本项目需要的 Affiliate Creator API。

## 建议采用方式

建议采用“薄 SDK + 自有编排层”，而不是 fork 一个完整平台：

- SDK 层只复用请求签名、OAuth/token refresh、错误映射和基础数据模型；
- 自有域模型强制区分 `creator_account` 与 `seller_shop`，Token 不共享；
- 自有任务层实现上传 → 预检 → 发布 → 状态追踪状态机，并以业务幂等键去重；
- 借鉴 MCP 项目的只读默认、写操作 allowlist 和 dry-run，但把密钥放到运行时 Secret Store，不写 `shops.json` 或日志；
- 对每个候选库先做许可证、最近提交、接口版本、区域和 scope 复核，再决定是否引入依赖。

## 不建议直接采用

- 浏览器自动化扩展作为正式发布链路；
- 声称“TikTok Shop API”的非官方 scraper 或未注明 Partner Center 授权的项目；
- 仅有 README 或 starter 模板、没有可验证实现的 n8n 节点；
- 把 Seller token 当作 Creator token，或让机构主账号替代创作者授权。

## 参考来源

- [TikTok Shop Partner Center Developer Guide / API Reference](https://partner.tiktokshop.com/docv2/page/678e3a2ac0833b03100a4689)
- [Affiliate Creator API overview](https://partner.tiktokshop.com/docv2/page/affiliate-creator-api-overview)
- [Upload shoppable video file](https://partner.tiktokshop.com/docv2/page/upload-shoppable-video-file-202505)
- [Create video precheck task](https://partner.tiktokshop.com/docv2/page/create-video-precheck-task-202511)
- [Post shoppable video](https://partner.tiktokshop.com/docv2/page/post-shoppable-video-202603)
