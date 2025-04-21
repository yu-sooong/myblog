---
title: "打造隨機美食推薦系統 (POC)"
date: 2025-04-14T12:00:00+08:00
description: "使用 Node.js 搭配非公開 API 建構美食推薦系統實作 POC ，並思考如何轉型為 Line Bot 的技術實踐與踩坑記錄。"
tags: [Google Maps, Line Bot, Web Scraping, Node.js, TypeScript, Clean Code, Hugo, PaperMod]
cover:
   image: "/images/posts/2025-04-21-fav-recommender.webp"
   alt: "打造隨機美食推薦系統"
---

> 用自己的位置，推薦附近好吃的！

## 💡 發想起源


每次到了吃飯時間或是跟另一半出遊時、總是會問 「要吃什麼！？」

為了解決 ```都可以```、```隨便``` 的狀況出現避免延伸的可怕事情發生(誤  

我開始想：「能不能做一個簡單的服務，根據目前的位置，自動推薦附近 *我自己收藏過的美食*？」

再進一步：

- ✅ 能讀取我在 Google Maps 收藏的清單
- ✅ 根據經緯度找出最近的店家
- ✅ 每次亂數推薦一筆
- ✅ 未來還能透過 Line Bot 或 Web 前端互動

## 🛠️ 技術選型

| 功能      | 技術選型                             |
|---------|----------------------------------|
| 清單資料來源  | Google Maps 非公開 API (entitylist) |
| 後端程式語言  | Node.js + TypeScript             |
| 定位與距離計算 | Haversine Formula                |
| 前期實作形式  | CLI + JSON 檔案                    |
| API 串接  | Google Map API 串接實作              |


## 📦 初版功能說明

1. 透過 Google Maps 的 `getlist` 非公開 API，取得我的私人收藏清單（需從網址手動取得 Token）
   ![favlist](https://raw.githubusercontent.com/yu-sooong/ting-image/refs/heads/main/googlemap_favlist.png)
2. 把清單寫入本地端 `places.json`
3. 計算使用者座標與每間店家的距離，取出最近的三筆，再從中隨機挑出一筆
4. 再透過 Google Maps 的 `textsearch` API 取得該地點的 `place_id` 與正式地圖連結

## 🧠 清單資料結構解析

API 回傳的是類似 `)]}'\n[[...]]` 的奇怪格式，必須先 `replace(/^\)\]\}'/, '')` 才能轉為 JSON。

每筆店家資訊如下：

```ts
[
  0,
  [null, ..., ..., [address], ..., [latLng]],
  "店名",
  ...
  "https://maps.google.com/?cid=1234567890123456789" // 如果有，為原始地圖連結
]
```

但這個 `cid` 並不一定存在，且容易失效，因此改使用 `place_id` 比較穩定。

## 🧭 距離計算與推薦演算法

透過經緯度取得兩點間距離 半正矢公式 (Haversine formula)
```ts
function getDistance(lat1: number, lng1: number, lat2: number, lng2: number): number {
    const toRad = (x: number) => (x * Math.PI) / 180;
    const R = 6371;
    const dLat = toRad(lat2 - lat1);
    const dLng = toRad(lng2 - lng1);
    const a =
        Math.sin(dLat / 2) * Math.sin(dLat / 2) +
        Math.cos(toRad(lat1)) * Math.cos(toRad(lat2)) *
        Math.sin(dLng / 2) * Math.sin(dLng / 2);
    const c = 2 * Math.atan2(Math.sqrt(a), Math.sqrt(1 - a));
    return R * c;
}
```
透過自身位置取得 Google Map 清單中最近的距離( 2km 內)並且排序
```ts
function getNearbyPlaces(places, lat, lng, maxDistance = 2000) {
  return places
    .map(p => ({ ...p, distance: getDistance(lat, lng, p.lat, p.lng) }))
    .filter(p => p.distance <= maxDistance)
    .sort((a, b) => a.distance - b.distance);
}
```


為了讓推薦更有趣，我只取最近三筆，再 `Math.random()` 選出一筆回傳。

![googlemap_random](https://raw.githubusercontent.com/yu-sooong/ting-image/refs/heads/main/googlemap_random.png)

## 🐛 踩坑記錄

- Google Maps 非公開 API 的回傳格式是奇怪的 JS 陣列，要注意前綴清除
- 有些資料沒有 `cid`，只能自己組合 `textsearch` 取得地圖連結
- 地圖連結用 `https://www.google.com/maps/place/?q=place_id:{place_id}` 是最穩定的

## 🤖 轉型為 Line Bot 的構想

我發現要部署前端頁面有資訊暴露風險，因此決定轉向用 Line Bot 來串接，當使用者輸入關鍵字時：

1. 取得用戶目前經緯度（或用戶分享位置）
2. 從清單找出三個最近地點
3. 隨機選一筆推薦並回傳（附上地圖連結）

未來還能整合 Firebase 儲存我的清單、按讚次數、打卡次數等互動紀錄！並且透過 LINE FlexMessage 格式呈現

## ✅ 結語

這個 Side Project 不僅讓我玩了一把 Web Scraping，也練習了 Google Map API 的申請及使用。

下一步，會考慮將整後端搬進 NestJS 並正式部屬為 API，甚至變成可分享給朋友的小工具 🤩

🚀 GitHub Repo 👉 [詳細資訊請前往 github](https://github.com/yu-sooong/google_list_scraper)

> 如果你也對這個專案有興趣，歡迎留言、發 PR 或討論更多應用可能！

