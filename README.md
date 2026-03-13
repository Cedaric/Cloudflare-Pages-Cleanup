# Cloudflare-Pages-Cleanup

> [!NOTE]
> 根据 Cloudflare 官方文档的 [已知问题](https://developers.cloudflare.com/pages/platform/known-issues/#delete-a-project-with-a-high-amount-of-deployments)：如果一个 Pages 项目的部署数量过多（超过 100 个），可能无法直接删除该项目。建议先删除所有部署记录，然后再删除项目。本项目即是为此场景设计的自动化清理工具。

这个项目用于自动清理 Cloudflare Pages 的历史部署记录，支持多个项目并行处理，并通过 GitHub Actions 定时执行。

## 功能特点
- **多项目支持**：通过 GitHub Variables 一次性配置多个项目。
- **定时执行**：默认每月 1 号执行一次，也支持手动触发。
- **保护生产环境**：自动识别并跳过当前的生产环境部署。
- **可选清理别名**：支持选择是否删除带有别名（分支预览）的部署。

## 使用方法

### 1. 配置 GitHub Actions
在 GitHub 仓库的 **Settings > Secrets and variables > Actions** 中配置以下内容：

#### Secrets (安全凭据)
- `CF_API_TOKEN`: 你的 Cloudflare API 令牌（需具备 Pages 管理权限）。
- `CF_ACCOUNT_ID`: 你的 Cloudflare 账户 ID。

#### Variables (变量配置)
- `CF_PAGES_PROJECT_NAMES`: **JSON 数组格式**的项目名称列表。
  - 示例：`["my-app", "blog-pages", "test-project"]`
- `CF_DELETE_ALIASED_DEPLOYMENTS`: 设置为 `true` 以删除包含别名的部署（如分支预览），设置为 `false` 则保留。

### 2. 手动执行
1. 进入 GitHub 仓库的 **Actions** 选项卡。
2. 选择左侧的 **Cleanup Cloudflare Deployments**。
3. 点击右侧的 **Run workflow**。

### 3. 本地运行 (可选)
如果你想在本地运行脚本：
```bash
# 安装 pnpm (如果尚未安装)
npm install -g pnpm

# 安装依赖并运行
pnpm install
CF_API_TOKEN=xxx CF_ACCOUNT_ID=xxx CF_PAGES_PROJECT_NAME=xxx pnpm start
```

## 维护
脚本文件名为 `index.js`，工作流定义在 `.github/workflows/cleanup.yml`。
