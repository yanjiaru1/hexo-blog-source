---
title: Claude Code + CC Switch 接入国产大模型完整教程
date: 2026-06-16 20:34:03
tags:
---

本教程带你一步步完成：安装 Node.js → 安装 Claude Code → 安装 CC Switch → 配置 DeepSeek / MiMo 模型 → 在 VSCode 中使用。全程无需魔法，小白也能跟着操作。

> **备选：科学上网** — 如有需要，可注册使用：https://47.238.146.96/auth/register?code=ulvjRea7

------

## 一、环境准备

### 安装 Node.js

1. 访问官网下载安装包：https://nodejs.org/

2. 选择 LTS（长期支持）版本，下载并运行安装程序

3. 安装完成后，打开终端验证：

   ```
   node -v
   npm -v
   ```

------

## 二、安装 Claude Code

```
npm install -g @anthropic-ai/claude-code
```

安装完成后验证：

```
claude --version
```

看到版本号即安装成功。

------

## 三、安装 CC Switch

1. 访问 CC Switch 下载页面：https://github.com/farion1231/cc-switch/releases
2. 根据操作系统选择安装包：
   - **Windows**：下载 `CC-Switch-xxx-Windows.msi`，双击安装
   - **macOS**：下载 `.dmg` 文件，拖入 Applications
   - **Linux**：下载 `.AppImage` 或 `.deb`，按对应方式安装
3. 安装完成后启动 CC Switch，确保顶部选中的是 **Claude Code**

------

## 四、配置模型

### 4.1 配置 DeepSeek

#### 1. 获取 DeepSeek API Key

1. 访问 DeepSeek 开放平台：https://platform.deepseek.com/api_keys
2. 注册/登录 → 点击 **创建 API Key** → 命名后创建
3. **立即复制保存 Key**（格式：`sk-xxx...`，关闭后不可再见）

#### 2. 在 CC Switch 中添加 DeepSeek

1. 点击右上角 **“+”** → 预设下拉选择 **DeepSeek**
2. 配置信息如下：

| 字段         | 填写内容                             |
| ------------ | ------------------------------------ |
| **Base URL** | `https://api.deepseek.com/anthropic` |
| **认证类型** | `ANTHROPIC_AUTH_TOKEN`               |
| **API 格式** | `Anthropic Message`                  |
| **API Key**  | 填入上一步复制的 `sk-xxx...`         |

1. 模型映射：

| 模型配置项             | 填写内容            | 说明                                   |
| ---------------------- | ------------------- | -------------------------------------- |
| **主模型（Opus）**     | `deepseek-v4-pro`   | 复杂任务，可加 `[1m]` 开启 100万上下文 |
| **标准模型（Sonnet）** | `deepseek-v4-pro`   | 日常任务                               |
| **快速模型（Haiku）**  | `deepseek-v4-flash` | 简单快速任务                           |

1. 点击 **保存** → 点击 **启用**（状态变绿）→ 点击 **健康检查** 验证

#### 3. 测试

终端输入 `claude`，发送：`你是什么模型？` 若回复含 DeepSeek 字样即配置成功。

------

### 4.2 配置 Mimo 模型

#### 1. 注册小米MiMo开放平台并获取API Key

1. 访问小米MiMo开放平台：[https://platform.xiaomimimo.com?ref=W8ZQGU](https://platform.xiaomimimo.com/?ref=W8ZQGU)
2. 注册账号，首次注册填写邀请码可获 ¥10 体验金
3. 登录后，左侧菜单 → **API Keys** → 点击 **创建 API Key**
4. 命名（如 `claude-code`）→ 确认创建 → **立即复制保存 Key**（格式：`sk-xxx...`，关闭后不可再见）

#### 2. 在CC Switch中添加MiMo

1. 打开 CC Switch，确保顶部选中的是 **Claude Code**
2. 点击右上角 **“+”** 按钮 → 添加供应商
3. 预设下拉选择 **小米 mimo**（或手动填写）

##### 推荐配置（预设自动填充）：

| 字段           | 填写内容                                         |
| -------------- | ------------------------------------------------ |
| **供应商名称** | `Xiaomi MiMo`                                    |
| **请求地址**   | `https://token-plan-cn.xiaomimimo.com/anthropic` |
| **API 格式**   | `Anthropic Message（原生）`                      |
| **认证字段**   | `ANTHROPIC_AUTH_TOKEN`（默认，不要选 API_KEY）   |
| **API Key**    | 填入上一步复制的 `sk-xxx...`                     |

##### 备选手动配置：

| 字段         | 填写内容                           |
| ------------ | ---------------------------------- |
| **Base URL** | `https://api.xiaomi.com/anthropic` |
| **认证类型** | ⚠️ 必须选 `ANTHROPIC_AUTH_TOKEN`    |

#### 3. 配置模型映射

填写完基本配置后，展开模型映射区域，将三个模型分别填入：

| 模型配置项             | 填写内容            | 说明                          |
| ---------------------- | ------------------- | ----------------------------- |
| **Opus（复杂任务）**   | `mimo-v2.5-pro[1m]` | `[1m]` 后缀激活 1M 超长上下文 |
| **Sonnet（日常任务）** | `mimo-v2.5-pro`     | 不带后缀，200K 上下文         |
| **Haiku（快速任务）**  | `mimo-v2-flash`     | 轻量快速模型                  |

> ⚠️ **注意**：模型名必须全部小写！四个模型框填完后点击”一键配置”。

点击 **保存** 完成添加。

#### 4. 启用并验证

1. 在供应商列表中找到刚添加的 MiMo，点击 **启用**（状态变绿）
2. 点击 **健康检查**，确认显示绿色通过
3. 如果失败：检查 API Key 是否正确、网络是否连通

#### 5. 测试

终端输入 `claude` 启动，发送：

```
COPY请告诉我你是什么模型？
```

若回复含 “小米 / MiMo” 字样则表示配置成功。

### 在vscode中安装claude code插件

1. 打开 VSCode，按 `Ctrl+Shift+X` 打开扩展商店
2. 搜索 **Claude Code**（发布者为 Anthropic）
3. 点击 **Install** 安装
4. 安装完成后，编辑器右上角会出现 **Spark 图标**，点击即可使用
5. 如果已通过 CC Switch 配置了第三方模型，VSCode 插件会自动读取 `~/.claude/settings.json` 中的配置，无需额外设置

> 也可以通过命令面板 `Ctrl+Shift+P` → 输入 “Claude Code: Open in New Tab” 来打开。

------

以上就是 Claude Code + CC Switch 接入国产大模型的完整流程。配置完成后在终端输入 `claude` 或在 VSCode 中点击 Spark 图标，即可开始 AI 编程之旅。