# super-stock

AI理財學習教練

> **介紹**：協助理財新手學習投資理財的 AI 助教，透過引導式提問釐清觀念、以案例教學練習判斷，並以幻覺提醒標示 AI 可能出錯之處，培養使用者查證資訊、不盲信 AI 的習慣。

---

## 1. 背景與動機

- 青年與初入社會者普遍缺乏理財素養，容易在預算、儲蓄、信用與投資上做出不佳決策。
- 傳統理財課程偏重知識講授，缺少個人化回饋與情境練習，學習後不易落實到生活。
- 新手難以分辨 AI 回答是否正確，容易被幻覺誤導。
- 生成式 AI 可引導提問並即時回饋，適合擔任理財學習教練。

## 2. 專題目標

- 協助理財新手理解基礎金融與投資理財知識。
- 透過引導式提問，培養自主思考與財務判斷能力。
- 運用生活化案例，提升金融知識的理解與應用能力。
- 建立 AI 資訊查證與幻覺提醒機制，提升使用者對 AI 理財資訊的辨識能力。
- 
## 3. 核心功能
- AI 引導式學習： 透過循序漸進的提問，引導使用者理解金融概念，而非直接提供答案。
- 投資案例教學： 提供股票、ETF、基金、定期定額等情境案例，讓使用者練習分析與判斷。
- AI 理財知識問答： 使用者可自由詢問投資理財問題，AI 依照使用者程度提供解釋。
- AI 幻覺與風險提醒： 對不確定、缺乏來源或可能過時的資訊進行標示，提醒使用者查證，並區分一般知識與投資建議。
- 學習紀錄與能力回饋： 記錄學習歷程、案例作答及常見觀念錯誤，提供個人化學習建議。

## 4. 系統流程
flowchart TD
    %% 樣式定義
    classDef startEnd fill:#f5ebe0,stroke:#8d6e63,stroke-width:2px,color:#3e2723,font-weight:bold
    classDef decision fill:#fff3cd,stroke:#d4a373,stroke-width:2px,color:#5c4033,font-weight:bold
    classDef aiNode fill:#e8f4f8,stroke:#457b9d,stroke-width:1.5px,color:#1d3557
    classDef userNode fill:#e9f5db,stroke:#718355,stroke-width:1.5px,color:#2b3a1e
    classDef safetyNode fill:#fde2e4,stroke:#e56b6f,stroke-width:1.5px,color:#6d1a26,font-weight:bold

    A(["① 進入 AI 理財學習教練"]):::startEnd --> B{"② 選擇學習路徑"}:::decision

    %% 路徑一：自由提問
    B -->|自由提問| C["③-A AI 理財即時問答"]:::aiNode
    C --> F["④ AI 知識解釋與概念回饋"]:::aiNode

    %% 路徑二：主題學習（加入子區塊凸顯鷹架流程）
    subgraph Guided ["主題式決策鷹架路徑"]
        direction TB
        D["③-B AI 引導式提問<br/>釐清目標與風險承受度"]:::aiNode --> E["投資情境與案例解析"]:::aiNode
        E --> G["使用者模擬分析與決策"]:::userNode
    end

    B -->|主題學習| D
    G --> F

    %% 收斂與安全把關
    F --> H["⑤ 資訊查證與 AI 幻覺提醒<br/>（強調非投資建議）"]:::safetyNode
    H --> I["⑥ 生成學習紀錄與成效回饋"]:::aiNode
    I --> J{"⑦ 是否繼續學習？"}:::decision

    J -->|是，切換或延伸主題| B
    J -->|否，結束練習| K(["⑧ 完成本次理財學習"]):::startEnd

## 5. 實作範圍

| 項目 | 第一版（期末完成） | 未來擴充 |
| --- | --- | --- |
| 年級與單元 | 暫定：三年級「三位數加減直式（進退位）」 | 擴充到其他年級與單元 |
| 情緒偵測 | 表情圖卡／情境選擇 | 語音情緒偵測 |
| 小老師糾正 | 點選錯誤位置＋文字說明 | 語音說明（瀏覽器內建 Web Speech API） |
| 答案判斷 | 程式計算（不交給 AI） | — |

> 為什麼先不做語音情緒偵測？準確度不穩定，而且會蒐集兒童聲音，個資風險較高。

## 6. 技術架構

```mermaid
flowchart LR
    U["學生的瀏覽器<br/>（前端網頁）"] -->|只送題目與孩子的文字| F["Vercel Functions<br/>（後端代理）"]
    F -->|金鑰只存在這裡| G["Google Gemini API"]
    G --> F --> U
```

