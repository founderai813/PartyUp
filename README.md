# PartyUp

> 聚會互動工具集 — 一頁式 Web App，零安裝、零後端、全裝置可用

🔗 **線上版**：[partyup.futurestarai.com](https://partyup.futurestarai.com)
🏷️ **母品牌**：[FuturestarAI](https://futurestarai.com)

---

## 🎯 這是什麼

把朋友聚會、公司尾牙、課堂活動會用到的**互動小工具**全部塞進**一頁 HTML**：

- 不用下載 App、不用註冊帳號、不用後端伺服器
- 所有資料只存在你瀏覽器的 localStorage
- 多人互動全部走 **WebRTC P2P**（裝置直接對連，伺服器只幫忙建立連線）
- 響應式設計，手機 / 平板 / 電腦都能用

---

## 🧰 10 個工具

| Tab | 功能 | 多人？ |
|---|---|---|
| 🏠 **首頁** | 工具集線頁，9 張卡片快速導覽 | — |
| ✊ **猜拳** | 遠端 P2P 對戰、計分、再戰 | 2 人 |
| 🎲 **抽籤** | 多名單管理、加權抽籤、不重複/可重複/分組、排除歷史紀錄 | 單機 |
| 💘 **配對** | 一對一 / A 配 B / 秘密聖誕老人（derangement 演算法） | 單機 |
| 📡 **遠端同步** | 主持人抽籤、多位觀眾同步看到動畫 | 多人 |
| 🗳️ **投票** | 主持人出題、觀眾即時投票、長條圖統計 | 多人 |
| ⏱️ **計時** | 倒數計時器，到時嗶嗶聲，適合投影 | 單機 |
| 🔊 **音效** | 12 個 Web Audio 合成音效 / 短旋律 | 單機 |
| 🕵️ **臥底** | 角色卡分發（誰是臥底 / 狼人殺通用） | 多人 |
| 💞 **默契** | 兩人同題回答、揭曉合不合、累計分數 | 2 人 |

**全部房號自訂**：可用中文、emoji、空格、任何字元（內部 hash 成合法 PeerJS ID）

---

## 🛠️ 技術棧

**前端**
- 純 **HTML5 / CSS3 / Vanilla JavaScript（ES6+）**
- 無框架、無 build step、單檔 `index.html`（~3400 行）

**Web APIs**
- **WebRTC**（透過 PeerJS）— P2P 即時資料通道
- **Web Audio API** — 即時音效合成（震盪器 / 包絡 / 濾波 / LFO / 噪音 buffer）
- **LocalStorage** — 多名單、設定持久化
- **Clipboard API** — 一鍵複製
- **Hash routing** — `#rps=房名`、`#draw`、`#vote=` 等

**第三方**
- `peerjs@1.5.4` — WebRTC 訊號 + 連線抽象
- `qrcode@1.5.3` — QR Code 產生

**部署**
- **GitHub Pages** + 自訂網域 (`partyup.futurestarai.com`)
- 純靜態，CDN 全球分發

---

## 🚀 本地執行

不需要任何工具，直接打開 `index.html`：

```bash
git clone https://github.com/founderai813/partyup
cd partyup
open index.html        # macOS
# 或
python3 -m http.server 8000   # 然後打開 http://localhost:8000
```

> ⚠️ 因為要用 PeerJS WebRTC，**file://** 協定的 clipboard 和某些功能可能受限。建議用 `python3 -m http.server` 起一個本機 HTTP server。

---

## 📂 檔案結構

```
.
├── index.html       # 整個 app（HTML + CSS + JS 一檔搞定）
├── CNAME            # GitHub Pages 自訂網域
├── README.md        # 你正在看的這個
├── SKILLS.md        # 技術技能總結
└── HANDOVER.md      # 開發進度交接
```

---

## 🎨 設計系統

```css
--bg:       #0a0a0a   /* 深黑背景 */
--panel:    #141414   /* 卡片底 */
--accent-1: #00d2ff   /* 藍 */
--accent-2: #7b2ff7   /* 紫 */
--pink:     #ff5fa2   /* 粉 */
```

統一風格：深色底 + 藍紫漸層 + 圓角 20px + 等寬字體 + 大量動畫過場

---

## 📈 未來規劃

- [ ] Sample-based 音效（嵌入 base64 音檔取代純合成）
- [ ] 即時投票結果匯出 CSV
- [ ] 抽籤遠端同步加上加權設定
- [ ] PWA 支援（可加入主畫面 + 離線使用）
- [ ] 你畫我猜（畫布 P2P 同步）
- [ ] OX / 四子棋（兩人棋盤對戰）

---

## 📄 授權

MIT License © FuturestarAI

---

**Made with 🤖 Claude Code + ❤️ by [FuturestarAI](https://futurestarai.com)**
