# 故障排查

当前阶段先按接口工作流记录故障：授权失败、Token 过期、签名错误、上传失败、预检失败、商品不可用、配额/风控限制、Webhook 丢失和重复任务。

每条故障至少记录：`organization_id`、`account_id`、任务 ID、接口路径、TikTok request ID、错误码、时间和下一步；不要记录完整 Token、App Secret 或授权码。
