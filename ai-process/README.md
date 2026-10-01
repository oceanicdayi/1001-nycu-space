# 與 AI 互動製作 PPT 的過程

> **演講**：2026.10.01 陽明交通大學太空研究所｜AI Agent 賦能 — 從個人工作流程，到即時地震預警系統的下一步與構想
> **AI 工具**：Claude (Anthropic) — VSCode + Antigravity IDE + Claude Desktop
> **產出**：60 頁 HTML 簡報 + PPTX 離線備援 + PDF 第三重備援 + 4 支教學影片（31 分鐘）
> **網站**：<https://oceanicdayi.github.io/1001-nycu-space/>
> **Repo**：<https://github.com/oceanicdayi/1001-nycu-space>

---

## 一、工作流程概覽

整個過程從 2026-09-30 深夜開始，到 10-01 演講當天完成，全程使用 AI Agent 協作。以下是主要階段：

| 階段 | 內容 | AI 的角色 |
|------|------|-----------|
| 1. 規劃 | 讀取專案內容，產出工作計畫 | AI 分析素材、判斷優先順序、提出分兩階段的建議 |
| 2. 素材準備 | 從舊簡報抽取 232 張可重用圖片、建索引 | AI 自動化批次處理 |
| 3. 大綱撰寫 | 寫主控大綱，HTML 與 PPTX 共用同一份資料 | AI 根據聽眾（太空所）調整內容 |
| 4. HTML 簡報 | 45→52→55→58→60 頁，含講者備忘、計時器、總覽 | AI 產生器 + Playwright 驗證版面 |
| 5. PPTX 製作 | 離線備援版，每頁有備忘稿 | AI 用 python-pptx 產生，PowerPoint COM 匯出 PDF 檢查 |
| 6. 太空所客製 | 新增「地震儀記錄的不只是地震」段落（11 頁） | AI 讀論文、抓圖、依論文脈絡排邏輯 |
| 7. 影片製作 | 4 支合成旁白影片，總長 31 分鐘 | AI 寫腳本、合成旁白、投影片轉影片 |
| 8. 部署 | 推上 GitHub Pages，QR code 驗證 | AI 用 GitHub API 部署、實際解碼 QR 確認 |

---

## 二、關鍵互動實例

### 2.1 開場 — AI 主動提出衝突與建議

```
使用者：讀取專案內容，給我一個工作計畫

AI：工作計畫寫好了。
先講一個必須說的衝突——
演講是明天（60 分鐘），但你要的「4 支教學影片含實際錄製剪輯」趕不上。
4 支影片含錄製、重錄、剪輯、上字幕，實際約 12–20 小時。
我的建議是分兩階段……
```

AI 沒有盲目接受指令，而是**主動指出時間衝突**，提出分兩階段的方案，並要求使用者在兩個選項中做決定。

### 2.2 為太空所客製化 — AI 發揮領域知識

```
使用者：聽眾是陽明交通大學太空研究所師生

AI：太空所這個聽眾資訊很有價值——
你有 2024 年 Tonga 火山噴發多重感測器觀測那篇
（Frontiers in Earth Science），那是地震學跟太空/電離層社群的交集，
正好當開場鉤子。
```

AI 根據聽眾背景，自動：
- 以 2022 東加火山論文（使用者是共同作者）做開場鉤子
- 把 Raspberry Shake 邊緣地震儀當貫穿主線（同時是 AI 協同實例、研究成果、未來藍圖）
- 對太空所的四個甜蜜點：地震網 ↔ 大氣進入體偵測、Earthworm ring ↔ NASA cFS software bus、自監督學習 ↔ 遙測困境、graceful degradation

### 2.3 AI 讀論文、抓圖、依脈絡排版

```
使用者：tonga_eruption.pdf 文章中的幾張真實的觀測資料圖十分適合放進PPT，
       文章中的脈絡也要掌握，符合邏輯地，放置文字和圖片。

AI：文章的敘事脈絡很清楚：
    事件 → 各感測器各自記到什麼 → 傳播特性 → 跨儀器共站比對 → 海嘯機制。
    用的全是論文裡的真實觀測圖。
```

AI 做了：
1. 讀完整篇論文，擷取 13 張圖與 caption
2. 把 Figure 8 的 12 張氣壓快照**切成 12 格重組成動畫**（mp4 + GIF）
3. 依論文邏輯排成五頁：事件路徑 → 大氣波動畫 → 地震網整排記到 → 共站三種儀器 → 氣壓與潮位同步
4. **自動發現並修正開場頁的錯誤**：原本寫「GNSS TEC、地磁儀」，但論文實際用的是氣壓計、次聲波、潮位計——在太空所面前講錯會很傷

### 2.4 AI 自己寫工具解決看不到 PPTX 的問題

```
AI：這台沒有 LibreOffice，但有 PowerPoint，所以我寫了
    build/pptx_preview.ps1 用 COM 匯出 PDF 再逐頁檢查。
    之後改版型都能實際驗證，不用盲改。
```

