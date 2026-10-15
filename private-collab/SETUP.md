# 私人共編行程設定

這是準備稿；建立 Firebase 專案並套用安全規則前，請勿把舊版公開行程當作私人網站使用。

## 1. 保留既有資料

先在目前的行程網頁按「匯出備份」，保存 JSON。這份檔案可能包含預約資訊，不要上傳到 GitHub。

## 2. 建立 Firebase

1. 在 Firebase Console 建立專案及「網頁應用程式」，取得 `firebaseConfig`。
2. 啟用 Authentication 的 **Google** 登入方式。
3. 建立 **Cloud Firestore** 資料庫；不要使用允許公開讀寫的測試規則。將 `firestore.rules` 的完整內容貼到 Firestore「Rules」並發布。
4. 在 Authentication 的「Settings → Authorized domains」加入 GitHub Pages 網站網域，例如 `使用者名稱.github.io`。
5. 在 `index.html` 開頭的 `firebaseConfig` 填入 Firebase 網頁設定值。這些值是公開識別資訊；**不要**貼入服務帳戶 JSON、私密金鑰或管理員憑證。

## 3. 發布登入頁面

在**新的 GitHub 儲存庫**只上傳設定後的 `index.html`，並以 GitHub Pages 發布。不要上傳另附的 `private-trip-setup.json`、現有行程備份或舊版含內建行程的 HTML。

原有公開網站仍可能讓任何人看到舊行程；完成新網站後，請將舊網站取消發布或刪除舊公開檔案。新登入頁面本身是公開檔案，但不包含行程資料；Firestore 規則才是資料存取界線。

## 4. 加入團員

每位團員先開新網站並使用 Google 登入。尚未加入名單時，頁面會顯示其 Firebase UID。專案管理者在 Firestore Console 建立 `members/{UID}` 文件（可加一個 `name` 字串欄位），再請該團員重新登入。`members` 文件只能由 Firebase Console 管理，網頁無權新增團員。

## 5. 搬入旅程

由一位已加入的團員依序操作：

1. 按「匯入私人旅程設定」，選取另附的 `private-trip-setup.json`。它只包含日期、每日標題與說明；不要把它上傳到 GitHub。
2. 按「匯入現有行程 JSON」，選取步驟 1 從舊網頁匯出的 JSON。這只允許在共用資料庫尚無行程和預約時執行一次。
3. 用另一位已加入的團員帳號打開網站，核對行程同步與編輯權限。再用不在名單內的帳號確認無法讀取行程。

同一筆行程若已被其他團員先改動，網頁會拒絕覆蓋並提示重新查看。不同項目各自儲存，可以同時編輯。請定期從共編網頁匯出 JSON 備份。
