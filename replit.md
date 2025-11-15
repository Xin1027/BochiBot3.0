# 波奇Discord机器人 (Bochi Discord Bot)

## Overview

波奇是一个专为图片识别和AI点评设计的Discord机器人。该机器人集成了AI服务，能够自动对用户分享的图片进行反应和智能点评，特别专注于Discord服务器中的图片分析和互动功能。

## 功能特点

- **智能图片识别**: 自动检测频道中的图片并进行处理
- **AI点评系统**: 使用Gemini或OpenAI API对图片进行友好的点评
- **自动表情反应**: 对精美图片自动添加表情反应（支持标准表情和服务器自定义表情）
- **频道级别独立配置**: 每个频道可以独立设置表情反应，优先级为：频道 > 服务器 > 全局
- **服务器表情管理**: 扫描→选择→确认的表情配置流程，支持分页浏览和多页选择
- **权限控制**: 支持全局管理员、服务器所有者和个人授权的多层级权限系统
- **多API支持**: 同时支持Gemini和OpenAI API，支持多密钥轮询
- **模型选择**: 支持动态获取和选择不同的AI模型
- **自定义AI提示词**: 可配置AI点评的风格和内容
- **Discord原生交互**: 使用Discord的按钮、选择菜单、模态框等原生组件进行配置

## 机器人设置指南

### 1. Discord开发者门户设置

1. 访问 https://discord.com/developers/applications
2. 创建新应用或选择现有应用
3. 在"Bot"页面中：
   - **必须启用**: "MESSAGE CONTENT INTENT" (用于检测图片消息)
   - **必须启用**: "SERVER MEMBERS INTENT" (可选，但建议开启)
4. 复制Bot Token并在Replit中设置为DISCORD_TOKEN密钥
5. 在"OAuth2 > URL Generator"中：
   - 选择"bot"和"applications.commands"作用域
   - 选择必要的权限：
     - Send Messages
     - Use Slash Commands
     - Add Reactions
     - Attach Files
     - Read Message History
     - Use External Emojis
     - Manage Messages (可选)
   - 使用生成的URL邀请机器人到服务器

### 💡 重要提示
- 如果没有启用MESSAGE CONTENT INTENT，机器人无法自动检测和反应图片
- 当前机器人使用基础权限运行，配置功能正常，但图片反应功能需要上述权限才能工作

### 2. 机器人权限配置

机器人控制权限通过多层级权限系统进行管理，权限检查优先级如下：

#### 全局管理员（最高优先级）
- **环境变量**: `BOCHI_GLOBAL_ADMIN_ID`
- **功能**: 设置后该用户ID在所有服务器中都具有完整管理权限
- **用途**: 适合机器人维护者跨服务器管理
- **设置方法**: 在Replit Secrets中设置Discord用户ID

#### 服务器权限（次优先级）
- **服务器所有者**: 自动拥有管理权限
- **个人授权**: 通过"/波奇面板"中的"权限设置"单独授权特定用户

#### 权限继承规则
- 默认情况下，如果未设置任何角色，所有用户都可以使用
- 支持多角色权限控制
- 全局管理员权限覆盖所有服务器级别的权限设置

## Recent Changes

**2025-11-15**: 频道级别独立表情反应功能
- 实现了完整的频道级别表情配置系统
- 每个频道可独立配置标准表情和服务器自定义表情
- 表情优先级：频道设置 > 服务器设置 > 全局设置
- 支持完整的分页浏览和多页选择功能
- 添加了频道表情管理界面和交互按钮
- 所有数据流和处理器已正确实现并通过测试

**2025-11-15**: 权限系统优化
- 移除了"BOT维护员"默认管理员角色
- 简化为三层权限系统：全局管理员 > 服务器所有者 > 个人授权用户

**2025-11-15**: 初始配置
- 安装所有Node.js依赖 (discord.js, @google/generative-ai, axios, dotenv)
- 配置DISCORD_TOKEN密钥进行机器人认证
- 设置"Discord Bot"工作流持续运行
- 配置VM模式部署以实现24/7运行
- 创建.gitignore保护敏感文件
- 机器人成功连接并在服务器中注册斜杠命令

## User Preferences

Preferred communication style: Simple, everyday language.

## System Architecture

### Core Application Structure
- **Single-file architecture**: The main application logic is contained in `index.js`, following a class-based design pattern with the `BochiBot` class as the central controller
- **Event-driven architecture**: Built on Discord.js event system to handle user interactions, message events, and bot commands
- **Configuration-driven design**: Centralized configuration object managing bot settings, API configurations, and user permissions

### Bot Framework
- **Discord.js v14**: Primary framework for Discord API integration
- **Gateway Intents**: Configured with minimal required permissions (Guilds intent)
- **Slash Commands**: Uses Discord's modern slash command system for user interactions
- **Interactive Components**: Implements buttons, select menus, and modals for rich user experiences

### AI Integration Layer
- **Multi-provider support**: Dual AI provider architecture supporting both Google Gemini and OpenAI
- **Fallback mechanism**: Primary Gemini integration with OpenAI as alternative option
- **API key rotation**: Built-in support for multiple Gemini API keys with automatic rotation
- **Model flexibility**: Configurable model selection (gemini-1.5-flash, gpt-4-vision-preview)

### Permission System
- **Role-based access control**: Configurable allowed roles for bot administration
- **Permission validation**: Built-in permission checking for sensitive operations
- **Security-first approach**: Ephemeral responses for unauthorized access attempts

### Configuration Management
- **Runtime configuration**: Dynamic settings that can be modified through bot interface
- **Persistent state**: Configuration maintained in memory during bot runtime
- **Hierarchical settings**: Separate configuration domains for bot behavior and API settings

## External Dependencies

### AI Services
- **Google Generative AI**: Primary AI provider using `@google/generative-ai` package for image analysis and text generation
- **OpenAI API**: Secondary AI provider accessed via HTTP requests using axios
- **Model Support**: Gemini 1.5 Flash and GPT-4 Vision Preview for multimodal capabilities

### Discord Platform
- **Discord.js**: Complete Discord API wrapper providing bot functionality, event handling, and rich message components
- **Discord API**: Real-time gateway connection for live message processing and user interactions

### HTTP Client
- **Axios**: HTTP client library for external API communications, particularly for OpenAI integration and potential webhook support

### Environment Management
- **dotenv**: Environment variable management for secure API key storage and configuration

### Runtime Dependencies
- **Node.js**: JavaScript runtime environment
- **NPM ecosystem**: Standard package management and dependency resolution