# 🎬 CineLog — 個人影評與劇評筆記網站

> **沉浸式深色影院模式 · 16:9 高清劇照 · 多維度評析 · 觀眾選片指南 · 防雷手風琴折疊**

![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=flat-square&logo=html5&logoColor=white)
![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-38B2AC?style=flat-square&logo=tailwind-css&logoColor=white)
![JavaScript](https://img.shields.io/badge/Vanilla_JS-F7DF1E?style=flat-square&logo=javascript&logoColor=black)
![License](https://img.shields.io/badge/License-MIT-blue?style=flat-square)

CineLog 是一個專為電影與影集愛好者打造的精緻個人筆記網站。採用**純靜態架構（Pure Static HTML + Tailwind CSS CDN + 原生 JavaScript）**，無需安裝 Node.js、npm 或任何打包工具，開箱即用，可一鍵部署至 **GitHub Pages** 或 **Vercel**。

---

## ✨ 網站核心特色

- 🍿 **深色影院氛圍 (Dark Cinema Design)**：採用曜石黑 (`#0a0a0c`) 與極致微霧深灰卡片，搭配專屬星級暖金與氛圍色，營造置身專業放映廳的沉浸感。
- 🖼️ **16:9 劇照視覺化**：所有作品均配備純粹無文字遮蔽的 16:9 高畫質劇照，支援懸停微放大動效。
- 📊 **個人觀影數據儀表板 (Stats Bar)**：即時統計典藏總數、綜合平均評分、電影/影集比例與涵蓋題材類型。
- 🔍 **即打即篩即時搜尋與分類標籤**：
  - 支援片名（中英文）、導演、領銜主演及題材標籤即時模糊比對。
  - 分類標籤快速切換（全部、🎬 電影、📺 影集、⭐ 高分精選）。
- 📝 **獨立影評頁面精緻規格**：
  - **⏱️ 閱讀時長預估**
  - **四欄作品檔案規格表 (Film Facts Grid)**（上映年份/片長、主創導演、主演陣容、推薦觀影設備/情境）
  - **📋 一鍵複製金句**（調用原生 Clipboard API）
  - **多維度深度評析進度條 (Dimension Rating Bars)**（劇本、演技、視聽、格局等量化分析）
  - **觀眾選片決策指引 (Audience Decision Guide)**（適合誰看 vs 觀看前心理準備）
  - **⚠️ 防雷折疊手風琴**（原生 `<details>` 標籤，預設完全閉合防劇透）
  - **篇章平滑導航**（上一篇 / 下一篇雙向連貫閱讀卡片）

---

## 📂 典藏作品清單

| 作品名稱 | 類型 | 評分 | 導演 / 主創 | 風格重點色 |
| :--- | :--- | :--- | :--- | :--- |
| **《素媛》** (Hope / 소원) | 電影 · 劇情 / 家庭 | ★★★★☆ **4.3** | 李濬益 | 溫暖曙光粉 (`rose-500`) |
| **《絕命毒師》** (Breaking Bad) | 影集 · 犯罪 / 驚悚 | ★★★★★ **4.8** | 文斯·吉利根 | 冰毒翠綠 (`emerald-500`) |
| **《玩具總動員4》** (Toy Story 4) | 電影 · 動畫 / 冒險 | ★★★★☆ **4.2** | 喬許·科雷 | 嘉年華幻紫 (`purple-500`) |
| **《後室》** (The Backrooms) | 電影 · 科幻 / 恐怖 | ★★★★☆ **4.0** | 凱恩·帕森斯 | 閾限硫磺黃 (`yellow-500`) |
| **《楚門的世界》** (The Truman Show) | 電影 · 喜劇 / 哲思 | ★★★★☆ **4.6** | 彼得·威爾 | 烏托邦晴空藍 (`sky-500`) |
| **《沙丘：第二部》** (Dune: Part Two) | 電影 · 史詩 / 科幻 | ★★★★☆ **4.5** | 丹尼·維勒納夫 | 厄拉科斯琥珀金 (`amber-500`) |

---

## 📁 目錄結構

```text
HW9/
├── index.html                   # 首頁目錄（數據儀表板、即時搜尋、分類篩選、16:9 劇照卡片清單）
├── reviews/                     # 獨立影評頁面專用資料夾
│   ├── movie-hope.html          # 《素媛》深度影評
│   ├── drama-breaking-bad.html  # 《絕命毒師》深度影評
│   ├── movie-toy-story-4.html   # 《玩具總動員4》深度影評
│   ├── movie-the-backrooms.html # 《後室》深度影評
│   ├── movie-the-truman-show.html # 《楚門的世界》深度影評
│   └── movie-dune-2.html        # 《沙丘：第二部》深度影評
├── assets/                      # 16:9 電影劇照存儲庫
│   ├── movie-hope.jpg
│   ├── drama-breaking-bad.jpg
│   ├── movie-toy-story-4.jpg
│   ├── movie-the-backrooms.jpg
│   ├── movie-the-truman-show.jpg
│   └── movie-dune-2.jpg
├── AGENTS.md                    # 內部開發規範與 AI Agent 維護指引
├── .gitignore                   # Git 忽略設定
└── README.md                    # 專案說明文件
```
