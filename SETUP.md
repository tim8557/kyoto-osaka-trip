# 京都大阪共編版

本機 index.html 已整合 Firebase Google 登入與 Firestore 即時同步。尚未登入或未列入團員名單，不能讀取雲端行程。每筆修改以交易檢查版本，避免覆蓋他人修改。離線時暫停寫入。

## 必須完成的雲端設定

1. 在 Firebase 專案 kyoto-osaka-trip-60ea1 的 Firestore → Rules，貼上專案根目錄 firestore.rules 的完整內容並發布。僅啟用 Firestore 不代表已套用權限規則。
2. 透過已授權的 HTTPS 網址或 localhost 開啟 index.html，不能直接雙擊 HTML 登入。localhost 也須加入 Authentication 的授權網域。
3. 使用 Google 登入後，畫面會顯示 UID。在 Firestore Console 建立 members/{UID} 文件，新增 role 字串欄位，填 editor（編輯）、viewer（僅檢視）或 owner（編輯）。可用 disabled 布林欄位 true 停用。名單僅由 Console 管理，網頁不能自行加人或升權。既有未填 role 的團員相容為 editor；建議明確指定。
4. 第一位可編輯團員匯入 private-trip-setup.json，再匯入 initial-trip-backup.json；若舊網頁有修改，請改用舊網頁匯出的 JSON。原瀏覽器 localStorage 資料不會被刪除。
5. 以兩個團員帳號測試新增、編輯、刪除同步及同筆編輯衝突；以 viewer 測試僅檢視，再以非團員帳號確認不能讀取。雲端測試需實際登入，目前未代為完成。

## 備份與還原

可匯出含日期設定、行程及預約的 JSON。匯入／還原會先確認，並用交易取代目前畫面中已有的資料；若已有資料被其他團員修改則拒絕。匯入期間其他團員新建的資料會保留，避免誤刪。現有與備份合計上限 450 筆。日期設定僅初次建立，不會因還原改動。

原專案已備份到本次 Codex 工作區的 work/original-project 資料夾（不會放入網站目錄）。舊版 localStorage 可由原版匯出；若網站使用相同來源，新版仍不會自動將本機內容上傳，避免覆蓋共用旅程。

## 發布檔案

網站只需 index.html（或 private-collab/index.html）。不要把 initial-trip-backup.json、private-trip-setup.json、備份或舊版 HTML 發布到公開網站。舊公開網址若仍存在，其歷史行程仍可被查看；登入頁無法回收已公開的內容。

firebase.json 僅用於發布規則，不包含 Hosting 設定。若已安裝並登入 Firebase CLI，可在專案目錄執行 `firebase deploy --only firestore:rules --project kyoto-osaka-trip-60ea1`；也可直接使用 Console。

本次已檢查 JavaScript 語法、輸入驗證、地圖連結、編輯日期不受同步重設、衝突拒絕、檢視者寫入攔截、登出清除介面。未完成真實 Google 登入、雙帳號同步或 Firestore 規則模擬器測試。

