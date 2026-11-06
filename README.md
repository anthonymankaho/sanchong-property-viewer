# 中山三鄉睇樓評分表 · Sanxiang Property Viewing Scorer

一個**純離線、單檔案**嘅手機網頁應用，用嚟幫香港買家喺中山三鄉睇樓時即場記錄同評分單位。

A **fully offline, single-file** mobile-first web app for Hong Kong buyers to record and score properties while viewing homes in Sanxiang, Zhongshan.

---

## ✨ 特點 / Features

| 中文 | English |
|---|---|
| 📱 手機優先，44px 觸控目標 | 📱 Mobile-first, 44px minimum tap targets |
| 📴 純離線，冇後端、冇 CDN、冇 npm | 📴 Fully offline — no backend, no CDN, no npm |
| 💾 資料自動存喺瀏覽器 `localStorage` | 💾 Auto-saves to browser `localStorage` |
| 📷 照片自動壓縮（長邊 800px、JPEG 0.6） | 📷 Auto photo compression (800px long edge, JPEG q0.6) |
| ⭐ 總體評分 1–10 ＋ 六項細分評分 | ⭐ Overall score 1–10 plus six sub-scores |
| 🧮 自動計算每 ㎡ 單價 | 🧮 Auto-calculates price per ㎡ |
| 📥📤 JSON 匯出／匯入備份 | 📥📤 JSON export / import backup |
| 🔒 唔會上傳任何資料到網絡 | 🔒 Never uploads any data anywhere |

---

## 🚀 快速開始 / Quick start

**中文**

1. 下載或者複製 `index.html` 到你嘅裝置。
2. 用手機瀏覽器直接打開（Android 用檔案管理員點擊；iPhone 傳入「檔案」App 後用 Safari 開啟）。
3. 撳「＋ 新增單位」開始記錄。

**English**

1. Download or copy `index.html` to your device.
2. Open it directly in a mobile browser (Android: tap it in Files; iPhone: send it to the Files app, then open in Safari).
3. Tap "＋ 新增單位" (Add unit) to start recording.

> 唔需要 `npm install`、唔需要 build、唔需要 server。
> No `npm install`, no build step, no server required.

### 三種打開方法 / Three ways to open

| 方法 / Method | 點做 / How |
|---|---|
| **A. 直接打開 / Direct** | 用檔案管理員點 `index.html` → 選瀏覽器 |
| **B. 傳到手機 / Transfer** | WhatsApp、Telegram、WeChat 檔案傳輸助手，或者 AirDrop |
| **C. 區域網絡 / LAN** | 喺資料夾執行 `python -m http.server 8000`，手機連同一 Wi-Fi 開 `http://<電腦IP>:8000/index.html`，再「加到主畫面」 |

---

## 📋 記錄欄位 / Data fields

**基本資料 / Basics**
樓盤名稱（必填）· 座數／單位 · 類型（一手／二手）· 建築面積 ㎡ · 房間 · 樓層／總樓層 · 裝修（毛坯／簡裝／精裝／豪華裝修）· 朝向

**報價 / Price**
代理報價（人民幣，step 10000）→ 自動計算每 ㎡ 單價

**評分 / Scoring**
- 總體評分 1–10（可點擊數字按鈕，徽章顏色分級：**≥7 綠 · 5–6 橙 · ≤4 紅**）
- 六項細分：位置交通 · 戶型格局 · 裝修狀況 · 小區環境 · 性價比 · 代理可信度

**其他 / Others**
備註（滲水、潮濕、噪音、採光、議價空間、代理說法）· 現場照片（最多 12 張，可逐張刪除）

---

## 🗄 資料模型 / Data model

`localStorage` key: **`sz_sq_units`**

```jsonc
{
  "estate":  "雅居樂悅景園",   // string  — 樓盤名稱（必填）
  "unit":    "3座 1205室",     // string
  "type":    "二手",           // "一手" | "二手"
  "area":    89.5,             // number  — 建築面積 ㎡
  "rooms":   "2房2衛",         // string
  "floor":   "12/26",          // string
  "decor":   "精裝",           // 毛坯 | 簡裝 | 精裝 | 豪華裝修
  "facing":  "南",             // string
  "price":   480000,           // number  — 人民幣
  "overall": 7,                // number  1–10
  "scores": {                  // 各 1–10（0 = 未填）
    "loc": 6, "layout": 7, "decor": 6,
    "env": 8, "value": 6, "agent": 6
  },
  "note":    "輕微滲水，可議價 3 萬",  // string
  "photos":  ["data:image/jpeg;base64,..."],  // string[]，最多 12 張
  "time":    "2026-11-06T10:30:00.000Z"       // ISO 8601
}
```

匯出檔名格式 / Export filename format: `sanchong_YYYY-MM-DD.json`

---

## ⚠️ 重要提示 / Important notes

**中文**

- `localStorage` 以「瀏覽器 + 來源」為單位。用唔同瀏覽器（Chrome vs Safari）或者唔同方式（`file://` vs `http://`）打開，資料係**分開儲存、唔會互通**。請固定用同一個方式打開。
- 清除瀏覽器資料、使用無痕模式，都會令記錄消失 — **記得定期按「📥 匯出備份」**。
- 每張壓縮後嘅相片約佔 30–80KB。瀏覽器 `localStorage` 一般上限約 5MB，即大約可存 60–150 張相。若彈出「儲存失敗」提示，請先匯出備份再刪相。
- 「🗑 清除全部」需要**兩次確認**，防止誤刪。

**English**

- `localStorage` is scoped per browser + origin. Opening the file in a different browser (Chrome vs Safari) or a different way (`file://` vs `http://`) gives you a **separate, non-shared** datastore. Stick to one method.
- Clearing browser data or using private/incognito mode will erase your records — **export a backup regularly**.
- Each compressed photo takes roughly 30–80KB. Browser `localStorage` is typically capped near 5MB, so about 60–150 photos. If you see a "storage failed" alert, export a backup first, then delete photos.
- "Clear all" requires **two confirmations** to prevent accidental wipes.

---

## 🔧 技術細節 / Technical notes

- 單一 `index.html`，內含全部 HTML + CSS + JS，零外部依賴 / Single self-contained file, zero external dependencies
- JS 包喺 IIFE 入面，唔污染全域（只暴露必要嘅 `window.*` handler）/ JS wrapped in an IIFE; only the required handlers touch `window`
- 所有 `localStorage` 讀寫都包 `try-catch`，處理 quota exceeded / All storage access is `try-catch`-wrapped for quota errors
- 相片壓縮用 `FileReader` + `Image.onload` + `<canvas>`，**等全部完成先 render**，避免中途渲染 / Compression waits for every file before rendering
- 列表卡片同相片預覽都用**事件委派**，避免 re-render 之後索引錯亂或者 listener 洩漏 / List cards and photo previews use event delegation to avoid stale indices and listener leaks
- 匯入後會清空 file input 嘅 `value`，令同一個檔案可以再揀一次 / Import clears the file input so the same file can be re-selected
- 所有用戶輸入經 `escapeHtml()` 處理先插入 DOM / All user input is escaped before insertion

---

## 📄 授權 / License

[MIT](LICENSE) © 2026 Anthony Man
