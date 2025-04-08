---
title: "✨部落格打造過程與 Netlify 部署心得"
date: 2025-04-07T15:00:00+08:00
draft: false
cover:
  image: "/images/posts/2025-04-08-deploy.webp"
  alt: "封面圖"
  responsive: true
url: "/posts/2025-04-08/hugo-paper-deploy/"
categories: ["Hugo 教學"]
tags: ["Hugo", "PaperMod", "靜態網站"]

---

最近完成了自己的部落格建置，這篇紀錄一下整個調整、踩坑、優化與部署的過程！

<!--more-->

---

## 🌐 使用 Netlify 部署流程

整合 GitHub 自動部署的方式非常順暢，部署步驟如下：

### ✅ 1. 專案準備

- 確認本地已可使用 `hugo` 正常建置
- 將整個 Hugo 專案推上 GitHub Repo（如 `myblog`）
- 確保你的 `config.yaml` 中 `baseURL` 設為：

```yaml
baseURL: "https://你的網域.netlify.app/"
```

---

### ✅ 2. 新增 Netlify 設定檔 `netlify.toml`

在根目錄建立 `netlify.toml`，內容如下：

```toml
[build]
  publish = "public"
  command = "hugo"

[context.production.environment]
  HUGO_VERSION = "0.143.1"
```

這樣 Netlify 才知道要用 Hugo 哪個版本來建置你的網站。

---

### ✅ 3. Netlify 介面操作（Import Git）

- 前往 [https://app.netlify.com/](https://app.netlify.com/)
- 點「Add new site → Import an existing project」
- 選擇 GitHub → 授權並選擇你的 Repo
- 在設定中填入：

| 設定項目 | 值 |
|----------|----|
| **Build Command** | `hugo` |
| **Publish Directory** | `public` |
| **環境變數 HUGO_VERSION** | `0.143.1`（如果沒用 netlify.toml 的話） |

- 點擊「Deploy site」開始首次部署

---

### ✅ 4. 完成後

Netlify 會自動分配網址，例如：

```
https://my-hugo-blog.netlify.app/
```

你也可以在 `Site settings → Domain` 中設定自訂子網域或綁定自己的網域。

---

### ✅ 5. 每次更新內容

只要你更新內容並 push 到 GitHub，Netlify 就會自動重新建置部署，非常方便：

```bash
git add .
git commit -m "更新內容"
git push
```

---

如果你也想打造個人技術站，推薦你從 Hugo + PaperMod 開始！  
部署到 Netlify 幾乎零痛感，也支援自訂網域、HTTPS、自動部署 ✨

> 有任何 Hugo 或 Netlify 部署、樣式問題，也歡迎留言或聯絡我！