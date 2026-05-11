# PartyUp 開發進度交接

> 詳細的專案介紹見 [README.md](README.md)，技術能力盤點見 [SKILLS.md](SKILLS.md)。本檔案只記**還沒做、可以做**的事。

---

## ✅ 已完成（已部署）

| Tab | 狀態 | 重點 |
|---|---|---|
| 🏠 首頁 | ✅ | 9 張卡片導覽 |
| ✊ 猜拳 | ✅ | P2P 對戰、計分、自訂房號 |
| 🎲 抽籤 | ✅ | 多名單、加權（±按鈕）、3 模式、排除歷史 |
| 💘 配對 | ✅ | 一對一 / A 配 B / 秘密聖誕老人 |
| 📡 遠端同步 | ✅ | 主持人抽、多觀眾同步看 |
| 🗳️ 投票 | ✅ | 即時長條圖、主持人關閉投票 |
| ⏱️ 計時 | ✅ | 預設值、紅色脈動、嗶嗶聲 |
| 🔊 音效 | ✅ | 12 個旋律類音效（單音效獨佔） |
| 🕵️ 臥底 | ✅ | 誰是臥底 / 自訂角色（狼人殺通用） |
| 💞 默契 | ✅ | 兩人同題、揭曉、累計分數 |

---

## ⚠️ 部署狀態

| 項目 | 狀態 |
|---|---|
| GitHub Pages | ✅ 已啟用（從 claude 分支發佈） |
| 自訂網域 `partyup.futurestarai.com` | 🟡 等 DNS 設定 |
| GitHub 直接網址 `founderai813.github.io/partyup` | ✅ 可用 |

### DNS 還沒完成的步驟
1. Cloudflare → DNS → Add record：CNAME `partyup` → `founderai813.github.io`（灰色雲）
2. 等 5–30 分鐘 DNS 傳播
3. GitHub Pages 設定頁 → Enforce HTTPS 變可勾 → 勾

---

## 🛠 未來可做（待規劃）

### 工具擴充
- [ ] **開團上傳照片**（最近被打斷的需求）：開房間時可上傳 3 張圖片給觀眾看
- [ ] **遠端同步抽籤加上加權**：目前遠端 tab 抽籤是平均機率
- [ ] **PWA**：加入主畫面、離線使用
- [ ] **匯出 CSV / 截圖**：投票結果、抽籤結果

### 新工具
- [ ] **你畫我猜**：畫布 P2P 同步
- [ ] **OX / 四子棋**：兩人棋盤
- [ ] **Bingo 賓果**：自訂卡片 + 答題
- [ ] **快速搶答按鈕**：誰先按誰回答

### 音效
- [ ] 嵌入 base64 音檔，補上純合成做不像的（真鼓 / 真掌聲 / 真警報）

### UX
- [ ] 淺色 / 深色模式切換
- [ ] tab 拆「遊戲類」vs「工具類」兩組
- [ ] 手機 bottom nav

---

## 📂 重要檔案

- `index.html` — 整個 app（~3400 行）
- `CNAME` — `partyup.futurestarai.com`
- `README.md` — 專案首頁（GitHub 顯示）
- `SKILLS.md` — 技能盤點
- 本檔（HANDOVER.md）— 你正在看

---

## 🔑 LocalStorage Keys

```
partyup.draw.lists         # 多名單陣列
partyup.draw.current       # 當前名單 ID
partyup.draw.history       # 已抽過的人名
partyup.draw.exclude       # 排除歷史開關
partyup.pair.a / b / people / mode   # 配對輸入
```

---

## 🎨 PeerJS ID 命名空間

| 工具 | Prefix |
|---|---|
| 猜拳 | `pu-rps-` |
| 遠端抽籤 | `pu-rm-` |
| 投票 | `pu-vote-` |
| 臥底 | `pu-uc-` |
| 默契 | `pu-qz-` |

全部用 `djb2(房名)` 轉合法 ID，所以使用者可以打中文 / emoji。
