# PartyUp 專案交接文件

## 專案概覽

- **母品牌**：FuturestarAI (futurestarai.com)
- **子品牌**：PartyUp — 聚會互動工具集
- **預計網址**：https://partyup.futurestarai.com
- **Repo**：founderai813/futurestarai.com
- **分支**：`claude/remote-rps-game-VYYu2`（尚未 merge 到 main）

## 品牌階層

```
FuturestarAI (母品牌)
├── JoinUp → joinup.futurestarai.com（揪團，活動前）
├── PartyUp → partyup.futurestarai.com（互動工具，活動中）
│   ├── 遠端猜拳 /rps/
│   ├── 抽籤 /draw/（含一般/配對/遠端同步 3 分頁）
│   └── 未來：投票、倒數計時...
└── LINE 摘要器 /line_summarizer.html
```

## 檔案結構 & 行數

| 路徑 | 行數 | 說明 |
|---|---|---|
| `index.html` | 116 | 首頁，有 JoinUp + PartyUp 兩入口 |
| `rps/index.html` | 751 | 遠端猜拳（PeerJS P2P）|
| `draw/index.html` | 1748 | 抽籤主頁（3 分頁 + 全部功能）|
| `partyup/index.html` | 259 | PartyUp 集線 hub 頁（未來搬到獨立 repo）|
| `joinup/index.html` | 12 | 轉址到 joinup 子網域 |
| `CNAME` | 1 | futurestarai.com |
| `line_summarizer.html` | 762 | 既有 LINE 摘要器（本次未動）|

## 已完成功能清單

### `/rps/` 遠端猜拳

- PeerJS WebRTC P2P 連線
- 房號產生 + URL hash 分享（`#rps-xxx-yyyy`）
- 出招同步揭曉（shake 動畫）
- 計分、再戰、重設比分、離開

### `/draw/` 抽籤（3 分頁）

**一般抽籤 tab**

- 多名單管理（儲存/切換/重命名/刪除，localStorage）
- 加權語法：`王小明 x3` → 3 倍中獎率
- 3 模式：不重複 / 可重複 / 分組
- 抽過排除歷史（toggle + chip 顯示 + 單獨移除）
- spinReveal 動畫（快速滾動 → 逐一揭曉）
- 複製結果 + QR 分享

**配對抽籤 tab**

- A/B 兩組名單輸入
- 3 模式：一對一 / A-gets-B（可重複）/ 秘密聖誕老人（derangement）
- 結果列表動畫 + 複製 + QR 分享

**遠端同步 tab**

- PeerJS host → 多 viewer 架構
- 房號 + 分享連結（`#remote=draw-xxx-yyyy`）
- Host 抽籤 → viewer 同步看到 spinReveal 動畫
- URL hash 自動加入（`#remote=code` 自動切到遠端 tab 並連線）

### `/partyup/` hub 集線頁

- 大 LOGO（PartyUp + 漸層 + 圓點）
- 6 張工具卡（3 已完成 + 3 開發中預告）
- 連結用絕對 URL 指向 `futurestarai.com/rps/` 等

## 外部依賴（CDN）

| 套件 | 版本 | 用途 | CDN |
|---|---|---|---|
| PeerJS | 1.5.4 | WebRTC P2P | `unpkg.com/peerjs@1.5.4/dist/peerjs.min.js` |
| qrcode | 1.5.3 | QR Code 產生 | `cdn.jsdelivr.net/npm/qrcode@1.5.3/build/qrcode.min.js` |

## localStorage Keys

| Key | 用途 |
|---|---|
| `futurestarai.draw.lists.v2` | 多名單陣列 `[{id, name, content}]` |
| `futurestarai.draw.current.v2` | 當前名單 ID |
| `futurestarai.draw.history.v1` | 已抽過的人名陣列 |
| `futurestarai.draw.exclude.v1` | 排除開關 `'1'/'0'` |
| `futurestarai.draw.pairA.v1` | 配對 A 組文字 |
| `futurestarai.draw.pairB.v1` | 配對 B 組文字 |
| `futurestarai.draw.v1` | 舊版單一名單（migrate 用）|

## Git 歷史（本分支）

```
1ff2442 Update all links to use partyup.futurestarai.com subdomain
bb9195c Update hub page and add hash-based tab switching
ffd364c Add QR share for draw and pair results
936afb1 Add remote sync draw via PeerJS to /draw/
92e7fbd Add top tabs and pairing draw mode to /draw/
e342d44 Add multi-list management and weighted draw to /draw/
34343df Introduce PartyUp sub-brand for interactive tools
ad27d4b Link Join Us button directly to joinup subdomain
e69a682 Add drawn-history exclusion for lottery page
659b75d Add name-drawing lottery page
8e34c27 Add remote rock-paper-scissors P2P game
```

## 待做事項

### 部署（必做）

1. merge PR 到 main → GitHub Pages 發佈 `futurestarai.com/rps/` 和 `/draw/`
2. 建立獨立 repo `partyup` → 放 hub 頁面 + CNAME `partyup.futurestarai.com`
3. DNS 設定 → 加 CNAME `partyup` → `founderai813.github.io`

### 功能擴展（可選）

- 即時投票（VoteUp）
- 倒數計時器
- 音效 / confetti 慶祝動畫
- 暗號模式（秘密聖誕老人每人只看自己的配對）
- 匯出 CSV / 圖片

## 設計系統

```css
--bg:       #0a0a0a      /* 深黑背景 */
--panel:    #141414      /* 卡片底 */
--border:   #2a2a2e      /* 邊框 */
--text:     #e8e8ea      /* 主文字 */
--muted:    #888         /* 次要文字 */
--accent-1: #00d2ff      /* 藍 */
--accent-2: #7b2ff7      /* 紫 */
--pink:     #ff5fa2      /* 粉（PartyUp hub 用）*/
```

所有頁面統一：深色底 + 藍紫漸層 + 圓角 20px 卡片 + `ui-monospace` 等寬字體
