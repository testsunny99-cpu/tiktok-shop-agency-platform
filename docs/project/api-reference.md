# API 与授权资料

资料核对日期：2026-11-11。以下链接来自用户提供的资料或 TikTok Shop Partner Center 官方文档。区域、应用类别和 scope 以调用时官方页面为准。

## 用户提供的入口

- 美国区 TSP 服务入口：<https://partner.us.tiktokshop.com/service/gather?service_id=7699245963517724429&from=service_list>
- Developer Guide / Postman：<https://partner.tiktokshop.com/docv2/page/678e3a2ac0833b03100a4689>
- Webhook 使用方法：<https://partner.tiktokshop.com/docv2/page/650512b42f024f02be19755f>
- Creator Authorization：`https://shop.tiktok.com/alliance/creator/auth?app_key=${TTS_APP_KEY}`
- Seller Authorization：<https://services.tiktokshops.us/open/authorize?service_id=7699245963517724429>

## 官方 AI Toolkit

用户资料中提供的官方工具包安装命令：

```bash
npm install -g @tts-open-toolkit/cli
tts_open_toolkit skill add --agent codex
```

安装前应确认来源、版本和本机 Node.js 环境；真实凭证仍只通过本地运行时配置提供。

## Affiliate Creator API

- 总览：<https://partner.tiktokshop.com/docv2/page/affiliate-creator-api-overview>
- 获取创作者资料：<https://partner.tiktokshop.com/docv2/page/get-creator-profile-202508>
- 获取店铺商品：<https://partner.tiktokshop.com/docv2/page/get-shop-products-202509>
- 上传带货视频文件：<https://partner.tiktokshop.com/docv2/page/upload-shoppable-video-file-202505>
- 创建视频预检任务：<https://partner.tiktokshop.com/docv2/page/create-video-precheck-task-202511>
- 获取视频预检结果：<https://partner.tiktokshop.com/docv2/page/get-shoppable-video-precheck-result-202601>
- 发布带商品视频：<https://partner.tiktokshop.com/docv2/page/post-shoppable-video-202603>
- 查询带货视频状态：<https://partner.tiktokshop.com/docv2/page/get-shoppable-video-status-202509>
- 集成方案：<https://partner.tiktokshop.com/docv2/page/ng0l9170>

## 授权与 API 基础

- 授权指南：<https://partner.tiktokshop.com/docv2/page/authorization-guide-202309>
- 获取已授权店铺：<https://partner.tiktokshop.com/docv2/page/get-authorized-shops-202309>
- 请求签名：<https://partner.tiktokshop.com/docv2/page/sign-your-api-request>
- API 基础域名：`https://open-api.tiktokglobalshop.com`
- Token 域名：`https://auth.tiktok-shops.com`

## OAuth 回调地址与 `state`

后台的 Redirect URL 是 OAuth 授权完成后的回调入口，不是 Webhook 地址。它必须
指向本项目实际控制的 HTTPS 服务，并与后台配置逐字符一致。例如：

```text
https://api.example.com/tiktok/oauth/callback
```

`https://tts-callback.shop-oauth.workers.dev/callback` 使用的是 Cloudflare
`workers.dev` 公共子域；当前没有证据表明它属于本项目，因此应视为外部测试或第三方
服务，不能用于正式授权。若要使用 Cloudflare Worker，必须部署在自己的 Cloudflare
账户下，并确认回调代码、日志和数据存储由本项目控制。

发起授权时由服务端生成不可预测且短期有效的 `state`，绑定到当前会话和账号，拼接
到授权 URL；回调时校验 `state` 后再使用 `code` 换取 Token。示意：

```text
https://shop.tiktok.com/alliance/creator/auth?app_key=${TTS_APP_KEY}&state=${STATE}
```

Seller 授权使用 Seller 入口，Creator 授权使用 Creator 入口；两者的授权上下文、
scope 和 Token 必须分开保存。修改 Redirect URL 后必须点击后台的 **Publish changes**。

`Manage Webhook` 配置的是订单、商品等事件通知，和 OAuth `/callback` 是两个不同的
接口，不能混用。

## 凭证安全

App Secret、授权码、Token 和 webhook 签名密钥只允许进入本地运行时配置或密钥管理
系统，不得写入 README、日志、截图或 Git。若凭证曾经出现在共享文件、聊天或截图中，
应立即在后台轮换并更新本地配置。

## 关键约束

- 发布带商品视频要求创作者授权上下文和 `creator.video.write` scope。
- 机构主账号 Token 不能替代每个创作者账号的独立授权。
- 官方文档提示 Affiliate API 受注册地区、目标市场和应用类别限制；英国/欧盟可用性需在对应 API 页面底部检查。
- 外部合作方不能通过 API 自动完成创作者入驻审核。
- 具体版本、错误码、配额和发布频率以官方 API Reference 当前页面为准。
