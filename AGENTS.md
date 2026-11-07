# AGENTS.md — 個人影評與劇評筆記網站維護規範

本文件為本專案的 **AI Agent 作業指引與系統架構標準作業程序（SOP）**。未來任何 AI Agent（或開發者）在協助新增影評、維護頁面或更新首頁時，**必須嚴格遵循本文件規範**，以確保全站視覺風格、程式碼品質與資料結構的一致性。

> [!IMPORTANT]
> **內部規範文件宣告（禁止出現在前端 UI）**：  
> 本文件 `AGENTS.md` 僅供 AI Agent 與開發者內部參考與遵循。**嚴禁在任何使用者可見的網頁前端（包含首頁導覽列、頁腳 Footer、影評詳情頁等）放置任何指向或提及 `AGENTS.md` 的超連結或文字。**

---

## 1. 專案概覽與目錄結構

### 1.1 專案目標
打造一個兼具**深色影院氛圍（Dark Cinema）**、**16:9 劇照視覺化**、**多維度評析與選片指南**，以及**作品專屬風格氛圍色**的個人電影／戲劇評論筆記網站（CineLog）。採用純靜態架構（Tailwind CSS CDN + 原生 JavaScript），確保零建置環境相依性，可隨時在瀏覽器中直接開啟或部署於 GitHub Pages / Vercel。

### 1.2 目錄結構
```text
HW9/
├── AGENTS.md                    # 本規範文件（AI Agent 遵守指南，前端不顯示）
├── index.html                   # 首頁目錄（數據儀表板、即時搜尋、分類篩選、16:9 劇照卡片清單）
├── reviews/                     # 獨立影評頁面專用資料夾
│   ├── movie-hope.html          # 電影評論範例（素媛）
│   ├── drama-breaking-bad.html  # 影集評論範例（絕命毒師）
│   ├── movie-toy-story-4.html   # 電影評論範例（玩具總動員4）
│   ├── movie-the-backrooms.html # 電影評論範例（後室）
│   ├── movie-the-truman-show.html # 電影評論範例（楚門的世界）
│   └── movie-dune-2.html        # 電影評論範例（沙丘：第二部）
└── assets/                      # 電影 16:9 劇照圖片存放處（直接存放於此，無子目錄）
    ├── movie-hope.jpg
    ├── drama-breaking-bad.jpg
    ├── movie-toy-story-4.jpg
    ├── movie-the-backrooms.jpg
    ├── movie-the-truman-show.jpg
    └── movie-dune-2.jpg
```

---

## 2. 技術規格與設計系統 (Design System)

### 2.1 技術棧
- **架構**：純靜態 HTML5 + 原生 JavaScript（無須 Node.js、npm 或建置步驟）。
- **全站分頁圖示 (Favicon)**：所有頁面必須在 `<head>` 引入電影拍板 SVG 圖示：
  ```html
  <link rel="icon" href="data:image/svg+xml,<svg xmlns=%22http://www.w3.org/2000/svg%22 viewBox=%220 0 100 100%22><text y=%22.9em%22 font-size=%2290%22>🎬</text></svg>">
  ```
- **樣式引擎**：Tailwind CSS (透過官方 CDN 引入)：
  ```html
  <script src="https://cdn.tailwindcss.com"></script>
  <script>
    tailwind.config = {
      darkMode: 'class',
      theme: {
        extend: {
          colors: {
            cinema: {
              black: '#0a0a0c',
              dark: '#121216',
              card: '#18181f',
              border: '#262630',
              accent: '#e50914',     // 影院紅重點色
              gold: '#f5c518',       // 評分暖金星級色
              textMuted: '#9e9ea7',
            }
          }
        }
      }
    }
  </script>
  ```

### 2.2 視覺規範（深色影院與 16:9 劇照風格）
| 元素類型 | Tailwind Class / 規範 | 說明 |
| :--- | :--- | :--- |
| **全站背景** | `bg-[#0a0a0c] text-neutral-100 min-h-screen` | 曜石黑、沉浸感極高的觀影氛圍 |
| **卡片背景** | `bg-[#18181f] border border-[#262630] rounded-2xl overflow-hidden` | 懸浮深灰微霧感卡片，包覆 16:9 劇照 |
| **劇照規格** | `aspect-video overflow-hidden object-cover` | 16:9 比例寬螢幕電影劇照，乾淨純粹，懸停微放大 |
| **星級評分色** | `text-[#f5c518]` (Amber/Gold) | 專用暖金黃，表現 5 星評分 |
| **標籤 Pills** | `px-2.5 py-1 text-xs rounded-full bg-neutral-800 text-neutral-300 border border-neutral-700` | 類型分類徽章 |
| **防雷警告色** | `bg-amber-950/40 border-amber-600/50 text-amber-200` | 警示讀者該區塊包含劇情雷 |

