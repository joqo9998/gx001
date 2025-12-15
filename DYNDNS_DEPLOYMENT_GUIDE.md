# 使用动态 DNS 域名 (joqo.dpdns.org) 部署指南

## ⚠️ 重要说明

你的域名 `joqo.dpdns.org` 是一个**动态 DNS (DynDNS)** 域名，这是一个子域名服务。

### 关于动态 DNS 域名的限制

**问题：**
- 动态 DNS 域名（如 .dpdns.org）通常**不能直接添加到 Cloudflare**
- 因为它是一个子域名，你不拥有根域名 `dpdns.org`
- 你无法修改 nameservers（DNS 服务器）

**这意味着什么？**
- ❌ 不能使用传统的 Cloudflare Pages + 自定义域名方案
- ✅ 但有其他解决方案！

---

## 🎯 推荐方案：三种部署选择

### 方案 1️⃣：直接使用 Cloudflare Pages 提供的域名（最简单）

**优点：**
- 完全免费
- 自动 HTTPS
- 全球 CDN
- 部署简单

**步骤：**

1. **上传代码到 GitHub**
   ```bash
   cd my-homepage
   git init
   git add .
   git commit -m "Initial commit"
   git remote add origin https://github.com/你的用户名/my-homepage.git
   git branch -M main
   git push -u origin main
   ```

2. **在 Cloudflare Pages 部署**
   - 登录 https://dash.cloudflare.com
   - Workers & Pages → Create → Pages → Connect to Git
   - 选择 GitHub 仓库
   - 配置：
     - Framework: None
     - Build command: 留空
     - Build directory: `/`
   - Deploy

3. **获得免费域名**
   - 你会得到：`your-project.pages.dev`
   - 例如：`joqo-homepage.pages.dev`
   - 完全可用，专业，免费 HTTPS

4. **（可选）使用你的 DynDNS 域名转发**

   在你的 DynDNS 服务商设置 HTTP 重定向：
   - 源：`joqo.dpdns.org`
   - 目标：`joqo-homepage.pages.dev`

   这样访问 `joqo.dpdns.org` 会跳转到你的 Pages 网站

---

### 方案 2️⃣：购买一个真正的域名（推荐）

**为什么推荐？**
- 更专业（如 `yourname.com`）
- 完全控制
- 可以配置 Cloudflare 的所有功能
- 很便宜（.com 约 $10-15/年）

**哪里购买域名：**

1. **Cloudflare Registrar**（推荐）
   - 网址：https://www.cloudflare.com/products/registrar/
   - 价格：成本价，无加价
   - .com 约 $9.77/年
   - 自动集成 Cloudflare DNS

2. **Namesilo**
   - 网址：https://www.namesilo.com
   - 价格便宜
   - 免费 WHOIS 隐私保护
   - .com 约 $8.99/年

3. **腾讯云**（中国用户）
   - 网址：https://dnspod.cloud.tencent.com/
   - .com 首年约 ¥55
   - 支持支付宝/微信

4. **阿里云/万网**（中国用户）
   - 网址：https://wanwang.aliyun.com/
   - .com 首年约 ¥60

**购买后：**
- 按照 `CUSTOM_DOMAIN_GUIDE.md` 的步骤部署
- 完美支持所有 Cloudflare 功能

---

### 方案 3️⃣：使用 Cloudflare Workers（高级）

如果你坚持使用 `joqo.dpdns.org`，可以通过 Cloudflare Workers 反向代理：

**步骤：**

1. **部署网站到 Cloudflare Pages**（方案 1 的步骤 1-3）

2. **创建 Cloudflare Worker**
   - 在 Cloudflare Dashboard → Workers & Pages → Create
   - 创建一个 Worker

3. **添加反向代理代码**
   ```javascript
   addEventListener('fetch', event => {
     event.respondWith(handleRequest(event.request))
   })

   async function handleRequest(request) {
     const url = new URL(request.url)
     url.hostname = 'joqo-homepage.pages.dev'

     const response = await fetch(url, request)
     return response
   }
   ```

4. **配置 DynDNS 的 DNS**
   - 在你的 DynDNS 控制面板
   - 添加 CNAME 记录指向 Worker 的 URL

**注意：** 这个方案比较复杂，不建议初学者使用

---

## 💡 我的建议（按优先级）

### 🥇 最推荐：购买真正的域名
**理由：**
- 一年只要 $10 左右
- 专业形象
- 完全控制
- 支持所有功能

**行动：**
1. 在 Cloudflare/Namesilo 购买域名（如 `joqo.com`）
2. 按照 `CUSTOM_DOMAIN_GUIDE.md` 部署
3. 享受专业的个人网站

