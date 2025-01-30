# 69云自动签到 Cloudflare Worker

## 🚀 核心功能

- **零本地环境** - 完全基于网页端操作
- **自动签到** - 每日定时执行机场账户签到
- **双重触发** - 支持手动URL触发和定时任务
- **失败重试** - 智能重试机制(最多3次)
- **隐私保护** - Telegram通知自动掩码敏感信息
- **即时通知** - 支持Telegram机器人推送结果

## 🌐 网页端部署指南

### 第一步：创建Worker
1. 登录 [Cloudflare Dashboard](https://dash.cloudflare.com)
2. 进入「Workers & Pages」→ 「Create Application」
3. 选择「Create Worker」新建服务

### 第二步：配置代码
1. 在「Quick Edit」编辑器界面
2. 清空默认代码，粘贴[完整代码](https://github.com/ly921002/cf-69yun-checkin/blob/main/worker.js)
3. 点击右上角「Save and Deploy」

### 第三步：设置环境变量
1. 进入「Settings」→ 「Variables」
2. 在「Environment Variables」添加以下变量：

| 变量名        | 必填 | 示例值                  |
|---------------|------|-------------------------|
| DOMAIN        | ✅  | your_airport.com       |
| USERNAME      | ✅  | your_email@domain.com  |
| PASSWORD      | ✅  | your_password          |
| TRIGGER_PATH  | ❌  | /secret-checkin        |
| TG_BOT_TOKEN  | ❌  | 123456:ABC-DEF1234gh   |
| TG_CHAT_ID    | ❌  | -100123456789          |
| MAX_RETRY     | ❌  | 3                       |

### 第四步：配置定时任务
1. 进入「Triggers」→ 「Cron Triggers」
2. 添加新触发器：
   - Cron表达式：`0 0 * * *` (UTC时间每天0点/北京时间8点)
   - 选择部署环境：Production

### 第五步：获取访问地址
部署完成后，在「Workers」→ 「Overview」获取您的专属域名：
https://your-worker.your-subdomain.workers.dev

## 🛠 使用说明

### 手动触发签到
访问（请替换实际路径）：https://[你的Worker域名][TRIGGER_PATH]
示例：https://checkin.example.workers.dev/secret-checkin

### 根路径提示
访问Worker根域名将显示：
请访问 /secret-checkin 触发签到


## 🔔 Telegram通知配置

1. 创建Bot：
   - 联系 @BotFather 创建新机器人
   - 获取并保存 `TG_BOT_TOKEN`

2. 获取Chat ID：
   - 向机器人发送任意消息
   - 访问 `https://api.telegram.org/bot<TG_BOT_TOKEN>/getUpdates`
   - 查找 `chat.id` 字段值

3. 隐私设置：
   - 联系 @BotFather 执行 `/setprivacy`
   - 选择你的机器人 → 设置为 `DISABLE`

## ⚠️ 重要安全设置

1. **自定义触发路径** 
   - 必须修改默认 `TRIGGER_PATH` 值
   - 建议使用包含字母+数字的组合路径

2. **变量保护**
   - 所有敏感变量必须设置在「Environment Variables」
   - 切勿将凭证写入代码

3. **访问限制**
   - 可绑定自定义域名后配置防火墙规则
   - 推荐开启「Workers」→ 「Triggers」→ 速率限制

## 📌 常见问题

Q: 如何测试配置是否正确？
A: 访问 `https://your-worker-domain/你的触发路径` 返回签到结果

Q: 为什么收不到Telegram通知？
A: 检查：
1. Bot Token 和 Chat ID 是否正确
2. 机器人隐私模式已关闭
3. 已向机器人发送过消息

Q: 如何修改执行时间？
A: 在Cron Triggers中调整表达式：
- `0 9 * * *` → 每天UTC 09:00 (北京时间17:00)
- `0 0 * * *` → 每天UTC午夜 (北京时间08:00)

Q: 最多可以重试几次？
A: 通过 `MAX_RETRY` 变量控制，建议不超过5次

## 📞 技术支持
遇到问题请提交Issue并提供：
1. Worker运行日志（「Overview」→ 「Logs」）
2. 错误截图
3. 配置信息（敏感信息打码）