### 2.3 作品專屬風格氛圍色系統 (Atmospheric Theme System)
每篇新影評應根據電影的調性與主題，指定一組風格重點色（Theme Accent Color）：
- **沙漠/史詩冒險**（如《沙丘》）：**厄拉科斯琥珀金** (`amber-500` / `#f59e0b`)，卡片邊框 `hover:border-amber-500/50`，金句邊框 `border-amber-500`。
- **烏托邦/反思**（如《楚門的世界》）：**烏托邦晴空藍** (`sky-500` / `#0284c7`)。
- **孤寂/神秘/閾限空間**（如《後室》）：**閾限硫磺黃** (`yellow-500` / `#eab308`)。
- **治癒/冒險/童年抉擇**（如《玩具總動員4》）：**嘉年華幻紫** (`purple-500` / `#a855f7`)。
- **罪惡/蛻變/冷酷化學**（如《絕命毒師》）：**冰毒晶瑩翠綠** (`emerald-500` / `#10b981`)。

---

## 3. 檔案命名與圖片規範

### 3.1 影評檔案命名 (Path & Slug)
- 格式：`reviews/{type}-{slug}.html`
- `{type}`：`movie` 或 `drama`。
- `{slug}`：作品英文全小寫短名，如 `movie-the-truman-show.html`、`drama-breaking-bad.html`。

### 3.2 劇照圖片命名與路徑
所有電影劇照直接放置於 `assets/` 目錄下（**不設子目錄**）：
- 格式：`assets/{type}-{slug}.jpg` (或 `.png`)
- 範例：《沙丘：第二部》 $\rightarrow$ `assets/movie-dune-2.jpg`
- 範例：《絕命毒師》 $\rightarrow$ `assets/drama-breaking-bad.jpg`
- 首頁引用相對路徑：`assets/{type}-{slug}.jpg`
- 獨立頁面引用相對路徑：`../assets/{type}-{slug}.jpg`

### 3.3 評分標準（5 星制）
評分以 0.1 為精細度（如 4.8、4.5、4.2、4.0），並同時呈現**星星符號**與**數值**：
- 滿分神作：★★★★★ `4.8 ~ 5.0 / 5.0`
- 卓越必看：★★★★☆ `4.3 ~ 4.7 / 5.0`
- 優良佳作：★★★★☆ `3.8 ~ 4.2 / 5.0`

---

## 4. 核心頁面結構與精緻組件標準

### 4.1 獨立影評頁面精緻組件標準 (`reviews/xxx.html`)
所有新生成的影評頁面，必須具備以下精緻組件：
1. **16:9 劇照展示**（純粹畫面，不疊加突兀的風格文字）。
2. **作品標頭元資料**：類型 Pills、發布/完結日期、大標題、副標題、5 星評分徽章、**⏱️ 閱讀時長提示**。
3. **作品檔案規格面板 (Film Facts Grid)**：四欄式小面板，展示上映年份/片長、導演/主創、領銜主演、推薦觀影情境與音響規格。
4. **一句話短評 / 金句摘錄**：套用專屬風格重點色 `border-l-4`，右側附帶 **📋 一鍵複製金句** 按鈕與原生 Clipboard API 回饋。
5. **多維度深度評析 (Dimension Rating Bars)**：以 4 項指標評分進度條展示細膩維度（如劇本、演技、視聽、格局）。
6. **觀眾選片決策指引 (Audience Decision Guide)**：
   - 綠框：✓ 這部作品極度適合你，如果...
   - 琥珀框：✕ 觀看前請做好心理準備，如果...
7. **無雷簡介 (Spoiler-Free)**：清晰的故事介紹與觀影指引。
8. **有雷心得分析 (Spoiler Section)**：**原生手風琴折疊 `<details>`，嚴禁預設 `open` 屬性**，確保未觀影讀者絕不誤觸劇透。
9. **篇章導覽 (Prev / Next Review Navigation)**：底部雙欄導覽卡片，支援上一篇、下一篇影評或返回首頁目錄。
10. **頁腳宣告**：純淨版權宣告，**絕無 `AGENTS.md` 連結**。

### 4.2 首頁精緻組件標準 (`index.html`)
首頁具備現代影評誌必備的豐富功能：
1. **個人觀影數據儀表板 (Stats Bar)**：典藏作品數、綜合均分、電影/影集比例、涵蓋類型。
2. **即打即篩搜尋欄 (Search Bar)**：透過原生 JavaScript 監聽 `input`，即時過濾片名、導演、主演與標籤。
3. **分類切換標籤 (Filter Tabs)**：全部、🎬 電影、📺 影集、⭐ 高分精選。
4. **16:9 卡片網格與 Data 屬性**：每張卡片附帶 `data-type`、`data-rating`、`data-keywords`，以支援即時篩選。
5. **空狀態提示 (Empty State)**：搜尋無果時優雅呈現提示圖示與文字。

