---
title: "打造會 Trash Talk 的 OX 遊戲 AI（Minimax + TypeScript）"
date: 2025-04-11T16:00:00+08:00
tags: ["AI", "Minimax", "TypeScript", "TicTacToe", "Unit Test"]
cover:
  image: "/images/posts/2025-04-11-tictactoe.webp"
  alt: "OX AI 對戰遊戲"
description: "一步步用 TypeScript 打造 OX 遊戲 AI，整合 minimax 演算法與 Vitest 單元測試，還能 trash talk！"
summary: "想做一個有點中二、會 Trash Talk 的 OX AI 對戰遊戲？  
這篇就帶你用 TypeScript、Minimax、單元測試，一次搞定！"

---

想做一個有點中二、會 Trash Talk 的 OX AI 對戰遊戲？  
這篇就帶你用 TypeScript、Minimax、單元測試，一次搞定！

---

## 🎬 Demo 操作影片（YouTube）

<iframe width="560" height="315" src="https://www.youtube.com/embed/wteKPhDTANw?si=k-Jck4BYNqnk3bYn" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

---

## 🎮 最終功能

- ✅ 使用 Minimax 演算法計算最強棋步
- ✅ 支援單元測試（使用 Vitest）
- ✅ AI 下棋支援聲音、動畫、手機震動
- ✅ 會 trash talk，還能用 SpeechSynthesisUtterance - Web APIs 說出來
- ✅ 整合 GitHub Actions 自動執行測試

---

## 🧠 Minimax AI 核心程式碼

```ts
const scores = { X: -100, O: 100, draw: 0 };

export function getBestMove(board: string[][]): { row: number; col: number } {
  let bestScore = -Infinity;
  let move = { row: 0, col: 0 };

  for (let i = 0; i < 3; i++) {
    for (let j = 0; j < 3; j++) {
      if (!board[i][j]) {
        board[i][j] = 'O';
        const score = minimax(board, 0, false);
        board[i][j] = '';
        if (score > bestScore) {
          bestScore = score;
          move = { row: i, col: j };
        }
      }
    }
  }

  return move;
}

export function minimax(board: string[][], depth: number, isMaximizing: boolean): number {
  const result = checkWinner(board);
  if (result !== null) {
    const score = scores[result] - depth;
    console.log(`[depth=${depth}] Result found: ${result}, score = ${score}`);
    return score;
  }

  if (isMaximizing) {
    let best = -Infinity;
    for (let i = 0; i < 3; i++)
      for (let j = 0; j < 3; j++)
        if (!board[i][j]) {
          board[i][j] = 'O';
          const score = minimax(board, depth + 1, false);
          board[i][j] = '';
          console.log(`[depth=${depth}] Maximizing: tried (${i},${j}) => score ${score}`);
          best = Math.max(score, best);
        }
    return best;
  } else {
    let best = Infinity;
    for (let i = 0; i < 3; i++)
      for (let j = 0; j < 3; j++)
        if (!board[i][j]) {
          board[i][j] = 'X';
          const score = minimax(board, depth + 1, true);
          board[i][j] = '';
          console.log(`[depth=${depth}] Minimizing: tried (${i},${j}) => score ${score}`);
          best = Math.min(score, best);
        }
    return best;
  }
}
```

---

## ✅ 單元測試：確保 AI 聰明又不踩錯格

```ts
it('should block opponent win', () => {
  const board = [
    ['X', 'X', ''],
    ['', '', ''],
    ['', '', ''],
  ];
  const move = getBestMove(board);
  expect(move).toEqual({ row: 0, col: 2 }); // 阻擋 X
});
```

---

## 🔁 自動化測試 CI

使用 GitHub Actions，每次 PR / push：

```yaml
name: Deploy Vite App to GitHub Pages

on:
  push:
    branches:
      - main

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout source
        uses: actions/checkout@v3

      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: 18

      - name: Install dependencies
        run: npm install

      - name: Run Vitest
        run: npm run test

      - name: Build Vite project
        run: npm run build
```

---

## 🚀 Demo + Open Source

GitHub Repo 👉 [詳細資訊請前往 github s](https://github.com/yu-sooong/ooxx)

Demo 👉 [立刻來玩看看](https://yu-sooong.github.io/ooxx/)

---

## ❤️ 感想

這次用 TypeScript + Tailwind 打造一個有趣的 AI OX 遊戲，  
還結合了 SpeechSynthesisUtterance - Web APIs、Trash Talk、單元測試、自動部署，真的非常好玩！

---

若你也有興趣玩這套遊戲或一起改進，歡迎 PR 或留言給我！  
下一步我可能會試著 🤖🧠

- [ ] 支援 NxN 棋盤擴充
- [ ] PvP 雙人模式
- [ ] 串接 實際 AI (HuggingFace/Open AI) / GPT 多模型切換

```