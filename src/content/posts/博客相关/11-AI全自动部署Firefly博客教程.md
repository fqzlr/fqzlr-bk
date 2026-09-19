---
title: "零成本搭建 Firefly (Astro) 个人博客，AI 全自动部署教程"
published: 2026-09-15
updated: 2026-09-15
description: "博客系列第11期。Windows 全流程零基础教程：Fork 仓库、Trae AI 自然语言全自动改站、Vercel / Cloudflare 双平台免费部署，全程零服务器、零成本、不用背命令。"
image: "api"
tags:
  - 博客
  - Firefly
  - Astro
  - Trae
  - Vercel
  - Cloudflare
slug: ai-auto-deploy-firefly-blog
category: 博客相关
draft: false
pinned: false
author: fqzlr
---

# 零成本搭建 Firefly (Astro) 个人博客，AI 全自动部署教程

> 全程不需要服务器、不需要付费、不需要背命令。你要做的只有三件事：注册账号、点鼠标、用中文告诉 AI 你想要什么。

## 一、方案总览

本教程的完整链路：

```text
Fork 官方仓库 → Trae AI 克隆项目 → AI 安装依赖 → AI 修改配置
→ 本地预览 → AI 写第一篇文章 → AI 推送 GitHub → Vercel / Cloudflare 自动部署
```

三个关键词：

- **Firefly**：基于 Astro 的开源博客主题，构建产物是纯静态页面，不需要服务器跑后端
- **Trae**：AI 编程 IDE（字节出品，内置 Work 智能体），用自然语言驱动它完成克隆、改配置、写文章、Git 操作
- **Vercel / Cloudflare**：免费静态托管平台，连接 GitHub 仓库后，每次推送代码自动重新构建上线

## 二、前置准备

### 1. 需要注册的账号

| 账号 | 用途 | 官网 | 费用 |
| --- | --- | --- | --- |
| GitHub | 存放博客源码，触发自动部署 | https://github.com | 免费 |
| Trae | AI 编程 IDE（国内版登录即可用 AI） | https://www.trae.com.cn | 免费 |
| Vercel | 部署方案一（国外访问快） | https://vercel.com | 免费 |
| Cloudflare | 部署方案二（二选一即可） | https://dash.cloudflare.com | 免费 |
| 域名（可选） | 自定义访问地址 | 任意注册商，如 Spaceship | 6~20 元/年，可不买 |

> 部署平台二选一即可，后续想换也能随时迁移。没有域名也能部署，平台会送一个 `xxx.vercel.app` / `xxx.pages.dev` 的免费二级域名。

### 2. 需要安装的软件

| 软件 | 版本要求 | 用途 | 官网 |
| --- | --- | --- | --- |
| Node.js | **≥ 22 LTS**（选 LTS 版） | 博客构建运行环境 | https://nodejs.org |
| Git | 最新版即可 | 版本管理、推送代码 | https://git-scm.com |
| Trae | 最新版 | AI IDE，克隆/改站/写文章/推送 | https://www.trae.com.cn |
| pnpm | 最新版（命令安装） | 依赖包管理器 | 命令安装，见下文 |

### 3. 安装 pnpm（唯一需要敲的一条命令）

装完 Node.js 和 Git 后，按 `Win + R` 输入 `powershell` 回车，粘贴执行：

```powershell
npm install -g pnpm
```

验证（输出版本号即成功，权限不足就右键"以管理员身份运行"终端再执行一次）：

```powershell
node -v   # 应显示 v22.x
git --version
pnpm -v
```

## 三、Fork 官方仓库（1 分钟）

1. 打开 Firefly 官方仓库：**https://github.com/CuteLeaf/Firefly**
2. 登录 GitHub 后，点右上角 **Fork** 按钮
3. 保持默认选项，点 **Create fork**
4. 等待 3~5 秒，你就拥有了属于自己的仓库：`https://github.com/你的用户名/Firefly`

> 为什么 Fork 而不是下载压缩包？Fork 后你的仓库和部署平台是连通的，以后每次改动推送到 GitHub，博客都会自动重新构建上线。

## 四、Trae AI 克隆项目

1. 打开 Trae，在右侧打开 **AI 对话窗口**（快捷键 `Ctrl + U` 唤起 Builder / Work 模式）
2. 用自然语言告诉它（把用户名换成你的）：

```text
帮我把 https://github.com/你的用户名/Firefly 克隆到本机，目录放在 D:\blog\Firefly
```

