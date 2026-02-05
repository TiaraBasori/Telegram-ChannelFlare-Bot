# Telegram-ChannelFlare-Bot

仅使用 Cloudflare 免费服务实现的 Telegram 频道消息全能优化机器人。

<p align="center">
  <img src="https://img.shields.io/badge/Cloudflare-Workers-F38020?logo=cloudflare&logoColor=white" alt="Cloudflare Workers">
  <img src="https://img.shields.io/badge/Telegram-Bot_API-26A5E4?logo=telegram&logoColor=white" alt="Telegram Bot API">
  <img src="https://img.shields.io/badge/License-MIT-green.svg" alt="MIT License">
  <img src="https://img.shields.io/badge/Version-2.0.01-blue.svg" alt="Version 2.0.01">
</p>

<p align="center">
  <em>🤖 为 Telegram 频道提供智能消息美化、AI 改写、按钮添加等全方位优化功能</em>
</p>

## ✨ 核心功能

### 🚀 智能优化
- **AI 文案改写**: 支持 OpenAI、Google Gemini 等多种 AI 模型，自动优化消息文案
- **关键词提取**: 自动从消息中提取相关关键词作为标签
- **自定义提示词**: 可配置 AI 改写的要求和风格

### 🎨 消息美化
- **页脚签名**: 在消息末尾添加自定义签名，支持富文本格式
- **底部按钮**: 添加交互式按钮（链接、评论按钮等）
- **转发优化**: 智能显示转发来源，支持多种显示位置
- **链接预览控制**: 灵活控制链接预览的显示与隐藏

### 🛡️ 内容管理
- **屏蔽词库**: 自动过滤包含屏蔽词的消息
- **字数限制**: 设置最小字数要求，过滤过短消息
- **系统消息清理**: 自动删除频道系统消息
- **指令清理**: 发送后自动清理指令词

### ⚙️ 高级功能
- **严格模式**: 需要管理员确认后才发送消息
- **管理群组绑定**: 将频道绑定到管理群组，方便团队协作
- **配置导入/导出**: 一键备份和恢复频道配置
- **媒体组支持**: 完美处理多图/多视频消息组

## 🚀 快速开始