---

### 🥈 次推荐：使用 Cloudflare Pages 免费域名
**理由：**
- 完全免费
- 立即可用
- 功能完整

**行动：**
1. 部署到 Cloudflare Pages
2. 使用 `yourproject.pages.dev` 域名
3. 也可以在简历、社交媒体上分享

---

### 🥉 保留 DynDNS：作为跳转
**理由：**
- 保持你现有的域名
- 作为跳转链接使用

**行动：**
1. 主站部署到 Cloudflare Pages
2. DynDNS 设置 301 重定向到 Pages 域名
3. 最好的两全方案

---

## 🚀 快速开始：立即部署到 Cloudflare Pages

**5 分钟部署教程：**

### 步骤 1：准备 GitHub

如果还没创建仓库：
1. 访问 https://github.com/new
2. Repository name: `my-homepage`
3. Public
4. Create repository

### 步骤 2：上传代码

打开命令行，在 `my-homepage` 文件夹中：

```bash
git init
git add .
git commit -m "Initial commit: 个人主页"
git remote add origin https://github.com/你的GitHub用户名/my-homepage.git
git branch -M main
git push -u origin main
```

### 步骤 3：部署到 Cloudflare

1. 打开 https://dash.cloudflare.com
2. 注册/登录（如果还没有账号）
3. 点击 "Workers & Pages"
4. 点击 "Create application"
5. 选择 "Pages" 标签
6. 点击 "Connect to Git"
7. 选择 "Connect GitHub"
8. 授权后选择 `my-homepage` 仓库
9. 项目配置：
   ```
   Project name: joqo-homepage
   Production branch: main
   Framework preset: None
   Build command: (留空)
   Build output directory: /
   ```
10. 点击 "Save and Deploy"

### 步骤 4：完成！

几秒钟后，你会得到：
```
https://joqo-homepage.pages.dev
```

访问这个网址，你的网站就上线了！🎉

---

## 🔗 关于 joqo.dpdns.org 的使用

部署完成后，你可以：

### 选项 A：直接使用 Pages 域名
- 在简历、社交媒体分享：`https://joqo-homepage.pages.dev`
- 专业、免费、带 HTTPS

### 选项 B：设置 DynDNS 重定向
1. 登录你的 DynDNS 服务控制面板
2. 找到 `joqo.dpdns.org` 的设置
3. 设置 HTTP 重定向或 URL 转发：
   - 目标：`https://joqo-homepage.pages.dev`
   - 类型：301 永久重定向
4. 这样别人访问 `joqo.dpdns.org` 会跳转到你的 Pages 网站

### 选项 C：购买新域名（长期方案）
- 选一个好记的域名，如：
  - `joqo.com`
  - `joqo.me`
  - `joqo.dev`
- 一年 $10-15
- 完全属于你

---

## ❓ 常见问题

### Q: DynDNS 域名可以用在 Cloudflare Pages 吗？
**A:** 不能直接用作自定义域名，但可以作为跳转链接。真正的网站建议用 Pages 提供的 `.pages.dev` 域名或购买真实域名。

### Q: .pages.dev 域名看起来不专业吗？
**A:** 其实很多开发者都用这个，GitHub Pages 用的是 `.github.io`，Vercel 用的是 `.vercel.app`，这些都很常见和被接受。

### Q: 一定要买域名吗？
**A:** 不一定。如果只是个人项目、学习、作品集，`.pages.dev` 完全够用。如果是商业用途或个人品牌，建议买个域名。

### Q: 域名很贵吗？
**A:** 不贵！.com 一年只要 $10 左右，相当于一杯咖啡的价格。

---

## 📝 下一步行动

**我建议你现在就：**

1. ✅ **立即部署到 Cloudflare Pages**
   - 按照上面的"快速开始"步骤
   - 5 分钟就能完成
   - 获得 `yourproject.pages.dev` 域名

2. ✅ **先用着，体验一下**
   - 看看网站效果
   - 分享给朋友
   - 测试各种功能

3. 💭 **之后再决定是否买域名**
   - 如果你喜欢这个网站
   - 想长期使用
   - 再考虑买个域名

---

## 🎁 额外福利

### 免费域名选项

如果你是学生，可以申请免费域名：

1. **GitHub Student Pack**
   - 访问 https://education.github.com/pack
   - 包含免费 .me 域名 1 年
   - 还有很多其他开发者工具

2. **Freenom**（免费顶级域名）
   - .tk, .ml, .ga, .cf, .gq 免费
   - 不过不太推荐，SEO 不友好

---

需要我帮你完成部署步骤吗？或者有任何问题随时问我！🚀
