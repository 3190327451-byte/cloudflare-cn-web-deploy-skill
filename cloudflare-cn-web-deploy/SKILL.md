---
name: cloudflare-cn-web-deploy
description: 使用 Cloudflare Pages 免费发布静态网站，并面向中国大陆访问场景检查构建、深层路由、PWA 离线缓存和实际可达性。用于用户希望获得公开 pages.dev 地址、降低对特殊网络环境的依赖或排查大陆访问问题；不得承诺 Cloudflare 免费域名在中国大陆永久稳定直连。
---

# Cloudflare 大陆友好部署

目标是用 Cloudflare Pages 交付一个公开、可刷新深层路径、可继续更新的网站，并针对中国大陆用户说明真实可访问性和离线边界。

## 先判断方案是否合适

- 纯 HTML、Vite、React/Vue SPA 和静态导出项目通常适合 Pages。
- 如果项目依赖服务端渲染、私密环境变量、数据库或受保护 API，不要为了静态发布删除功能；改用 Pages Functions、Workers 或其他托管方案。
- `pages.dev` 免费且常能访问，但不同地区、运营商和时间的表现可能不同。不得把一次测试成功表述为“大陆永久稳定”。
- 如果用户要求面向大陆商业运营、稳定 SLA 或长期收款，明确说明通常需要境内云、域名和 ICP 备案；Cloudflare 免费部署只能作为低成本验证或备用方案。

## 数据与权限边界

- 只有用户明确要求发布后，才创建项目或公开部署。
- 不上传 `.env`、密钥、令牌、数据库文件或真实用户内容。
- 浏览器数据按域名隔离。更换网址后，IndexedDB 和 localStorage 中的旧内容不会自动出现；发布前提醒用户导出，进入新网址后再导入。
- Cache Storage 只保存网页程序和静态资源。文章、便签等用户内容应保存在 IndexedDB 或后端，不能依赖普通网页缓存。

## 构建与路由

1. 阅读项目配置，确认真实构建命令和输出目录，不假设一定是 `dist`。
2. 对单页应用配置深层路由回退。Cloudflare Pages 常用 `_redirects`：`/* /index.html 200`。
3. 如果原项目已有 Worker 或 SSR 构建，建立独立静态构建入口，保留原部署。
4. 运行生产构建和现有测试，确认输出含 `index.html`、JS/CSS；PWA 项目还应包含 manifest、图标和 service worker。

## 离线与更新

- 离线使用通常要求用户至少成功联网打开一次网站，让 service worker 缓存应用外壳。
- service worker 使用版本化缓存并在激活时清理旧版本，避免长期显示旧界面。
- 写作类应用应在新版本就绪时提示“保存并刷新”，不要在输入过程中自动刷新。
- 在 `visibilitychange`、`pagehide` 等离开时机补写尚未落盘的数据，并显示“已保存在本机/离线使用中/上次保存时间”。

## 发布

优先使用项目已有的 Cloudflare 连接或 Wrangler。没有登录时，让用户在浏览器完成授权。

```text
wrangler pages project list
wrangler pages project create <project-name> --production-branch main
wrangler pages deploy <output-directory> --project-name <project-name> --branch main
```

只有在开发者明确使用本机代理且连接失败时，才对当前发布命令临时使用同一代理；不要永久修改系统代理。终端能否访问 Cloudflare 与最终用户能否打开网站是两件事。

## 必须验证

- 固定生产地址返回 200，页面标题与核心资源正确。
- 至少一个真实深层路径刷新后仍返回应用，而不是 404。
- JS、CSS、manifest 和 service worker 正常加载。
- 首次在线加载后，断网仍能打开应用；离线新增内容在恢复联网后仍存在。
- 请用户用目标地区的手机流量和家庭宽带各测试一次。记录测试时间、地区和运营商，不用单一网络结果代替普遍结论。

## 交付说明

交付固定生产网址，不只给一次性预览地址，并说明：

- 首次离线使用前需要成功联网打开一次。
- 新域名不会自动继承旧域名的本地文章，需要导出和导入。
- 后续更新可重新构建并发布；连接 GitHub 自动构建后，推送主分支即可。
- “已经部署”和“在中国大陆长期稳定可访问”必须分开表述。
