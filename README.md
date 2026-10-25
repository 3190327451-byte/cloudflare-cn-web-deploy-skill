# Cloudflare 大陆友好部署 Skill

一个面向 Codex 的中文 Skill，用 Cloudflare Pages 免费发布静态网站，并检查中国大陆访问场景中最容易被忽略的问题：深层路由、首次联网、PWA 离线缓存、浏览器本地数据隔离和实际网络可达性。

## 适合什么情况

- 想为前端项目获得公开的 `pages.dev` 地址。
- 希望用户首次打开后可以离线继续使用。
- 希望尽量降低大陆用户访问门槛，同时如实说明免费 Cloudflare 的稳定性边界。
- 需要从旧域名迁移，并保护 IndexedDB 或 localStorage 中的数据。

它不会承诺 `pages.dev` 在中国大陆所有地区、运营商和时间都稳定。若项目需要长期商业运营和稳定 SLA，Skill 会建议评估境内云、域名与 ICP 备案。

## 安装

把 [`cloudflare-cn-web-deploy`](./cloudflare-cn-web-deploy) 文件夹安装到 Codex Skills 目录，或者直接让 Codex 安装这个 GitHub 仓库中的同名子目录。

可以这样对 Codex 说：

```text
请安装并使用这个 Skill：
https://github.com/3190327451-byte/cloudflare-cn-web-deploy-skill/tree/main/cloudflare-cn-web-deploy
```

## 使用

```text
请使用 $cloudflare-cn-web-deploy，把这个静态网站免费部署到 Cloudflare Pages，保留已有部署，并检查大陆访问、深层路由、离线缓存和换域名的数据迁移风险。
```

## 许可

MIT License。可以自由使用、修改和分享，但请保留许可证文本。
