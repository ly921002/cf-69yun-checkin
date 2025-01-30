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
2. 清空默认代码，粘贴[完整代码](https://github.com/your-repo/airport-checkin-worker/blob/main/index.js)
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

![环境变量设置截图](https://example.com/cf-vars-screenshot.png)

### 第四步：配置定时任务
1. 进入「Triggers」→ 「Cron Triggers」
2. 添加新触发器：
   - Cron表达式：`0 9 * * *` (UTC时间每天9点/北京时间17点)
   - 选择部署环境：Production

### 第五步：获取访问地址
部署完成后，在「Workers」→ 「Overview」获取您的专属域名：
