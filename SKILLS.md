# PartyUp — 技能總結

> 本檔案整理建造 PartyUp 過程中**實際用到**的技術技能，可作為履歷 / 作品集 / 面試說明用。

---

## 🎯 專案概覽（一句話）

> 一個 ~3400 行的**單檔 HTML SPA**，整合 10 個聚會互動工具，透過 **WebRTC P2P** 連線、**Web Audio** 即時合成、**LocalStorage** 持久化，全靜態部署於 GitHub Pages。

---

## 1. 前端核心技術

### HTML5 / CSS3
- **Semantic HTML**：`<header>`、`<nav>`、`<main>`、`<section>`、`<footer>`
- **CSS Variables（Custom Properties）**：建立設計系統 token，一個檔案改色全站變
- **Flexbox + Grid**：響應式布局，9 張卡片自動排版、tab 列水平滑動
- **CSS Animations / Keyframes**：popIn 彈跳、shake 抖動、脈動、漸層流動、淡入淡出
- **CSS Mask Image**：tab 列邊緣淡出漸層
- **Sticky positioning**：頂部 tab 列固定
- **Backdrop-filter**：毛玻璃模糊效果
- **Media queries**：手機 / 桌機分流（820px 斷點）

### Vanilla JavaScript（ES6+）
- **無框架、無 build step**：純 ES6+，立即執行
- **Module pattern**：每個工具一個 namespace 物件（`RPS`, `DRAW`, `PAIR`, `REMOTE`, `VOTE`, `TIMER`, `UC`, `QZ`）
- **Closure + 私有狀態**：每個模組私有變數不外洩
- **Async/await**：AudioContext unlock、PeerJS 連線建立
- **Promise wrapping**：把 callback-style PeerJS 包成 promise
- **Event-driven**：addEventListener、自訂事件、生命週期管理

---

## 2. Web APIs（瀏覽器原生能力）

### WebRTC（透過 PeerJS）
- **P2P 連線建立**：訊號伺服器交換 SDP，建立直接資料通道
- **兩種拓樸**：
  - **1-to-1**：猜拳、默契問答（單一對等連線）
  - **Host / Multi-viewer**：投票、臥底、遠端同步抽籤（host 維護多個連線）
- **連線生命週期**：open / data / close / error 事件處理
- **斷線重連 / 錯誤回復**：清理孤兒連線、處理 `unavailable-id`
- **房號設計**：使用者輸入任意字串（中文 / emoji） → djb2 hash → 合法 PeerJS ID

### Web Audio API
- **OscillatorNode**：sine / triangle / sawtooth / square 波形
- **GainNode + ADSR 包絡**：attack / decay / sustain / release 自然音色
- **BiquadFilterNode**：lowpass / highpass / bandpass / 共振峰模擬
- **BufferSource + AudioBuffer**：白噪音生成、Click transient
- **LFO 調變**：振盪器 → gain / frequency 形成 vibrato / siren
- **音樂理論編碼**：MIDI note → frequency 表、tempo / beat 轉秒、melody 序列
- **iOS Safari 解鎖**：用戶手勢中播放靜音 buffer，async await resume()
- **音色設計**：
  - 不諧波分音模擬教堂鐘聲（1 / 2.76 / 5.4 倍頻）
  - 低通濾波器掃頻模擬銅管 brass attack
  - 共振峰 bandpass 模擬人聲母音
  - 加法合成（多 oscillator 疊加）做柔和音色

### LocalStorage
- **多名單管理**：JSON 序列化 / 反序列化
- **狀態持久化**：使用者偏好、最近編輯內容
- **資料 migration**：自動補欄位（例如新增 `weights` 物件）

### 其他
- **Clipboard API + execCommand fallback**：跨瀏覽器複製
- **History API**：`history.replaceState` 同步 URL hash
- **URL Hash routing**：`#rps=房名` 自動跳 tab + 預填表單
- **encodeURIComponent**：中文 / emoji 安全傳遞

---

## 3. 演算法 / 邏輯