```html
<!-- START: Review Card for {作品名稱} -->
<article data-type="{type}" data-rating="{rating}" data-keywords="{片名} {英文片名} {導演} {主演} {標籤}" class="bg-[#18181f] border border-[#262630] rounded-2xl overflow-hidden flex flex-col justify-between hover:border-{themeColor}/50 hover:shadow-2xl hover:shadow-{themeColor}/20 transition duration-300 group">
  <!-- 16:9 劇照封面 (乾淨畫面，無文字貼紙覆蓋) -->
  <a href="reviews/{type}-{slug}.html" class="block aspect-video overflow-hidden relative">
    <img src="assets/{type}-{slug}.jpg" alt="{作品名稱}劇照" class="w-full h-full object-cover group-hover:scale-105 transition duration-500">
    <div class="absolute inset-0 bg-gradient-to-t from-[#18181f] via-transparent to-transparent"></div>
  </a>

  <div class="p-6 pt-3 space-y-3 flex-grow flex flex-col justify-between">
    <div class="space-y-3">
      <!-- 標籤、時長與發布日期 -->
      <div class="flex items-center justify-between">
        <div class="flex items-center gap-1.5 flex-wrap">
          <span class="px-2 py-0.5 text-xs font-semibold rounded-full bg-red-950/80 text-red-300 border border-red-800">電影</span>
          <span class="px-2 py-0.5 text-xs rounded-full bg-neutral-800 text-neutral-400">科幻</span>
        </div>
        <span class="text-xs text-neutral-500 font-mono">約 4 分鐘讀完</span>
      </div>

      <!-- 標題與原名 -->
      <div>
        <h3 class="text-xl font-bold text-white group-hover:text-{themeColor} transition">
          <a href="reviews/{type}-{slug}.html">{作品中文名稱}</a>
        </h3>
        <p class="text-xs text-neutral-400 mt-0.5">English Title (2024)</p>
      </div>

      <!-- 評分 -->
      <div class="flex items-center gap-2">
        <span class="text-sm text-[#f5c518]">★★★★☆</span>
        <span class="text-xs font-bold text-neutral-200">4.5 <span class="text-neutral-500 font-normal">/ 5.0</span></span>
      </div>

      <!-- 無雷一句話摘要 -->
      <p class="text-sm text-neutral-400 line-clamp-3 leading-relaxed">
        無雷摘要說明...
      </p>
    </div>

    <!-- 前往閱讀按鈕 -->
    <div class="pt-5 mt-4 border-t border-[#262630] flex items-center justify-between">
      <span class="text-xs text-neutral-500">含防雷深入解析</span>
      <a href="reviews/{type}-{slug}.html" class="inline-flex items-center text-xs font-bold text-{themeColor} hover:underline transition gap-1">
        閱讀全文 →
      </a>
    </div>
  </div>
</article>
<!-- END: Review Card for {作品名稱} -->
```

---

## 5. AI Agent 標準作業程序 (SOP)

當使用者發出新增影評請求時，AI Agent 必須按照以下 **4 步驟 SOP** 嚴格執行：

```
[使用者請求] 
    ↓
【步驟 1：萃取元資料與準備 16:9 劇照】(片名 / 風格色彩 / 下載或確認 assets/{type}-{slug}.jpg)
    ↓
【步驟 2：建立獨立影評頁】(寫入 reviews/{type}-{slug}.html，含規格表、4維評分、選片指南、複製金句、防雷折疊)
    ↓
【步驟 3：更新首頁目錄】(將 16:9 卡片填入 data 屬性並插入 index.html #reviews-grid 頂部，更新儀表板數字)
    ↓
【步驟 4：自檢與驗證】(檢查圖片路徑、Favicon、返回連結、防雷折疊閉合、絕無 AGENTS.md 前端連結)
```

### 步驟 4：自檢與驗證清單 (Verification Checklist)
- [ ] 圖片存放在 `assets/{type}-{slug}.jpg`，路徑正確（首頁無 `../`，內頁有 `../`）。
- [ ] 所有頁面的 `<head>` 均包含電影拍板 Favicon。
- [ ] 卡片排版符合 16:9 `aspect-video` 規格，圖片乾淨無覆蓋文字。
- [ ] 有雷區塊正常閉合（無 `open` 屬性）。
- [ ] 首頁與內頁的導航與頁腳**絕無任何 `AGENTS.md` 連結**。
- [ ] 複製金句按鈕與原生剪貼簿腳本正常無誤。