| 層級 | 使用技術 | 說明 |
| --- | --- | --- |
| 前端 | HTML、CSS、JavaScript（原生） | 不使用大型框架，平板與電腦都能開 |
| 後端 | Vercel Functions | 代替前端呼叫 AI，保護金鑰 |
| AI | Google Gemini API | 產生慢速提示與笨笨機器人的對話 |
| 部署 | Vercel（連結本 GitHub 儲存庫） | 推送程式後自動更新網站 |

**金鑰安全**

- 本儲存庫是公開的，**Gemini 金鑰不可寫在程式碼或前端網頁裡**。
- 金鑰設定在 Vercel 後台的環境變數 `GEMINI_API_KEY`；本機測試時放在 `.env`（已列入 `.gitignore`）。
- 在 Google AI Studio 設定用量上限，避免網址被濫用導致額度耗盡。

## 7. 預期成效與評估方式（暫定）

| 研究問題 | 評估方式 |
| --- | --- |
| RQ1：使用系統後，學生的數學焦慮是否下降？ | 使用前後的心情 Check-in 紀錄、數學焦慮量表 |
| RQ2：學生的單元解題正確率是否提升？ | 前後測 |
| RQ3：學生「我做得到」的自信是否提升？ | 數學自我效能問卷、小老師挑戰的成功率 |

## 8. 個資與資料保護

- 學生一律使用**代號**（班級代號＋座號，例如 30105），不蒐集姓名。
- 只蒐集達成研究目的的最少資料：代號、心情選項、作答紀錄。
- 情緒資料屬於敏感資料，**活動結束後即刪除**。
- 使用 Google Gemini 屬於資料委外處理；到班試用前，會先向學校資安窗口報備，並取得家長同意。

## 9. 時程規劃（暫定，依課程期限調整）

| 階段 | 工作內容 |
| --- | --- |
| 第 1–2 週 | 確定年級單元、整理迷思錯誤清單、設計畫面草圖 |
| 第 3–5 週 | 三個模組分頭開發，並約定模組之間的資料格式 |
| 第 6–7 週 | 串接三部曲流程、串接 Gemini API |
| 第 8 週 | 小規模試用與修正 |
| 第 9 週 | 整理成果、期末報告 |

## 10. 組員分工

| 組員 | 負責模組 | 主要工作 |
| --- | --- | --- |
| 余玟靜 | ① 情緒 Check-in | 情緒選項設計、安撫話術、前後心情比較 |
| 呂佳蓁 | ② 慢速鷹架解題 | 引導提問設計、提示分級、卡關判斷 |
| 張祐誠 | ③ 笨笨小老師挑戰 | 迷思錯誤設計、笨笨機器人對話、Gemini 串接 |
| 待定 | 系統整合 | 三個模組的資料串接、部署 |

## 11. 參考文獻（引用前請再核對原文）

> 近五年 SSCI 論文與相關專利（不限年份）整理，請見 [docs/文獻與專利.md](docs/文獻與專利.md)。

**經典理論文獻**

- Adams, D. M., et al. (2014). Using erroneous examples to improve mathematics learning with a web-based tutoring system. *Computers in Human Behavior, 36*, 401–411.
- Ashcraft, M. H. (2002). Math anxiety: Personal, educational, and cognitive consequences. *Current Directions in Psychological Science, 11*(5), 181–185.
- Biswas, G., Leelawong, K., Schwartz, D., Vye, N., & TAG-V. (2005). Learning by teaching: A new agent paradigm for educational software. *Applied Artificial Intelligence, 19*(3–4), 363–392.
- CASEL. *What is the CASEL framework?* https://casel.org/
- Chase, C. C., Chin, D. B., Oppezzo, M. A., & Schwartz, D. L. (2009). Teachable agents and the protégé effect: Increasing the effort towards learning. *Journal of Science Education and Technology, 18*(4), 334–352.
- Vygotsky, L. S. (1978). *Mind in society: The development of higher psychological processes*. Harvard University Press.
- Wood, D., Bruner, J. S., & Ross, G. (1976). The role of tutoring in problem solving. *Journal of Child Psychology and Psychiatry, 17*(2), 89–100.

---

## 資料夾結構

```
team-work/
├── docs/           文件、會議紀錄、簡報摘要、文獻與專利整理
├── .env.example    環境變數範本（複製成 .env 後自行填寫）
├── .gitignore      不上傳到 GitHub 的檔案清單
└── README.md       專題計畫書（就是這份）
```

## 協作規則

- 動手前先 `git pull`，取得組員最新的修改。
- 提交前先 `git status`，確認沒有 `.env` 或金鑰檔。
- commit 訊息用中文簡短寫清楚「改了什麼」，例如：`新增情緒 Check-in 畫面`。
- 本儲存庫為公開（Public），**不可放入金鑰、密碼或學生個資**。