| 演算法 | 用在哪 |
|---|---|
| **Fisher-Yates shuffle** | 抽籤、配對 |
| **Weighted random sampling** | 加權抽籤（依權重抽 N 個不重複） |
| **Derangement（錯排）** | 秘密聖誕老人 — 確保沒人抽到自己 |
| **djb2 string hash** | 房號 → PeerJS ID |
| **Reservoir / 隨機分組** | 抽籤分組模式（平均分配） |
| **debounce** | 默契問答的題目同步輸入 |
| **ADSR envelope shaping** | 音效合成包絡 |
| **MIDI → frequency** | `440 * 2^((midi-69)/12)` 標準等律平均律 |

---

## 4. 系統設計 / 架構決策

### 單檔 SPA 的取捨
- ✅ **零 build step**：直接打開 `index.html` 就能跑、改完即時生效
- ✅ **單一 HTTP request**：載入快、CDN 友善
- ✅ **離線可用**：除了 PeerJS 訊號伺服器，主體不依賴後端
- ❌ **代價**：~3400 行單檔，可讀性需靠章節註解維護

### 為什麼選 WebRTC 而非 WebSocket
- 不用自己架後端
- P2P 延遲低
- 訊號伺服器（PeerCloud）免費、開源可自架

### 為什麼用 Web Audio 合成而非音檔
- 零下載成本
- 動態參數（音高、tempo 即時調）
- 但**有侷限**：寫實音效（掌聲、警報、人聲）做不像，最終決定砍掉只留旋律類

---

## 5. UX / 互動設計

- **手機優先**：所有按鈕 ≥44px 觸控區、無 hover-only 互動
- **Tab 水平滑動**：10 個 tab 在手機單行可滑、active tab 自動置中
- **URL 分享連結**：每個房間都有 `#kind=房名` 連結，掃 QR 自動填入
- **即時視覺回饋**：loading、connected、error 狀態以彩色徽章顯示
- **動畫敘事**：抽籤前 spin 跑名字 → 揭曉時 popIn 彈跳，符合「期待→驚喜」節奏
- **降級處理**：clipboard 失敗 fallback execCommand、PeerJS error 顯示中文錯誤
- **無障礙基礎**：語意化 HTML、鍵盤可操作、color contrast 達標

---

## 6. 部署 / 維運

- **GitHub Pages** 自動部署：push 到分支即發布
- **Custom Domain**：CNAME 檔 + DNS CNAME 紀錄 + GitHub Pages 自訂網域
- **HTTPS by GitHub**：自動簽 Let's Encrypt 憑證
- **版本控制**：原子化 commit、descriptive message、單一 feature branch
- **無 server cost**：純靜態 + P2P + PeerCloud 免費層 = $0 / 月

---

## 7. 軟性技能 / 流程

- **需求分解**：把模糊的「做一個聚會工具」拆成 10 個獨立可交付的 tab
- **迭代開發**：每個 tab 一個 commit、即時部署、立即測試回饋
- **使用者回饋驅動**：根據用戶實測一輪一輪修（例如音效從 17 個砍到 12 個只留好聽的）
- **技術決策說明**：寫得出「為什麼選 X 不選 Y」（例如為什麼砍寫實音效）

---

## 📊 量化指標

| 指標 | 數字 |
|---|---|
| 程式碼總行數（index.html） | ~3400 |
| 互動工具數 | 10 |
| 第三方依賴 | 2（peerjs、qrcode） |
| Build step | 0 |
| 後端伺服器 | 0 |
| 月維運成本 | $0 |
| 支援瀏覽器 | Chrome / Safari / Firefox / Edge（含手機） |

---

## 🏷️ 技能 Tag 雲（一行版）

```
HTML5 · CSS3 · JavaScript ES6+ · WebRTC · PeerJS · Web Audio API ·
LocalStorage · Clipboard API · Hash Routing · Responsive Design ·
SPA Architecture · P2P Networking · Audio Synthesis · ADSR ·
Weighted Random · Derangement · Fisher-Yates · MIDI · djb2 Hash ·
GitHub Pages · Custom DNS · CI-less Deploy · Mobile-First UX
```

---

**Made with 🤖 Claude Code + ❤️ by [FuturestarAI](https://futurestarai.com)**
