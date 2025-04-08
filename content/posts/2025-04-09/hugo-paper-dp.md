---
title: "Design Patterns in TypeScript：用設計模式打造可測試的架構"
date: 2025-04-08T12:21:11+08:00
draft: false
cover:
  image: "/images/posts/2025-04-08-dp.webp"
  alt: "封面圖"
  responsive: true
url: "/posts/2025-04-09/hugo-paper-dp/"
categories: ["技術文章"]
tags: ["TypeScript", "Design Patterns", "TDD", "OOP"]
summary: "這是一個使用 TypeScript 實作經典設計模式的學習型專案，搭配 Jest 撰寫單元測試。"
---

這是一個使用 **TypeScript** 撰寫的設計模式學習專案，目標是實作 GoF 經典設計模式，並搭配 **Jest** 撰寫單元測試，強化可維護性與可讀性。

👉 GitHub Repo：[yu-sooong/design-patterns-ts](https://github.com/yu-sooong/design-patterns-ts)

---

## 💡 專案動機

身為後端工程師，在開發大型系統時往往需要重構、模組化與抽象化，而設計模式正是建立這些能力的基礎。

本專案目的為：
- 熟悉 TypeScript 中如何實作經典設計模式
- 練習 TDD 思維（測試先行）
- 強化物件導向架構設計能力
- 準備面試作品集或教學內容

---

## 📁 專案結構與工具

```bash
src/
│── patterns/
│   ├── singleton/
│   ├── factory/
│   ├── strategy/
│   ├── proxy/
│── tests/
│── .github/workflows/ci.yml
│── coverage/
│── package.json
│── jest.config.js
│── tsconfig.json

```

### 🔧 使用工具

| 工具          | 說明                         |
|---------------|------------------------------|
| TypeScript     | 強型別語言，增強架構穩定性   |
| Jest           | 測試框架，支援 mock 與斷言    |
| ts-node        | 即時執行 TypeScript 程式碼   |
| Node.js        | 執行環境                     |

---

## 🧠 實作的設計模式

### 🏗️ Creational 創建型

| 模式        | 說明                                                   |
|-------------|--------------------------------------------------------|
| Factory     | 封裝建立物件的邏輯，減少耦合                           |
| Singleton   | 確保整個應用中只有一個實例（常見於設定、Log 等）      |

---

### 🧩 Structural 結構型

| 模式        | 說明                                                   |
|-------------|--------------------------------------------------------|
| Adapter     | 將不相容的介面進行轉換                                 |
| Decorator   | 在不改變原物件的前提下動態擴充其功能                   |

---

### 🔁 Behavioral 行為型

| 模式        | 說明                                                   |
|-------------|--------------------------------------------------------|
| Strategy    | 封裝演算法並可動態切換，符合開放封閉原則               |
| Observer    | 實作事件訂閱/通知機制，常見於 UI、即時通知等場景       |

---

## 🧪 單元測試設計（TDD）

所有設計模式皆搭配 **Jest** 撰寫單元測試，驗證功能正確性與設計邏輯。

### 📌 測試範例：Singleton

```ts
describe('Singleton Pattern', () => {
  it('should return the same instance', () => {
    const instance1 = Singleton.getInstance();
    const instance2 = Singleton.getInstance();

    expect(instance1).toBe(instance2);
  });
});
```

- ✅ 驗證是否為同一實例
- ✅ 可擴充行為測試

### 📌 測試範例：Strategy

```ts
describe('Strategy Pattern', () => {
  it('should execute the correct strategy', () => {
    const context = new Context(new ConcreteStrategyA());
    expect(context.executeStrategy()).toBe('Strategy A');

    context.setStrategy(new ConcreteStrategyB());
    expect(context.executeStrategy()).toBe('Strategy B');
  });
});
```

---

## ✨ 專案亮點與價值

- ✅ **100% 單元測試覆蓋**
- 🧱 **架構清晰、模組化分類**
- 🔁 **對應現實場景的模式實作**
- 📘 **適合教學 / 作品集 / 面試使用**
- 🚀 **可作為 TDD 或 OOP 架構模板**

---

## 🧭 延伸方向建議

| 項目 | 說明 |
|------|------|
| ➕ 增加更多設計模式 | 如 Command、State、Mediator、Prototype 等 |
| 🖼️ 加入 UML / Mermaid 圖示 | 輔助理解設計模式架構 |
| 🧪 加入測試覆蓋率報表 | 使用 `jest --coverage` |
| 🔄 加入 GitHub Actions CI | PR 自動測試，提升專案專業度 |
| 💼 整合實際業務案例 | 將模式應用在簡易 Web 或 CLI 專案中 |

---

## 📚 學習資源推薦

- 書籍：《Head First Design Patterns》、《Refactoring to Patterns》
- GitHub 開源專案：`refactoring.guru`、`The TypeScript Handbook`
- 視覺化學習設計模式：[https://refactoring.guru/design-patterns](https://refactoring.guru/design-patterns)

---

## 📬 聯絡與貢獻

如果你覺得這個專案對你有幫助，歡迎：
- ⭐ GitHub 給個星：[design-patterns-ts](https://github.com/yu-sooong/design-patterns-ts)
- 👋 寄信給我：ssocde543@gmail.com
- 💬 一起討論設計模式或開發心得！

---