### 1. 准备工作
1. **创建 Telegram Bot**: 通过 [@BotFather](https://t.me/BotFather) 创建机器人，获取 `BOT_TOKEN`
2. **注册 Cloudflare 账号**: 访问 [Cloudflare Workers](https://workers.cloudflare.com/) 注册账号

### 2. 一键部署

#### 方法一：通过 Cloudflare Dashboard 部署
1. 登录 [Cloudflare Workers](https://dash.cloudflare.com/)
2. 点击 "Create a Service"
3. 选择 "HTTP handler"
4. 将 `_worker.js` 代码复制到编辑器中
5. 点击 "Save and Deploy"

#### 方法二：使用 Wrangler CLI 部署
```bash
# 安装 Wrangler CLI
npm install -g wrangler

# 登录 Cloudflare
wrangler login

# 部署 Worker
wrangler deploy
```

### 3. 环境变量配置
在 Workers 设置中添加以下环境变量：

| 变量名 | 说明 | 示例 |
|--------|------|------|
| `BOT_TOKEN` | Telegram Bot Token | `1234567890:ABCdefGHIjklMNOpqrsTUVwxyz` |
| `WHITELIST` | 白名单 ID (可选) | `-10012345678,123456789` |
| `BLACKLIST` | 黑名单 ID (可选) | `-10098765432` |

### 4. 数据库初始化
访问以下 URL 初始化数据库和设置 Webhook：
```
https://你的worker域名.workers.dev/set
```


### 技术架构
- **Cloudflare Workers**: 无服务器运行环境
- **Cloudflare D1**: 关系型数据库（存储频道配置）
- **Cloudflare KV**: 键值存储（缓存和临时数据）
- **Telegram Bot API**: 与 Telegram 平台通信

### 核心模块
1. **配置管理器**: 管理频道配置，支持导入/导出
2. **消息处理器**: 处理消息美化、AI 改写等核心逻辑
3. **AI 处理器**: 对接 OpenAI/Gemini 等 AI 服务
4. **面板处理器**: 提供可视化配置界面
5. **严格模式管理器**: 处理需要确认的消息

### 开始使用
1. 将机器人添加为频道管理员
2. 私聊机器人发送 `/set` 进入配置面板
3. 配置所需功能
4. 在频道发送消息，机器人会自动处理

## 📖 详细配置

### 基础配置
通过私聊机器人发送 `/set` 进入可视化配置面板，支持以下功能配置：

- **AI 设置**: 配置 OpenAI/Gemini API，设置改写要求和关键词数量
- **页脚签名**: 设置自定义页脚内容和格式
- **按钮布局**: 配置底部按钮（支持普通链接和评论按钮）
- **转发优化**: 设置转发来源显示方式和位置
- **屏蔽词库**: 管理屏蔽词列表（支持正则表达式）
- **链接预览**: 控制链接预览的显示与隐藏
- **字数限制**: 设置消息最小字数要求
- **高级设置**: 配置严格模式、管理群组等

### 运行时指令
在消息末尾添加指令，可临时覆盖配置：

| 指令 | 说明 | 示例 |
|------|------|------|
| `ai on` / `ai off` | 启用/禁用 AI 改写 | `消息内容 ai on` |
| `keyword on` / `keyword off` | 启用/禁用关键词提取 | `消息内容 keyword on` |
| `footer on` / `footer off` | 启用/禁用页脚 | `消息内容 footer on` |
| `button on` / `button off` | 启用/禁用按钮 | `消息内容 button on` |
| `forward on` / `forward off` | 启用/禁用转发优化 | `消息内容 forward on` |
| `preview on` / `preview off` | 启用/禁用链接预览 | `消息内容 preview off` |
| `off` | 完全禁用处理 | `消息内容 off` |

### 手动处理命令
回复消息并发送 `/make` 命令可手动处理特定消息：
```
/make ai on footer off
```

## 🔧 开发指南

### 本地开发
```bash
# 克隆项目
git clone https://github.com/TiaraBasori/Telegram-ChannelFlare-Bot.git
cd Telegram-ChannelFlare-Bot

# 安装依赖（如需）
npm install

# 本地开发
wrangler dev
```

### 环境变量
创建 `.dev.vars` 文件用于本地开发：
```
BOT_TOKEN=你的Telegram_Bot_Token
```

### 数据库迁移
项目使用 Cloudflare D1 数据库，表结构会在首次访问 `/set` 时自动创建。

## 🤝 贡献指南

欢迎提交 Issue 和 Pull Request！

1. Fork 本仓库
2. 创建功能分支 (`git checkout -b feature/AmazingFeature`)
3. 提交更改 (`git commit -m 'Add some AmazingFeature'`)
4. 推送到分支 (`git push origin feature/AmazingFeature`)
5. 开启 Pull Request

## 📄 许可证

本项目基于 MIT 许可证开源 - 查看 [LICENSE](LICENSE) 文件了解详情。

## 🙏 致谢

- 感谢 [Moist的私聊机器人](https://github.com/moistrr/TGbot-D1) 为项目的许多部分提供灵感
- 感谢 [莫菲丝](https://t.me/mofeisi233) 赠予AI API
- 感谢朋友们的支持

## 📞 支持与反馈

- 提交 Issue: [GitHub Issues](https://github.com/TiaraBasori/Telegram-ChannelFlare-Bot/issues)
- 功能请求: 通过 Issue 提出
- 问题反馈: 详细描述问题场景和复现步骤

---

<p align="center">
  Made with ❤️ for Telegram channel administrators
</p>

<p align="center">
  <sub>如果这个项目对你有帮助，请给个 ⭐ Star 支持一下！</sub>
</p>