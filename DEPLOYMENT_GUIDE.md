# Cloudflare Pages 部署指南

本指南将教你如何将个人主页部署到 Cloudflare Pages，完全免费！

## 前提条件

1. 一个 Cloudflare 账号（免费）- 访问 https://dash.cloudflare.com/sign-up 注册
2. 一个 GitHub 账号 - 访问 https://github.com 注册
3. Git 已安装在你的电脑上

## 部署步骤

### 第一步：将代码上传到 GitHub

1. **创建 GitHub 仓库**
   - 访问 https://github.com/new
   - 仓库名称输入：`my-homepage`（或任何你喜欢的名字）
   - 选择 "Public"（公开）
   - 不要勾选 "Initialize this repository with a README"
   - 点击 "Create repository"

2. **初始化本地 Git 仓库并推送代码**

   在项目文件夹中打开终端（命令提示符或 PowerShell），执行以下命令：

   ```bash
   # 初始化 Git 仓库
   git init

   # 添加所有文件
   git add .

   # 创建第一个提交
   git commit -m "Initial commit: 创建个人主页"

   # 添加 GitHub 仓库为远程仓库（替换 YOUR_USERNAME 为你的 GitHub 用户名）
   git remote add origin https://github.com/YOUR_USERNAME/my-homepage.git

   # 推送代码到 GitHub
   git branch -M main
   git push -u origin main
   ```

   如果遇到认证问题，GitHub 会提示你登录。

### 第二步：在 Cloudflare Pages 创建项目

1. **登录 Cloudflare Dashboard**
   - 访问 https://dash.cloudflare.com
   - 登录你的账号

2. **进入 Pages**
   - 在左侧菜单中找到 "Workers & Pages"
   - 点击 "Create application"
   - 选择 "Pages" 标签
   - 点击 "Connect to Git"

3. **连接 GitHub**
   - 点击 "Connect GitHub"
   - 授权 Cloudflare 访问你的 GitHub 账号
   - 选择你刚才创建的 `my-homepage` 仓库

4. **配置构建设置**
   - **Project name**: my-homepage（或自定义）
   - **Production branch**: main
   - **Framework preset**: None（选择"无"）
   - **Build command**: 留空（不需要）
   - **Build output directory**: /（根目录）

   点击 "Save and Deploy"

5. **等待部署完成**
   - Cloudflare 会自动构建和部署你的网站
   - 通常只需要几秒钟
   - 部署成功后，你会看到一个 `.pages.dev` 的网址

### 第三步：访问你的网站

部署完成后，你会得到一个类似这样的网址：
```
https://my-homepage-xxx.pages.dev
```

点击链接即可访问你的个人主页！🎉

## 自定义域名（可选）

如果你有自己的域名，可以绑定到 Cloudflare Pages：

1. 在 Cloudflare Pages 项目页面，点击 "Custom domains"
2. 点击 "Set up a custom domain"
3. 输入你的域名（如：www.yourname.com）
4. 按照提示配置 DNS 记录
5. 等待 DNS 生效（通常几分钟到几小时）

## 更新网站内容

每次你想更新网站时：

1. **修改本地文件**（编辑 index.html、style.css 等）

2. **提交并推送更改**
   ```bash
   git add .
   git commit -m "描述你的更改"
   git push
   ```

3. **自动部署**
   - Cloudflare Pages 会自动检测到新的提交
   - 自动重新部署你的网站
   - 几秒钟后新版本就上线了

## 个性化你的网站

在 `index.html` 中修改：

1. **个人信息**
   - 第 21 行：修改你的名字
   - 第 35-37 行：修改关于我的描述

2. **技能标签**
   - 第 43-50 行：修改或添加你的技能

3. **项目信息**
   - 第 58-80 行：修改项目标题、描述和链接

4. **联系方式**
   - 第 91-106 行：更新你的邮箱和社交媒体链接

## 常见问题

### Q: 部署是免费的吗？
A: 是的！Cloudflare Pages 提供免费的托管服务，包括：
   - 无限带宽
   - 每月 500 次构建
   - 免费 SSL 证书
   - 全球 CDN 加速

### Q: 我可以使用自己的域名吗？
A: 可以！Cloudflare 支持自定义域名，而且完全免费。

### Q: 如何查看网站访问统计？
A: 在 Cloudflare Pages 项目页面，点击 "Analytics" 可以查看访问数据。

### Q: 我不懂编程，可以修改吗？
A: 可以！HTML 文件中的文本都可以直接修改，不需要编程知识。

### Q: 部署失败怎么办？
A: 检查以下几点：
   - 确保所有文件都已推送到 GitHub
   - 检查 Build output directory 设置为 `/`
   - 查看 Cloudflare 的构建日志了解错误信息

## 进阶优化

### 添加网站图标（Favicon）
在 `index.html` 的 `<head>` 部分添加：
```html
<link rel="icon" type="image/png" href="favicon.png">
```
然后将你的图标文件命名为 `favicon.png` 放在项目根目录。

### 添加 Google Analytics
在 `index.html` 的 `</head>` 标签前添加 Google Analytics 代码。

### 添加 Open Graph 标签（用于社交分享）
在 `index.html` 的 `<head>` 部分添加：
```html
<meta property="og:title" content="我的个人主页">
<meta property="og:description" content="欢迎来到我的个人主页">
<meta property="og:image" content="https://your-site.pages.dev/preview.jpg">
```

## 需要帮助？

- Cloudflare Pages 文档：https://developers.cloudflare.com/pages/
- GitHub 文档：https://docs.github.com/
- Git 教程：https://git-scm.com/book/zh/v2

## 下一步

1. ✅ 修改 HTML 文件中的个人信息
2. ✅ 替换联系方式链接
3. ✅ 上传到 GitHub
4. ✅ 在 Cloudflare Pages 部署
5. ✅ 分享你的网站链接！

祝你部署成功！🚀
