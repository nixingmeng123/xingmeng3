# Anything Chat API with Token Authentication

一个可以部署到 Anything 的轻量聊天接口项目。它提供网页测试台和 `/api/chat` 后端接口，用于把用户输入转发到 Anything 项目内的 AI 模型集成。

默认模型集成：

```text
anthropic-claude-opus-4-7
```

> 这是一个纯净可分享版本，不包含真实 `.env`、项目 token 或 API key。

## 功能

- 网页测试台：可直接在页面里测试接口
- Token 鉴权：通过 `Authorization: Bearer ...` 保护接口
- 文本聊天：接收 `message`，返回模型回复
- Anything 集成：默认调用 Anything 内部 Claude Opus 4.7 integration
- 环境变量配置：别人部署时填自己的 token 和项目配置

## 当前限制

- 不是 Claude Code 兼容接口
- 不是 OpenAI 兼容接口
- 不能直接给 Cherry Studio 当模型源
- 暂不支持联网搜索
- 暂不支持上传文件
- 暂不支持多轮上下文记忆
- 暂不支持流式输出

## 项目结构

```text
.
├─ apps/
│  ├─ web/
│  │  ├─ src/app/page.tsx              # 网页测试台
│  │  ├─ src/app/api/chat/route.ts     # 核心聊天接口
│  │  └─ .env.example                  # 环境变量示例
│  └─ mobile/
├─ publisher/
├─ 使用说明.md                          # 中文详细说明
├─ README-share.md                      # 分享说明
└─ package.json
```

## 环境变量

复制示例文件：

```text
apps/web/.env.example
```

需要配置：

```env
API_AUTH_TOKEN=change-this-token
ANYTHING_PROJECT_TOKEN=your_anything_project_token_here
NEXT_PUBLIC_CREATE_BASE_URL=https://www.anything.com
ANYTHING_MODEL_ID=anthropic-claude-opus-4-7
```

### API_AUTH_TOKEN

你给 `/api/chat` 设置的访问密码。调用接口时必须带：

```text
Authorization: Bearer API_AUTH_TOKEN的值
```

### ANYTHING_PROJECT_TOKEN

Anything 项目调用内部 AI 集成需要的 token。不要公开分享真实值。

### NEXT_PUBLIC_CREATE_BASE_URL

Anything 的基础地址。通常可以保持：

```text
https://www.anything.com
```

### ANYTHING_MODEL_ID

调用的模型集成 ID。默认是：

```text
anthropic-claude-opus-4-7
```

如果部署账号没有这个模型权限，需要换成该账号可用的 integration ID。

## 部署到 Anything

1. 把这个项目上传到 GitHub。
2. 在 Anything 里选择从 GitHub 项目创建/导入。
3. 按 `apps/web/.env.example` 配置 Secrets / Environment Variables。
4. Publish。
5. 打开发布后的网页，使用测试台验证 `/api/chat`。

> 如果 Anything 托管环境会自动注入 `ANYTHING_PROJECT_TOKEN` 和 `NEXT_PUBLIC_CREATE_BASE_URL`，只需要补充 `API_AUTH_TOKEN` 和可选的 `ANYTHING_MODEL_ID`。

## API 使用

发布后接口地址类似：

```text
https://your-project-domain/api/chat
```

请求：

```http
POST /api/chat
Authorization: Bearer your-token
Content-Type: application/json
```

请求体：

```json
{
  "message": "你好，请介绍一下自己"
}
```

返回：

```json
{
  "reply": "你好，我是一个 AI 聊天助手。"
}
```

## PowerShell 测试

```powershell
Invoke-RestMethod `
  -Method Post `
  -Uri "https://your-project-domain/api/chat" `
  -Headers @{ Authorization = "Bearer your-token" } `
  -ContentType "application/json" `
  -Body '{"message":"你好，请介绍一下自己"}'
```

## curl 测试

```bash
curl -X POST "https://your-project-domain/api/chat" \
  -H "Authorization: Bearer your-token" \
  -H "Content-Type: application/json" \
  -d '{"message":"你好，请介绍一下自己"}'
```

## 常见问题

### 401 Unauthorized

说明没有带 token，或者 token 不等于 `API_AUTH_TOKEN`。

### 500 Failed to get AI response

通常是 Anything integration 配置有问题。检查：

```text
ANYTHING_PROJECT_TOKEN
NEXT_PUBLIC_CREATE_BASE_URL
ANYTHING_MODEL_ID
```

### Cherry Studio 获取不到模型

当前项目只有 `/api/chat`，不是 OpenAI 兼容接口。Cherry Studio 需要：

```text
GET /v1/models
POST /v1/chat/completions
```

### Claude Code 不能用

Claude Code 需要 Anthropic Messages API 兼容接口，并且要支持工具调用。当前项目只是普通文本接口。

## 安全提醒

不要提交或分享真实 `.env` 文件。

不要公开：

```text
ANYTHING_PROJECT_TOKEN
真实 API key
真实后台 token
```

公开给别人使用时，消耗的是部署者自己的 Anything 项目额度。
