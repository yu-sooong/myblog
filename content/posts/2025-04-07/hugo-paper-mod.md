---
title: "使用 Hugo + PaperMod 建立個人部落格教學"
date: 2025-04-07T15:00:00+08:00
draft: false
cover:
  image: "/images/posts/posts.webp"
  alt: "封面圖"
  responsive: true
url: "/posts/2025-04-07/hugo-paper-mod/"
categories: ["Hugo 教學"]
tags: ["Hugo", "PaperMod", "靜態網站"]

---
為什麼選擇 PaperMod ?
<!--more-->
## ✨ 為什麼選擇 PaperMod？

- 美觀、簡約、輕量
- 支援語法高亮、Mermaid 圖表、Lightbox、社群卡片預覽
- 適合寫 Resume / Timeline / Blog

---

## ✅ 安裝流程

### 1. 安裝 Hugo Extended 版本

```bash
brew install hugo
# 或到 https://github.com/gohugoio/hugo/releases 下載 Hugo Extended
```

---

### 2. 建立站台

```bash
hugo new site my-blog
cd my-blog
```

---

### 3. 安裝 PaperMod 主題

```bash
git init
git submodule add https://github.com/adityatelange/hugo-PaperMod themes/PaperMod
```

---

### 4. 設定 config.yaml

在專案根目錄建立 `config.yaml`，內容範例如下：

```yaml
baseURL: "https://yourdomain.com/"
languageCode: "zh-tw"
title: "你的部落格名稱"
theme: "PaperMod"

params:
  homeInfoParams:
    Title: "Hi, 我是 3T 👋"
    Content: "這裡是我的技術與生活紀錄"

  socialIcons:
    - name: github
      url: "https://github.com/yu-sooong"
    - name: email
      url: "mailto:ssocde543@gmail.com"

menu:
  main:
    - identifier: blog
      name: Blog
      url: /posts/
      weight: 1
    - identifier: about
      name: About
      url: /about/
      weight: 2
```

⚠️ 注意：如果有 `hugo.toml`，請刪除，只留 `config.yaml`。

---

### 5. 建立第一篇文章

```bash
hugo new posts/hello-world.md
```

修改該檔案內容：

```markdown
---
title: "Hello World"
date: 2025-04-07T15:00:00+08:00
draft: false
---

這是我的第一篇文章！
```

---

### 6. 本地預覽

```bash
hugo server -D
```

打開 `http://localhost:1313/` 瀏覽你的部落格！

---

## 🛠 常見錯誤排除

### 🔺 沒有看到首頁內容？
- `posts/*.md` 是不是還是 `draft: true`？
- `config.yaml` 是否正確指定 `theme: PaperMod`？
- 是否有多個設定檔（例如 `hugo.toml` 和 `config.yaml`）？

### 🔺 WARN: found no layout file
代表主題沒有正確啟用，請確認：
- `themes/PaperMod/` 存在
- `config.yaml` 有指定 `theme: "PaperMod"`
- `hugo.toml` 被刪除（以免被讀取）

---

## 🧪 下一步：功能擴充（推薦）

- Timeline / Resume 模板
- Mermaid 圖表支援
- Lightbox 圖片效果
- Sitemap / RSS / robots.txt
- 社群預覽圖卡 (Open Graph)
- 自動部署 GitHub Pages / Netlify

---

有任何問題歡迎留言交流，祝大家 Hugo + PaperMod 架站順利 🚀
