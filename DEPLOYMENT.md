# Lianmin MindPocket

## 已停用（2026-09-17）

lm 明确不再需要本服务。Cloudflare workers.dev 正式入口、版本预览入口和 R2 r2.dev 公共访问均已关闭；下文部署验收仅为历史记录，不代表服务仍开放。源码、账号、D1、R2、Vectorize 和 Secret 保留，未删除数据。保留存储仍可能占用免费额度。

配置显式设置 `workers_dev=false`、`preview_urls=false`，避免再次部署时意外恢复公网入口。只有 lm 明确要求恢复时才重新启用；恢复附件访问还需单独启用 R2 公共访问。

- 云端入口：https://lianmin-mindpocket.uptimeworker.workers.dev/login
- Worker、D1、Vectorize、R2 均命名为 `lianmin-mindpocket`；Vectorize 为 1024 维 cosine，含 userId 元数据索引。
- 已创建本人账号；登录邮箱和随机密码在根目录 `.env.local` JSON 文件中，已被 Git 忽略。该文件用于本地保管，不是 Next.js 环境变量配置。
- Worker Secret：`BETTER_AUTH_SECRET`、`OWNER_EMAIL`；JSON 备份在 `apps/api/.dev.vars`，已被 Git 忽略。
- 首次注册仅接受 OWNER_EMAIL，已有用户后沿用上游关闭注册逻辑。重置密码邮件未配置，请保管本地密码。

## 能力边界

已启用登录、书签和内容保存；未配置收费或免费的外部模型，AI 对话及语义向量生成功能暂不可用。Vectorize 绑定存在不等于已有可用向量数据。

此上游使用公开 R2 URL 展示上传附件；现已关闭 `lianmin-mindpocket` 的 r2.dev 访问。若日后恢复公共附件访问，附件链接不是账号鉴权链接。Davflare 与 Artifacts 的桶仍为私有。

## 更新

当前维护路径：`C:\Users\余廉民\cloudflare-builds\mindpocket`。D 盘副本安装失败；服务运行不依赖电脑在线。

```powershell
npx --yes pnpm@10.9.0 --filter web... --filter api... install --frozen-lockfile
$env:NEXT_PUBLIC_APP_URL='https://lianmin-mindpocket.uptimeworker.workers.dev'
npx --yes pnpm@10.9.0 --filter web build
npx wrangler@4.130.0 d1 migrations apply DB --remote --config apps/api/wrangler.jsonc
npx wrangler@4.130.0 deploy --config apps/api/wrangler.jsonc
```

提交推送个人 fork；GitHub push 不自动部署。不要把任何本机凭据提交到 GitHub。

## 验证和成本

2026-09-17：前端构建和 API TypeScript 检查通过；改动文件 Biome 检查通过。线上登录页和健康检查 200、其他邮箱注册 403、本人注册和登录 200。书签导入从 pending 到 completed，内容读回一致，测试书签已删除。未执行浏览器界面验收或 AI 功能验证。

没有升级 Workers 付费套餐或接入付费模型。R2 有免费额度但可以超额计费，额度由账号内服务共享。