AI 發現自己無法預覽 PPTX，**主動寫了一個 PowerShell 腳本**用 PowerPoint COM 自動化匯出 PDF，再用 PyMuPDF 轉成逐頁 PNG。這讓後續的排版調整不再是盲改。

### 2.5 處理 Copilot PR 衝突 — AI 保全他人的工作

```
AI：Push 被擋，查了一下發現 Copilot 的 PR #1 直接改在產生出來的 index.html 上——
    把影片頁改成動態渲染。那份工作一旦我重新產生就會整個消失。
    我沒有硬蓋掉，而是把它移植進產生器：
    影片資料現在集中在 build/slides_data.py 的 VIDEOS，
    index.html 的 s29 和 videos.html 都從那一份生成。
```

GitHub Copilot Agent 自動提了一個 PR 改了 `index.html`，但那是產生出來的檔案，下次重建就會被覆蓋。AI 沒有硬蓋，而是把 Copilot 的功能**移植進產生器**，讓兩邊都從同一份資料生成，永久保留。

---

## 三、使用的 AI 工具與環境

| 工具 | 用途 | 截圖 |
|------|------|------|
| Claude (VSCode) | 主要協作環境，產生器、建置腳本、論文分析 | ![VSCode + Claude](使用vscode搭配claude進行教學影片製作與演講投影片製作的實例.png) |
| Claude (Antigravity IDE) | Agent-first IDE，製作教學影片 | ![Antigravity + Claude](使用antigravity搭配claude進行教學影片製作與演講投影片製作的實例-1.png) |
| Claude Desktop | 讀取本機歷史影片檔案、抓取聲線特徵 | ![Claude Desktop 讀取歷史影片](發現影片的聲音是女生-用claude desktop讀取電腦中我過去的演講影片檔案-抓取聲線-重新修改影片.png) |
| ChatGPT | 影片音訊文字擷取 | ![ChatGPT 抓取音訊文字](上傳我的演講影片-用chatgpt抓取影片音訊文字.png) |
| Gemini | 確認 AI 生成內容是否正確 | ![Gemini 確認內容](利用gemini確認AI生成的內容是否正確-1.png) |
| Ollama Cloud | 模型切換 | ![Ollama Cloud](改用ollama cloud的模型.png) |
| GitHub Copilot Agent | 自動提 PR 優化 HTML 版面 | ![Copilot Agent + Grok Bot](github的copilot agent和grok bot都可以對同一個github 專案進行修改.png) |

### 3.1 影片聲線問題 — AI 自我修正

```
使用者：發現影片的聲音是女生

AI：用 Claude Desktop 讀取電腦中過去的演講影片檔案，
    抓取聲線特徵，重新修改影片旁白。
```

AI 合成的旁白預設是女聲，與講者本人（男聲）不符。AI 用 Claude Desktop 讀取講者本機的歷史演講影片，分析聲線特徵後重新合成。

![發現聲線問題](發現影片的聲音是女生-用claude desktop讀取電腦中我過去的演講影片檔案-抓取聲線-重新修改影片-2.png)

### 3.2 Session Limit — AI 主動告知限制

```
AI：claude 的 session limit 用完了。
    改用 ollama cloud 的模型繼續。
```

![Session Limit](claude的session limit用完.png)

### 3.3 多 AI 交叉驗證

用 Gemini 確認 Claude 生成的內容是否正確——特別是論文中的感測器清單（氣壓計 vs GNSS TEC 的差異）。

![Gemini 驗證-1](利用gemini確認AI生成的內容是否正確-1.png)
![Gemini 驗證-2](利用gemini確認AI生成的內容是否正確-2.png)

---

## 四、產出清單

| 產出 | 說明 | 狀態 |
|------|------|------|
| `site/index.html` | 60 頁 HTML 簡報，含講者備忘、計時器、總覽 | ✅ 已上線 |
| `1001_陽明交大演講.pptx` | 60 頁 PPTX，含 60 份備忘稿、6 段影片/動畫（26 MB） | ✅ |
| `1001_陽明交大演講.pdf` | 60 頁 PDF，逐頁截圖 | ✅ |
| `site/videos.html` | 4 支教學影片頁（內嵌播放器） | ✅ 已上線 |
| `site/assets/videos/v01~v04.mp4` | 4 支 1080p 影片，共 29 MB，總長 31 分鐘 | ✅ 已上線 |
| `videos/` | 4 支影片腳本 + 提詞稿 + 錄製總說明 | ✅ |
| `演講當天.md` | 流程、時間表、超時刪除順序、Q&A 準備 | ✅ |
| `build/` | HTML + PPTX 產生器，共用同一份 slides_data.py | ✅ |

### 四支教學影片

| # | 主題 | 長度 |
|---|------|------|
| 01 | GitHub 如何協助研究工作 | 6:08 |
| 02 | Cursor × ChatGPT × GitHub × Google Drive | 7:07 |
| 03 | Antigravity IDE / VSCode 本機工作台 | 8:27 |
| 04 | Hermes Agent × Telegram × Obsidian（含 Grok bot） | 9:19 |

