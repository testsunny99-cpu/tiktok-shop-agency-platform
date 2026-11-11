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

## 关键约束

- 发布带商品视频要求创作者授权上下文和 `creator.video.write` scope。
- 机构主账号 Token 不能替代每个创作者账号的独立授权。
- 官方文档提示 Affiliate API 受注册地区、目标市场和应用类别限制；英国/欧盟可用性需在对应 API 页面底部检查。
- 外部合作方不能通过 API 自动完成创作者入驻审核。
- 具体版本、错误码、配额和发布频率以官方 API Reference 当前页面为准。