3. Trae 会自动执行 git clone，克隆完成后点"接受"即可
4. 首次打开项目文件夹时选择"信任此文件夹"

> 国内网络克隆慢或失败，先开启代理软件，再补一句："用代理 127.0.0.1:7890 重新克隆"。

## 五、安装依赖 + 本地预览

继续在 Trae 对话框里说：

```text
用 pnpm 安装项目依赖，安装完成后启动开发服务器
```

Trae 会自动执行 `pnpm install` 和 `pnpm dev`。等它告诉你本地地址后，浏览器打开：

```text
http://localhost:4321
```

看到博客首页，本地环境就全部搭好了。以后想重新启动预览，只需要对 Trae 说"启动开发服务器"。

## 六、修改博客配置（核心文件详解）

Firefly 是配置驱动的设计，**改站 = 改配置文件**，全部集中在 `src/config/` 目录。你不需要自己找，直接把下面两张表给 AI，然后用中文说需求。

### 1. 常用配置文件速查表

| 配置文件 | 作用 |
| --- | --- |
| `siteConfig.ts` | 站点核心：标题、副标题、网址、描述、关键词、主题色、页面开关 |
| `profileConfig.ts` | 侧栏个人卡片：头像、作者名、个人简介、社交链接 |
| `navBarConfig.ts` | 顶部导航栏：菜单项、下拉分组、顺序 |
| `footerConfig.ts` | 页脚：备案号、版权、自定义链接（HTML 模板在 `FooterConfig.html`） |
| `friendsConfig.ts` | 友链：好友头像、描述、地址、随机排序等 |
| `commentConfig.ts` | 评论系统：Waline / Twikoo / giscus 接入参数 |
| `sponsorConfig.ts` | 赞助页：收款码、赞助列表 |
| `fontConfig.ts` | 全站字体：正文、标题、代码字体 |
| `announcementConfig.ts` | 全站公告条 |
| `backgroundWallpaper.ts` | 背景壁纸：轮播图、每日一图 |
| `sideBarConfig.ts` | 侧栏模块开关与顺序 |
| `dynamicConfig.ts` | 说说/动态页配置 |

### 2. 对 AI 说需求（直接复制改词）

```text
把站点标题改成「XX的博客」，副标题改成「XX」，作者名改成 XX，
个人简介改成「XXX」，社交链接换成我的 GitHub（地址：xxx）和 B 站（地址：xxx）
```

```text
导航栏只保留：文章、归档、关于 这三项，其他菜单去掉
```

```text
主题色改成蓝色系，开启暗色模式跟随系统
```

Trae 会自动定位配置文件、修改并保存，你刷新浏览器就能看到效果。**改错了就说"撤销刚才的修改"**，这是 AI 驱动最大的好处。

### 3. 页面开关（siteConfig.ts 里的 pages）

想关掉某个不用的页面（比如赞助页、相册页），对 AI 说：

```text
把赞助页面和音乐页面关掉，设置成返回404并从导航栏隐藏
```

对应的就是 `siteConfig.ts` 顶部 `pages` 里的开关，`true` 开 / `false` 关。

## 七、新建第一篇文章

文章全部放在 `src/content/posts/` 目录，按分类建文件夹（如 `tech/`、`life/`），是 Markdown 文件。对 Trae 说：

```text
帮我写一篇标题为《我的第一篇博客》的文章，内容是我为什么开始写博客、
后续打算写什么。参考 posts 目录下已有文章的 frontmatter 格式，
保存到 tech 分类目录下，tags 加上"随笔"
```

生成的文件开头是 frontmatter（元信息），关键字段：

```yaml
---
title: "文章标题"          # 必填
published: 2026-09-15      # 发布日期
description: "摘要"        # 列表页摘要
tags: [随笔]               # 标签
category: tech             # 分类
draft: false               # true = 草稿不发布
pinned: false              # true = 置顶
---
```

 frontmatter 下面就是正文，标准 Markdown 语法，支持代码块、表格、图片。

## 八、推送代码到 GitHub

本地满意后，对 Trae 说：

```text
提交所有改动并推送到 GitHub 的 main 分支
```

Trae 会自动完成 `git add → git commit → git push`。推送成功后，你的 GitHub 仓库里就能看到最新代码——**到这里，源码托管完成，接下来只差部署**。

