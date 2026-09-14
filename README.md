# 阿姆斯特丹 · 雅典 · Milos 旅行手册

`public/index.html` 是完整网页：样式、地图、数据、倒计时均内嵌，不依赖外部图片、字体或脚本。网页可下载为单个 HTML 离线保存。Google 地图导航、订票和实时天气链接需联网。

## Cloudflare Workers + GitHub 自动发布

仓库包含：

- `public/index.html`：唯一网页文件。
- `wrangler.jsonc`：Cloudflare Workers Static Assets 部署配置。
- `.gitignore`：防止本地凭证和缓存误提交。

Cloudflare Workers 中导入此 GitHub 仓库：

| 设置 | 值 |
|---|---|
| Worker 名称 | `greece-trip-handbook`（必须与 wrangler.jsonc 一致） |
| 生产分支 | `main` |
| 根目录 | 仓库根目录 `/` |
| Build command | 留空（不需要构建） |
| Deploy command | `npx wrangler@4 deploy` |

使用 Cloudflare Free 计划。此站只提供静态资源，不运行应用 Worker 代码、不用数据库或付费 API。当前 Cloudflare 文档说明静态资源请求免费且不限量；自动构建受账号免费构建额度限制。请保留免费计划，不主动开启付费项目。

首次在 Cloudflare 中授权 GitHub 连接，选择本仓库；之后 push 到 `main` 会触发自动发布。不要把默认命令改为 `wrangler versions upload`，后者只上传版本，不保证成为线上页面。

部署成功后使用 Cloudflare 返回的实际 `workers.dev` 地址。访问者不需要登录；站点不应添加 Cloudflare Access 登录限制。

## 后续更新

直接修改 `public/index.html`，commit/push 到 `main`，等待 Cloudflare 构建成功。同一个公开地址会展示新的文件。无需重新连接账号。

不要把机票／酒店原始截图、确认号、证件号、房间号、私人住宅地址、密码或 API Token 提交到仓库。页面待办使用纯文本列表，没有勾选状态或 localStorage，多人读取同一份线上内容。

## 离线说明

打开网页后点击“保存离线手册”，或直接保存 `public/index.html`。这是本地文件离线，不是保证网站 URL 在首次断网访问时能打开；本项目按单文件要求没有额外 Service Worker。手机文件预览器可能不执行 JavaScript，此时正文仍可读，倒计时需支持脚本的浏览器。

## 文档

- [Cloudflare Workers Git 集成](https://developers.cloudflare.com/workers/ci-cd/builds/git-integration/)
- [创建与连接 Workers Builds](https://developers.cloudflare.com/workers/ci-cd/builds/)
- [静态资源费用与限制](https://developers.cloudflare.com/workers/static-assets/billing-and-limitations/)
- [Google Maps 导航链接限制](https://developers.google.com/maps/documentation/urls/get-started)
