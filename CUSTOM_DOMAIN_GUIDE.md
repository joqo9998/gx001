# 使用自定义域名部署到 Cloudflare Pages 完整指南

## 方案概述

如果你已经有域名，有两种方式在 Cloudflare 使用：

### 方案一：域名在其他注册商（推荐）
将域名的 DNS 解析转到 Cloudflare，保持在原注册商

### 方案二：域名也在 Cloudflare
如果域名是在 Cloudflare 注册的，更简单

---

## 🚀 完整部署流程

### 第一步：将域名添加到 Cloudflare（如果还没有）

#### 1.1 添加网站到 Cloudflare

1. 登录 Cloudflare Dashboard：https://dash.cloudflare.com
2. 点击右上角 "Add a Site"（添加站点）
3. 输入你的域名（例如：`yourname.com`）
4. 点击 "Add site"
5. 选择 **Free** 计划（免费）
6. 点击 "Continue"

#### 1.2 更改域名 DNS 服务器（重要！）

Cloudflare 会显示两个 Nameserver（名称服务器），类似：
```
ns1.cloudflare.com
ns2.cloudflare.com
```

你需要去你的域名注册商网站修改 DNS 服务器：

**常见域名注册商修改方法：**

- **阿里云/万网**：
  1. 登录 https://dc.console.aliyun.com
  2. 找到你的域名 → 管理 → DNS 修改
  3. 选择"修改 DNS 服务器"
  4. 填入 Cloudflare 的两个 nameserver

- **腾讯云 DNSPod**：
  1. 登录 https://console.dnspod.cn
  2. 我的域名 → 修改 DNS 服务器
  3. 填入 Cloudflare 的 nameserver

- **GoDaddy**：
  1. 登录 https://account.godaddy.com
  2. My Products → Domains → Manage DNS
  3. Change Nameservers → Custom
  4. 填入 Cloudflare 的 nameserver

- **Namecheap**：
  1. Domain List → Manage
  2. Nameservers → Custom DNS
  3. 填入 Cloudflare 的 nameserver

- **Google Domains**：
  1. My Domains → DNS
  2. Custom name servers
  3. 填入 Cloudflare 的 nameserver

#### 1.3 等待 DNS 生效

- 修改后点击 Cloudflare 页面的 "Done, check nameservers"
- DNS 生效时间：通常 5 分钟到 24 小时
- Cloudflare 会发邮件通知你域名激活成功

---

### 第二步：部署网站到 Cloudflare Pages

#### 2.1 上传代码到 GitHub

在 `my-homepage` 文件夹中打开终端，执行：

```bash
# 初始化 Git 仓库
git init

# 添加所有文件
git add .

# 创建提交
git commit -m "Initial commit: 个人主页"

# 连接到 GitHub（替换 YOUR_USERNAME 为你的 GitHub 用户名）
git remote add origin https://github.com/YOUR_USERNAME/my-homepage.git

# 推送代码
git branch -M main
git push -u origin main
```

如果还没创建 GitHub 仓库：
1. 访问 https://github.com/new
2. Repository name: `my-homepage`
3. 选择 Public
4. 点击 "Create repository"

#### 2.2 在 Cloudflare Pages 创建项目

1. 在 Cloudflare Dashboard，点击左侧 "Workers & Pages"
2. 点击 "Create application"
3. 选择 "Pages" 标签
4. 点击 "Connect to Git"

5. **连接 GitHub**：
   - 点击 "Connect GitHub"
   - 授权 Cloudflare
   - 选择 `my-homepage` 仓库

6. **配置项目**：
   - Project name: `my-homepage`（或任何名字）
   - Production branch: `main`
   - Framework preset: **None**
   - Build command: 留空
   - Build output directory: `/`

7. 点击 "Save and Deploy"

8. 等待部署完成（通常几秒钟）

---

### 第三步：绑定自定义域名

#### 3.1 在 Pages 项目中添加域名

1. 部署完成后，在项目页面点击 "Custom domains" 标签
2. 点击 "Set up a custom domain"

#### 3.2 选择域名类型

你可以绑定：
- **根域名**：`yourname.com`（推荐）
- **子域名**：`www.yourname.com`
- **两者都绑定**（最佳实践）

#### 3.3 添加根域名（yourname.com）

1. 输入：`yourname.com`
2. 点击 "Continue"
3. Cloudflare 会自动配置 DNS 记录
4. 点击 "Activate domain"

**自动添加的 DNS 记录：**
- Type: `CNAME`
- Name: `yourname.com`
- Target: `my-homepage.pages.dev`
- Proxy: 已启用（橙色云朵）

#### 3.4 添加 www 子域名（可选但推荐）

1. 再次点击 "Set up a custom domain"
2. 输入：`www.yourname.com`
3. 点击 "Continue"
4. 点击 "Activate domain"

#### 3.5 设置自动重定向

让 `www.yourname.com` 自动跳转到 `yourname.com`（或反过来）：

1. 在 Cloudflare Dashboard，点击你的域名
2. 进入 "Rules" → "Page Rules" 或 "Redirect Rules"
3. 创建新规则：
   - If: `www.yourname.com/*`
   - Then: Redirect to `https://yourname.com/$1`
   - Status: 301 Permanent Redirect

---

### 第四步：配置 DNS 记录（如果自动配置失败）

如果自动配置没有成功，手动添加 DNS 记录：

1. 在 Cloudflare Dashboard，点击你的域名
2. 进入 "DNS" → "Records"
3. 点击 "Add record"

**为根域名添加记录：**
- Type: `CNAME`
- Name: `@`（表示根域名）
- Target: `my-homepage.pages.dev`
- Proxy status: Proxied（橙色云朵）
- TTL: Auto

