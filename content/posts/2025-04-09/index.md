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


這篇筆記整理我在 GitHub 專案 [`design-patterns-ts`](https://github.com/yu-sooong/design-patterns-ts) 中實作的幾個經典設計模式，包括：

- Singleton 單例模式
- Strategy 策略模式（付款策略）
- Proxy 代理模式
- Simple Factory 簡單工廠模式（角色訓練營）

每個模式皆使用 TypeScript 撰寫，並搭配 Jest 撰寫測試，適合用來練習 OOP、TDD 以及作為求職作品展示。

---

## 🧠 Singleton 單例模式

**目的：**  
確保一個類別在整個應用中只有一個實例，常見於設定管理、資料庫連線、日誌紀錄器等場景。

**程式碼簡介：**
```ts
export class Singleton {
  private static instance: Singleton;

  private constructor() {
    // 初始化邏輯
  }

  static getInstance(): Singleton {
    if (!Singleton.instance) {
      Singleton.instance = new Singleton();
    }
    return Singleton.instance;
  }

  someMethod(): string {
    return 'This is a singleton instance.';
  }
}
```

**測試重點：**
- 多次呼叫 `getInstance()` 回傳同一個物件
- 驗證方法功能正確性

---

## 💳 Strategy 策略模式（付款策略）

**目的：**  
封裝不同付款邏輯（如現金、信用卡、Apple Pay、PayPal），可在執行時動態替換策略，符合開放封閉原則。

**目錄結構：**
```
strategy/
├── interface/paymentStrategy.ts
├── applePayPayment.ts
├── cashPayment.ts
├── creditCardPayment.ts
├── payPalPayment.ts
└── paymentContext.ts
```

**介面定義：**
```ts
export interface PaymentStrategy {
  pay(amount: number): string;
}
```

**具體策略範例：**
```ts
export class CashPayment implements PaymentStrategy {
  pay(amount: number): string {
    return `Paid $${amount} using Cash`;
  }
}
```

**上下文類別：**
```ts
export class PaymentContext {
  constructor(private strategy: PaymentStrategy) {}

  setStrategy(strategy: PaymentStrategy): void {
    this.strategy = strategy;
  }

  pay(amount: number): string {
    return this.strategy.pay(amount);
  }
}
```

**測試場景：**
- 切換不同策略執行付款
- 驗證每種策略輸出是否正確

---

## 🧥 Proxy 代理模式

**目的：**  
為其他物件提供一個代理來控制對它的訪問，常用於：存取控制、延遲初始化、日誌紀錄等。

**目錄結構：**
```
proxy/
├── interface/image.ts
├── realImage.ts
└── proxyImage.ts
```

**介面定義：**
```ts
export interface Image {
  display(): string;
}
```

**真實物件：**
```ts
export class RealImage implements Image {
  constructor(private filename: string) {}

  display(): string {
    return `Displaying ${this.filename}`;
  }
}
```

**代理物件：**
```ts
export class ProxyImage implements Image {
  private realImage: RealImage | null = null;

  constructor(private filename: string) {}

  display(): string {
    if (!this.realImage) {
      this.realImage = new RealImage(this.filename);
    }
    return this.realImage.display();
  }
}
```

**測試場景：**
- 第一次呼叫會載入圖片
- 後續呼叫不會重複載入

---

## 🏭 Simple Factory 簡單工廠模式（角色訓練營）

**目的：**  
集中角色的建立邏輯，根據不同角色類型自動產生對應實例。

**目錄結構：**
```
simple-factory/
├── enum/characterType.ts
├── interface/character.ts
├── archer.ts
├── warrior.ts
└── trainingCamp.ts
```

**角色介面：**
```ts
export interface Character {
  getDescription(): string;
}
```

**角色實作範例：**
```ts
export class Archer implements Character {
  getDescription(): string {
    return 'I am an Archer.';
  }
}
```

**工廠類別：**
```ts
export class TrainingCamp {
  trainCharacter(type: CharacterType): Character {
    switch (type) {
      case CharacterType.Archer:
        return new Archer();
      case CharacterType.Warrior:
        return new Warrior();
      default:
        throw new Error('Invalid character type.');
    }
  }
}
```

**測試場景：**
- 呼叫 `trainCharacter(CharacterType.Archer)` 是否產生弓箭手
- 驗證描述是否正確輸出

---

## 🔍 總結

| 模式 | 特色 |
|------|------|
| Singleton | 單一實例，共用狀態 |
| Strategy | 動態替換付款邏輯 |
| Proxy | 控制與延遲對真實物件的存取 |
| Simple Factory | 將建立邏輯集中化管理 |

---

📦 專案 GitHub Repo：  
👉 [https://github.com/yu-sooong/design-patterns-ts](https://github.com/yu-sooong/design-patterns-ts)

🧪 所有模式皆搭配單元測試撰寫，歡迎參考與 Star ⭐

📬 如有想法交流，歡迎聯絡我：[ssocde543@gmail.com](mailto:ssocde543@gmail.com)
```