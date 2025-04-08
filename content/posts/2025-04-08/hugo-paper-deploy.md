---
title: "✨ 我的部落格打造過程與 Netlify 部署心得"
date: 2025-04-08T20:00:00+08:00
draft: false
tags: ["Hugo", "PaperMod", "Netlify", "Blog 部署"]
categories: ["技術心得"]
cover:
  image: "/images/posts/2025-04-08-deploy.png"
  alt: "部落格部署封面"
  responsive: true
---

最近完成了自己的部落格建置，這篇紀錄一下整個調整、踩坑、優化與部署的過程！

<!--more-->

---

## 🚀 為什麼選擇 Hugo + PaperMod？

- 極致輕量又漂亮（真的）
- Markdown 撰寫好維護
- 社群活躍，PaperMod 功能齊全（支援封面圖、TOC、Lightbox、社群卡、Mermaid）

---

## 🧰 自訂與優化內容整理

✅ 首頁使用 `profileMode` 精簡呈現  
✅ 自訂主題色、字型大小  
✅ 支援文章封面圖比例統一（3:2、1536x1024）  
✅ 新增 TOC 預設展開、字數顯示、閱讀時間  
✅ About 頁面製作雙欄式 Resume，結合技術卡片與 CSS 樣式  
✅ 部落格列表 hover 動畫與 icon 加強  
✅ 社群預覽卡 / 分享連結（部份還在調整）

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