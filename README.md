# AnnoyingCats 🐱

一款用 Cocos Creator 做的直式網頁小遊戲：左右滑動擠成一排的貓咪方塊，讓牠們掉下來填滿整行消除，別讓貓堆到頂。

**▶ 線上試玩：<https://agan0617.github.io/AnnoyingCats/>**（手機、電腦瀏覽器都能玩）

## 玩法

- 棋盤是 **8 欄 × 10 行**，貓咪方塊有 1×1、1×2、1×3、1×4 四種長度。
- **按住貓咪左右拖曳**，只能在自己那一行裡水平移動，路徑上被其他貓擋住就過不去。拖曳時原位會留下半透明的藍色殘影。
- 放手後所有貓受重力往下掉，**填滿一整行就消除**；消除後上面的貓繼續掉，可能再觸發連鎖消除。
- **每一輪只能移動一次**。移動結算完，整個棋盤往上推，底部冒出新的一排或好幾排貓。
- 新冒出的每一排有 2～4 隻貓，而且一定留有空隙，不會生成直接就滿的行。
- 盤面剩不到 3 行時，會自動補貓補到至少 3 行。
- **Game Over**：棋盤往上推時頂端已經有貓，推不上去就結束。

## 難度與計分

**難度（Level）**：每 30 秒升一級，最高 Level 9。

| 等級 | 每輪從底部加幾排 |
|---|---|
| Level 1 | 盤面不到 5 行時補到 5 行，已經 5 行以上就加 1 排 |
| Level 2～9 | 加「等級數」排，但不會超過 10 行的棋盤高度 |

**分數**：每次移動結算時計算，最後乘上當下等級（×1～×9）。

| 項目 | 分數 |
|---|---|
| 連鎖消除 | 第 1 行 1 分、第 2 行 2 分、第 3 行 4 分……每多一行翻倍 |
| 結算後盤面清空 | +50 |
| 結算後只剩 1 行 | +20 |
| 結算後只剩 2 行 | +10 |

## 介面

- 畫面上方顯示 **Score／Time／Level／Remaining Moves／Name**。
- **Change Name**：設定玩家名稱，排行榜上會用這個名字。
- **Show Ranking**：開關排行榜，開著的時候計時會暫停。
- **Restart**：重新開局。
- 排行榜保留前 10 名，存在瀏覽器的 `localStorage`（`haru_leaderboard`、`haru_player_name`）。**只存在本機**，換瀏覽器或清除網站資料就會不見。

## 關於這個 repo

這裡放的是 **Cocos Creator 3.x 的 Web Mobile 建置產物**，直接由 GitHub Pages（`main` 分支根目錄）發佈，沒有附原始專案。

```
index.html          入口頁
index.js            啟動 Cocos 引擎
application.js      引擎初始化
cocos-js/           Cocos 引擎執行檔（含 Spine 模組）
src/                SystemJS 載入器、polyfill、專案設定
assets/main/        遊戲腳本、場景、圖片與音效
assets/internal/    引擎內建資源
.nojekyll           讓 GitHub Pages 不經 Jekyll 處理，保留底線開頭的檔名
```

遊戲邏輯編譯在 `assets/main/index.js` 裡，由五個腳本組成：

| 腳本 | 負責 |
|---|---|
| `GameController` | 輪次流程、計時與難度、計分、排行榜、音效 |
| `GridManager` | 8×10 棋盤資料、生成新排、下落、消行、連鎖計分 |
| `CatBlock` | 單隻貓的拖曳、碰撞判定、下落動畫 |
| `InputNamePanel` | 輸入玩家名稱的面板 |
| `LeaderboardDisplay` | 排行榜文字顯示 |

### 本機執行

要透過 HTTP 伺服器開，直接點兩下 `index.html` 會因為瀏覽器的 CORS 限制載不起來：

```bash
npx serve .
```

或

```bash
python -m http.server 8000
```

然後開 <http://localhost:8000>（`serve` 的預設埠是 3000）。

### 更新版本

在 Cocos Creator 裡用 **Web Mobile** 平台建置，把輸出資料夾的內容覆蓋到這個 repo 根目錄，記得保留 `.nojekyll` 跟這份 README，push 到 `main` 後 GitHub Pages 會自動更新。