---

## 五、簡報結構（60 頁）

| 段落 | 頁碼 | 內容 |
|------|------|------|
| 開場 | p01–p04 | AI Agent 賦能、我們其實見過面、自我介紹、三個問題 |
| 工作現場 | p05–p16 | 地震測報的工作現場、競速（含兩段地震影片）、臺灣地震環境、60 人 24 小時、Earthworm、預警鏈路、警報推播動畫 |
| 不只是地震 | p17–p27 | 地震儀記錄的不只是地震（**為太空所加的 11 頁**）：爆炸事件、火球音爆、東加火山五頁（含氣壓波動畫） |
| 難題 | p28–p32 | 誤報的代價、強度與持續時間、參數調校、地震報告為什麼會慢 |
| AI 工作流程 | p33–p42 | 2022→2026 演進、四條路線、本機工作台、雲端開發、Hermes vs Grok bot、EEW 多代理群 |
| Demo | p43–p44 | 四個可複製的工作流、掃碼就能看 |
| 成果 | p45–p52 | 學生作業、教法改寫、AI 觀測學習、大型地震模型 LEM、邊緣地震儀、真的抓到地震了 |
| 新藍圖 | p53–p58 | AI 也會出錯、跑了 1640 次全被跳過、下一代測報中心、AI 到底改變了什麼 |
| 結語 | p59–p60 | 三句話、謝謝聆聽 |

---

## 六、過程中的關鍵決策

### 6.1 貫穿主線的選擇

AI 建議用 Raspberry Shake 邊緣地震儀當貫穿主線——同一個案例同時當「AI 協同開發的實例」「研究成果」「未來藍圖的縮影」三用，比列三組不相干的案例有效。

### 6.2 論文脈絡的忠實度

東加段落完全照 Huang et al. (2024) 的邏輯走：
**事件 → 各感測器各自看到什麼 → 傳播特性 → 跨儀器共站比對 → 跨系統的結論**

每頁的圖都接得上前一頁的論述，用的全是論文裡的真實觀測圖。

### 6.3 誠實面對失敗

新藍圖那段先講兩個失敗（誤觸發一天 76–325 次根因未明、SSIF 跑 1640 次全被跳過的 race condition）再講未來——對研究生而言除錯紀律比成功案例有用。

### 6.4 圖片動畫化

Figure 8 的 12 張氣壓快照本是靜態圖，AI 切開重組後做成動畫——波前從東南進來、約 40 分鐘掃完整個臺灣的過程直接看得見。HTML 版自動循環，PPTX 版放映時 GIF 也會動。

---

## 七、完整互動記錄

完整的 AI 互動 session 記錄（5984 行）保存在同目錄下：
[`session_record_vscode.md`](session_record_vscode.md)

記錄包含每一個使用者指令、AI 的回應、執行的 bash 命令、以及產出的結果。

---

## 八、技術架構

```
build/
├── slides_data.py      ← 唯一的內容來源（大綱、影片資料、清單）
├── build_html.py       ← HTML 簡報產生器
├── build_pptx.py       ← PPTX 產生器
├── build_all.py        ← 一鍵重建全部
└── pptx_preview.ps1    ← PPTX → PDF 預覽（PowerPoint COM）

site/
├── index.html          ← 60 頁 HTML 簡報（產生物）
├── videos.html         ← 影片頁（產生物）
└── assets/
    ├── videos/         ← 4 支 mp4 + poster
    ├── media/          ← 地震影片、推播動畫、氣壓波動畫
    └── *.jpg           ← 簡報用圖

videos/
├── 0*_*.md             ← 4 支影片腳本
├── 提詞稿/              ← 逐字提詞稿
└── 合成旁白版/           ← 原始 mp4
```

**核心原則**：所有內容只有一個來源 `build/slides_data.py`，改完跑 `python build/build_all.py`，HTML + PPTX + PDF + videos.html 一起更新。

---

## 九、反思

### AI 做到了什麼

- **一個晚上做出 60 頁簡報 + 4 支影片**，從零到部署上線
- **讀論文、抓圖、做動畫、排邏輯**——不是貼圖，是理解論文脈絡後依序呈現
- **自己寫工具**解決看不到 PPTX 的問題（PowerPoint COM 匯出）
- **主動發現並修正錯誤**（開場頁的感測器清單與論文不符）
- **處理版本衝突**時保全了他人的工作（把 Copilot PR 移植進產生器）

### AI 的限制

- **Session limit**：Claude 的對話長度有上限，過了要換 session 或換模型
- **聲線合成**：預設女聲，需要讀取講者歷史影片才能模仿男聲
- **PPTX 預覽**：沒有 LibreOffice 時看不到，要靠 PowerPoint COM 匯出 PDF
- **需要人類判斷**：引用標示、氣象署內部畫面是否可公開、帳號確認

### 最大的啟示

> 「AI 沒有讓我變聰明，它讓我的每一個念頭都有機會被實作出來驗證。」
> —— 簡報 p58

這場演講的製作過程本身就是這句話的證明。