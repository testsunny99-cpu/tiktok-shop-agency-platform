# 里程碑

## 2026-11-11 — 项目初始化与目标澄清

Status: completed
Evidence: 已创建项目治理骨架；已整理用户提供的 API/授权/Webhook 资料；已明确机构多账号目标与账号隔离原则。
Docs updated: `README.md`, `docs/project/intake.md`, `docs/project/overview.md`, `docs/project/architecture.md`, `docs/project/decisions.md`, `docs/project/api-reference.md`
Known risks: 区域与应用类别可用性、创作者授权规模、配额、Token 刷新和技术栈均待验证。
Next node: 使用测试账号完成 OAuth 与单账号视频发布闭环，并记录真实接口响应。

## 下一阶段 — API 闭环验证

Status: planned
Goal: 在不触碰生产账号的前提下，完成多创作者授权、商品查询、视频上传、预检、发布和状态查询。
Exit criteria: 至少两个测试创作者账号独立授权；至少一个批量任务出现可解释的成功和失败；重试与幂等验证通过。

## MVP — 机构账号池与批量任务

Status: planned
Goal: 形成可操作的账号台账、任务队列、限流、审计和基础运营台。
Exit criteria: 运营人员无需直接调用 API 即可完成测试环境批量发布；敏感凭证无泄漏。

## 试点 — 小规模真实账号

Status: planned
Goal: 经机构负责人批准，在有限账号、有限视频和可暂停队列下试运行。
Exit criteria: 失败率、配额、人工介入、成本和风控指标达标。

## 规模化

Status: planned
Goal: 扩展账号数量、角色权限、监控、灾备和多市场支持。
Exit criteria: 资源容量、审计、合规和回滚流程经过演练。
