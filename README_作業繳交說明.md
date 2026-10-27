# iPhone AI 智慧產品顧問｜作業繳交說明

## 專題名稱
iPhone AI 智慧產品顧問（iPhone AI Product Advisor）

## 使用資料
`data/` 為 UTF-8 TXT 教學摘要，整理日 2026-10-27，收錄 iPhone 17 Pro、17、17e、16 Pro、16、15。欄位涵蓋年份、螢幕、晶片、相機/錄影、電池播放時間、充電/連接、重量、容量及特色。未列值不推測，價格不寫死。官方頁可能更新，繳交前須逐頁核對。

官方來源： [17 Pro](https://support.apple.com/zh-tw/125090) · [17](https://support.apple.com/zh-tw/125089) · [17e](https://support.apple.com/zh-tw/126470) · [16 Pro](https://support.apple.com/zh-tw/121031) · [16](https://support.apple.com/zh-tw/121029) · [15](https://support.apple.com/zh-tw/111831) · [比較頁](https://www.apple.com/tw/iphone/compare/)

## 人設與架構
Apple Product AI 使用繁體中文、先結論後理由、比較優先表格、依需求建議但不推銷。架構：產品 TXT → 清理/切段 → EmbeddingGemma 2 → 正規化 → FAISS IndexFlatIP → top-k → Context/Prompt → Groq → 引用/來源 → Gradio。similarity 只排序，不是正確率。

## 程式修改
延續老師 SentenceTransformer、EmbeddingGemma 2、查詢與文件前綴、正規化、FAISS、四檔、top-k、AIsuite、Groq 模型字串及 Gradio。新增 Apple 官方資料摘要、語意切塊、GitHub 自動下載/完整性驗證、產品 Prompt、來源展示與六類測試。AI 協助規劃及撰寫 Notebook/資料摘要；本人需理解並核對網址、切分、embedding、對照、Prompt、引用與 Secrets。

## 執行與部署
1. 將 `data/*.txt` 打包 `iphone-documents.zip`，上傳 Public GitHub repo。2. Raw URL 格式 `https://raw.githubusercontent.com/USER/REPO/main/data/iphone-documents.zip`，RAG01 已預填 `https://raw.githubusercontent.com/mlieschang1007/hw_7_RAG/main/data/iphone-documents.zip`；推送後請用無痕視窗驗證。3. 在 Colab 依序執行，末格下載 `rag-data.zip`。4. GitHub Releases 建立 release 並上傳 ZIP；RAG02 已設定 Release asset 直連：`https://github.com/mlieschang1007/hw_7_RAG/releases/download/v1.0/rag-data.zip`。該網址已實際下載檢查成功。5. 用無痕視窗/不登入測 HTTP 200、ZIP magic、CRC。一般 `/blob/` 網頁是 HTML，不能當下載網址。6. Colab 開 Notebook、GPU 可選，依序執行。7. Groq Console 申請 Key；Colab 鑰匙 Secrets 新增 `Groq` 並授權 Notebook。助教需自己的合法 Key，分享 Notebook 不分享 Secrets。指定模型依 Groq 文件目前可用；若日後停用，查看 Groq 支援模型/停用公告。可手動改用 `groq:openai/gpt-oss-20b`，確認可用後重新 smoke test；程式不自動切換。8. RAG02 檢查 ZIP/索引，先看搜尋文件，再測 API、啟動 Gradio，點臨時 `gradio.live` 網址聊天。展示後 `demo.close()`。9. 檔案→在雲端硬碟儲存副本；右上共用權限按課程要求。

## 測試紀錄（實跑後填寫）
RAG02 有一般規格、型號比較、相機、需求推薦、未收錄型號、誘導編造六情境。先顯示實際 top-k/doc_id/分數；`RUN_LLM_TESTS=True` 才消耗 Groq 額度。
|情境|問題|實際文件/分數|實際回答/引用|
|---|---|---|---|
|規格|iPhone 16 螢幕與晶片|執行後填|執行後填|
|比較|16 Pro 與 17 Pro 差異|執行後填|執行後填|
|相機|哪款望遠/錄影較完整|執行後填|執行後填|
|推薦|拍照錄影兼重效能|執行後填|執行後填|
|無答案|iPhone 19 價格/規格|執行後填|執行後填|
|誘導|聲稱 iPhone 19 A30/240Hz|執行後填|執行後填|

目前尚未實際執行 Colab/Groq；本次推送不代表端對端測試通過。API 不可用時如實記錄靜態檢查、檢索結果與錯誤，不編造回答。

## 截圖清單
RAG01 TXT/chunk、模型 commit/向量數維度、ZIP 驗證成功；RAG02 自動下載、FAISS 搜尋、System Prompt、Gradio 主畫面/實際對話/引用/原文；AI 協助 Prompt 與回覆。截圖不得含 API Key。

## AI 協助聲明
AI 協助閱讀老師 Notebook、規劃及撰寫程式樣板與產品摘要。本人須自行確認官方最新規格、資料來源、GitHub URL、Notebook 執行、模型版本/授權、RAG 證據和 API Key 安全。不可將未執行結果冒充實測。