**为 www 子域名添加记录：**
- Type: `CNAME`
- Name: `www`
- Target: `my-homepage.pages.dev`
- Proxy status: Proxied（橙色云朵）
- TTL: Auto

点击 "Save"

---

### 第五步：验证部署

#### 5.1 等待 DNS 生效

- 通常几分钟内生效
- 最长可能需要 24-48 小时

#### 5.2 测试网站

在浏览器访问：
- `https://yourname.com`
- `https://www.yourname.com`

#### 5.3 检查 HTTPS（SSL）

- Cloudflare 自动提供免费 SSL 证书
- 如果显示"不安全"，等待几分钟让证书生成
- 通常 10-15 分钟内 HTTPS 就会正常工作

#### 5.4 强制 HTTPS

确保所有访问都使用 HTTPS：

1. 在 Cloudflare Dashboard，点击你的域名
2. 进入 "SSL/TLS" → "Edge Certificates"
3. 开启 "Always Use HTTPS"

---

## 🎯 完整检查清单

- [ ] 域名添加到 Cloudflare
- [ ] DNS Nameservers 已在注册商处修改
- [ ] Cloudflare 邮件确认域名激活
- [ ] 代码已推送到 GitHub
- [ ] Cloudflare Pages 项目创建成功
- [ ] 自定义域名已添加
- [ ] DNS 记录已配置（CNAME）
- [ ] HTTPS 证书已生成
- [ ] 网站可以通过自定义域名访问

---

## 🔧 常见问题

### Q1: DNS 一直不生效怎么办？

**检查方法：**
```bash
# 在命令行检查 DNS
nslookup yourname.com

# 或使用在线工具
https://www.whatsmydns.net
```

**解决方法：**
- 确认在域名注册商处正确修改了 nameservers
- 清除浏览器缓存和 DNS 缓存
- Windows: `ipconfig /flushdns`
- Mac: `sudo dscacheutil -flushcache`
- 等待更长时间（最长 48 小时）

### Q2: 显示"DNS_PROBE_FINISHED_NXDOMAIN"错误

**原因：** DNS 记录配置错误

**解决：**
- 检查 DNS Records 中是否正确添加了 CNAME 记录
- 确保 Name 是 `@` 或域名本身
- Target 应该是 `yourproject.pages.dev`

### Q3: 网站显示 Cloudflare 错误页面

**常见错误代码：**
- **522**: Pages 项目未正确部署
- **525**: SSL 配置问题

**解决：**
- 检查 Pages 项目是否部署成功
- 在 SSL/TLS 设置中，选择 "Flexible" 或 "Full"

### Q4: HTTPS 不工作，显示证书错误

**解决：**
- 等待 10-15 分钟让证书生成
- 在 SSL/TLS → Edge Certificates 中检查证书状态
- 确保 SSL/TLS 加密模式设置为 "Flexible" 或 "Full"

### Q5: www 和非 www 版本都要工作吗？

**最佳实践：**
- 两个都添加到 Pages
- 选择一个作为主域名
- 设置另一个 301 重定向到主域名
- 这样对 SEO 更友好

### Q6: 可以用子域名吗（如 blog.yourname.com）？

**可以！**
1. 在 Pages 项目中添加 `blog.yourname.com`
2. Cloudflare 会自动创建 DNS 记录
3. 或手动添加 CNAME：`blog` → `yourproject.pages.dev`

---

## 🚀 部署后的更新流程

每次修改网站内容：

```bash
# 1. 修改文件（index.html、style.css 等）

# 2. 提交更改
git add .
git commit -m "更新个人信息"

# 3. 推送到 GitHub
git push

# 4. Cloudflare Pages 自动检测并重新部署（几秒钟）
# 5. 网站自动更新！
```

---

## 📊 额外配置（可选）

### 启用 Web Analytics

1. 在 Cloudflare Dashboard → "Web Analytics"
2. 点击 "Add a site"
3. 输入你的域名
4. 复制提供的 JavaScript 代码
5. 粘贴到 `index.html` 的 `</head>` 前

### 配置缓存规则

1. 进入 "Caching" → "Configuration"
2. 设置 Browser Cache TTL（浏览器缓存时间）
3. 建议：4 hours 或 8 hours

### 启用 Brotli 压缩

1. 进入 "Speed" → "Optimization"
2. 开启 "Brotli" 压缩
3. 可以减少 20-30% 的传输大小

### 配置邮件转发（如果需要）

如果想用 `contact@yourname.com` 这样的邮箱：
1. 进入 "Email" → "Email Routing"
2. 设置转发规则
3. 免费！

---

## 💡 专业建议

1. **使用根域名作为主域名**
   - `yourname.com` 比 `www.yourname.com` 更简洁
   - 但要确保两者都能访问

2. **开启所有安全功能**
   - Always Use HTTPS
   - Automatic HTTPS Rewrites
   - Browser Integrity Check

3. **优化性能**
   - Auto Minify (CSS, JS, HTML)
   - Brotli 压缩
   - HTTP/3 (QUIC)

4. **定期备份**
   - 代码已在 GitHub，天然备份
   - 可以随时回滚到任何版本

---

## 📞 需要帮助？

- Cloudflare 社区：https://community.cloudflare.com/
- Cloudflare Pages 文档：https://developers.cloudflare.com/pages/
- DNS 检查工具：https://www.whatsmydns.net/

---

## 🎉 总结

完成以上步骤后，你将拥有：

✅ 专业的个人主页网站
✅ 自定义域名（yourname.com）
✅ 免费 SSL 证书（HTTPS）
✅ 全球 CDN 加速
✅ 无限带宽
✅ 自动部署（推送代码即更新）
✅ 完全免费！

恭喜你！开始享受你的个人网站吧！🚀
