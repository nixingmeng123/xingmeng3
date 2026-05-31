# 部署说明

## 方式 A：通过 GitHub 给 Anything 读取

1. 新建一个 GitHub 仓库。
2. 上传本项目全部文件。
3. 在 Anything 中选择从 GitHub 导入/读取该项目。
4. 在 Anything 的 Secrets / Environment Variables 中添加：

```env
API_AUTH_TOKEN=你自己设置的接口访问密码
ANYTHING_PROJECT_TOKEN=你的Anything项目token
NEXT_PUBLIC_CREATE_BASE_URL=https://www.anything.com
ANYTHING_MODEL_ID=anthropic-claude-opus-4-7
```

5. Publish。
6. 用发布后的域名访问网页测试台。

## 方式 B：如果 Anything 支持直接导入项目

直接导入本项目，然后同样配置环境变量，再发布。

## 部署后检查

访问：

```text
https://你的域名/api/chat
```

这是 POST 接口，浏览器直接打开可能显示 404 或 Method Not Allowed，这是正常的。请用 POST 请求测试。
