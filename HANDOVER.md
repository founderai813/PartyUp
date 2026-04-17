# PartyUp 專案交接文件

## 目前完成的事情

### 已上線（已 merge 到 main，GitHub Pages 部署中）

| 頁面 | 網址 | 功能 |
|---|---|---|
| 首頁 | `futurestarai.com` | JoinUp + PartyUp 兩個入口 |
| PartyUp hub | `futurestarai.com/partyup/` | 工具集線頁（6 張卡片）|
| 遠端猜拳 | `futurestarai.com/rps/` | PeerJS P2P 對戰、計分、再戰 |
| 抽籤 | `futurestarai.com/draw/` | 3 個分頁：一般 / 配對 / 遠端同步 |

### 抽籤頁完成的功能

- 多名單管理（儲存/切換/重命名/刪除）
- 加權語法（`王小明 x3`）
- 抽 N 個不重複 / 可重複 / 分 N 組
- 抽過排除歷史紀錄
- 配對抽籤（一對一 / A-gets-B / 秘密聖誕老人）
- 遠端同步抽籤（PeerJS host → 多觀眾同步看）
- QR 分享結果
- spinReveal 動畫

### 品牌建立

- PartyUp 子品牌命名 + 漸層 LOGO
- 所有頁面 title / header / footer 統一品牌
- 連結指向 `partyup.futurestarai.com`（子網域尚未設定）

## 接下來要做的事

### 1. 合併成一頁（你剛要求的，正在做）

把猜拳、抽籤、配對、遠端同步全部放在 `/partyup/index.html` 一頁裡，用分頁切換，不用跳來跳去。

**狀態**：已建好新分支 `claude/all-in-one-partyup`，尚未開始寫。

### 2. PartyUp 子網域（可選，之後再弄）

- 建 GitHub repo `PartyUp`（已建，但還是空的）
- 推送 hub 頁面 + CNAME
- Cloudflare DNS 加 CNAME 記錄
- 你說看不懂，這部分先跳過也不影響功能

### 3. 未來可擴充的功能（做完合一頁後再問你）

- 即時投票（VoteUp）
- 倒數計時器
- 音效 / confetti 慶祝動畫
- 暗號模式（秘密聖誕老人每人只看自己的配對）
- 匯出 CSV / 圖片
