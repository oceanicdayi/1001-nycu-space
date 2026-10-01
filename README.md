# AI Agent 賦能｜從個人研究軌跡，看見智慧化地震測報新藍圖

*AI Agents and the next blueprint for earthquake early warning — NYCU Institute of Space Systems, 2026.10.01*

2026 年 10 月 1 日於國立陽明交通大學太空研究所的演講。講者陳達毅（中央氣象署地震測報中心測報技術科科長；臺北市立大學地球環境暨生物資源學系兼任助理教授）從國家級地震測報的作業現場出發，說明地震儀記錄的不只是地震、即時系統真正卡住的地方，以及這三年 AI Agent 如何改寫一個人的研究產能，並提出下一代智慧化測報的藍圖。本儲存庫是這場演講的 HTML 簡報與教學影片頁，約 58 頁，內建講者備忘、計時與總覽。

## 線上觀看

- [簡報](https://oceanicdayi.github.io/1001-nycu-space/)
- [教學影片](https://oceanicdayi.github.io/1001-nycu-space/videos.html)

影片頁收四支 AI 研究工作流教學（GitHub、Cursor 雲端鏈路、本機工作台、Hermes Agent／Grok bot）。連結若仍顯示「準備中」，代表影片尚未上線，網址本身不會改。

## 演講大綱

1. **工作現場** — 地震預警是在跟 P 波與 S 波之間那幾秒賽跑：觀測網、Earthworm 模組化即時系統，以及從偵測到發布的延遲預算。
2. **地震儀記錄的不只是地震** — 爆炸的聲波、進入大氣層的火球、8500 公里外的東加火山。臺灣的地震網同時是全天候的地面與大氣觀測網。
3. **測報難題** — 誤報的代價、震度之外的持續時間、參數調校的上限，以及人工檢核為什麼不能省。
4. **AI 工作流** — 從對話、整合介面到自主 Agent；本機、雲端、純網頁、手機四條路線，以及正在運轉的地震預警多代理群。
5. **Demo 與成果** — 四個可複製的工作流、教學與研究產出，以及一顆 Raspberry Pi 上的邊緣地震預警。
6. **新藍圖與結語** — 先講兩個失敗，再談邊緣、雲端、Agent、人的四層架構，以及留給太空所的三句話。

## 檔案結構

```
.
├── index.html       簡報本體（約 58 頁）
├── videos.html      四支教學影片的入口頁
└── assets/          圖片、封面、QR code；內嵌影片在 assets/media/
```

## 本機觀看

用瀏覽器直接開啟 `index.html` 即可。若要讓相對路徑與內嵌影片較穩定，可在儲存庫根目錄起一個靜態伺服器：

```bash
python3 -m http.server 8000
```

然後開啟 <http://localhost:8000/>；影片頁為 <http://localhost:8000/videos.html>。

簡報快捷鍵：`←` `→` 換頁、`N` 講者備忘、`O` 總覽、`T` 計時（預設 50 分鐘）、`F` 全螢幕。

## 相關連結

簡報中提到、可另行查看的示範：

- 邊緣地震儀即時監測：<https://drewhui.github.io/RASPBERRYSHAKE_CCU/>
- 氣象署地震預警 Earthworm 容器：`docker pull cwadayi/earthworm_ubuntu22.04_eew:v1`

## 授權

本儲存庫目前未附開源授權條款。簡報、圖文與影片為演講教材，權利保留；轉載或改作前請先徵得講者同意。