> 首次推送时如果弹出 GitHub 登录窗口，按提示在浏览器授权即可。国内推送失败就开代理，对 Trae 说"用代理 127.0.0.1:7890 推送"。

## 九、部署方案一：Vercel（推荐新手）

1. 打开 https://vercel.com ，点 **Sign Up → Continue with GitHub**，用 GitHub 账号直接登录
2. 授权后进入控制台，点 **Add New → Project**
3. 在 **Import Git Repository** 列表里找到你的 `Firefly` 仓库，点 **Import**
4. 配置页按下面填（多数会自动识别，核对即可）：

| 配置项 | 填写值 |
| --- | --- |
| Framework Preset | `Astro`（自动识别） |
| Root Directory | 留空 |
| Build Command | `pnpm build` |
| Output Directory | `dist` |
| Install Command | `pnpm install` |

5. 点 **Deploy**，等待 1~3 分钟构建完成
6. 完成后页面会给出你的博客地址：`https://你的项目名.vercel.app`，点开即是你的博客

**绑定自定义域名（可选）**：项目页 → Settings → Domains → 输入你的域名 → 按提示去域名注册商添加一条 CNAME 记录指向 Vercel 给出的地址，等 DNS 生效（几分钟到几小时）即可用自己域名访问，HTTPS 证书自动配好。

## 十、部署方案二：Cloudflare（国内可达性更好）

1. 打开 https://dash.cloudflare.com ，注册/登录
2. 左侧菜单 **Workers & Pages → Create → Pages → Connect to Git**
3. 选择 GitHub 并授权，选中你的 `Firefly` 仓库，点 **Begin setup**
4. 构建配置按下面填：

| 配置项 | 填写值 |
| --- | --- |
| Project name | 随意（决定免费域名前缀） |
| Production branch | `main` |
| Build command | `pnpm build` |
| Build output directory | `/dist` |

5. 点 **Save and Deploy**，等待构建完成
6. 完成后获得免费域名：`https://你的项目名.pages.dev`

**绑定自定义域名（可选）**：项目 → Custom domains → Set up a custom domain，输入域名；如果域名已托管在 Cloudflare，DNS 会自动配置，无需手动加记录。

> 两个平台选择建议：主要给国内读者看 → Cloudflare；追求构建速度和生态 → Vercel。也可以两个都连，用域名 DNS 分线路解析。

## 十一、日常维护：写文章 → 自动上线

搭好之后，你的日常发文流程只剩三句话：

```text
1. 对 Trae 说：帮我写一篇《文章标题》的文章，重点讲 XXX，保存到 XX 分类
2. 预览确认：浏览器打开 localhost:4321 检查排版
3. 对 Trae 说：提交并推送
```

推送后 **Vercel / Cloudflare 检测到新提交，自动构建、自动上线**，1~3 分钟后全网可见，你不需要碰任何部署按钮。

改配置同理：想换头像、加友链、调导航，全部用中文告诉 Trae，改完推送即生效。

## 十二、常见问题速查

| 问题 | 解决 |
| --- | --- |
| `pnpm install` 报错 | 确认 Node ≥ 22；删除 `node_modules` 后重装；换网络/开代理 |
| 本地预览空白 | 对 Trae 说"查看开发服务器终端报错并修复" |
| 推送 GitHub 超时 | 开代理，告诉 Trae 代理端口 |
| Vercel 构建失败 | 检查 Build Command 是否为 `pnpm build`；对 Trae 说"读取构建日志修复" |
| 改了配置没生效 | 硬刷新 `Ctrl + Shift + R`；开发服务器重启（对 Trae 说即可） |

## 十三、总结：这套方案强在哪

- **零服务器**：静态站点托管在 Vercel / Cloudflare 免费额度内，个人博客完全够用
- **零成本**：不买服务器、不买对象存储也能跑，唯一可选支出是一个域名（6 元/年起）
- **AI 全自动**：克隆、装依赖、改配置、写文章、Git 推送，全部用自然语言驱动 Trae 完成，全程不需要记忆任何命令
- **自动持续部署**：代码推上 GitHub 就自动构建上线，写博客的体验接近"存稿即发布"
- **完全拥有**：源码在你自己的 GitHub 仓库，随时可以迁移到任何静态托管平台

相关源码：[github.com/CuteLeaf/Firefly](https://github.com/CuteLeaf/Firefly)

祝搭建顺利，欢迎在评论区贴出你的博客地址。
