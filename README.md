# 狼人殺主持台

實體聚會玩狼人殺用的主持輔助網頁：上帝導讀夜晚/白天流程、記錄身分與技能發動、上警與放逐投票、即時票數統計。玩家完全不用登入、不用裝 App，開連結就能用。

- `index.html` — 正式版本，接 Firebase，部署在 GitHub Pages 給大家用連結打開。
- `werewolf-host.html` — 舊版本（Claude Artifact 版，需要 Claude 帳號登入），保留作參考，不部署。
- `firestore.rules` — Firestore 安全規則，設定 Firebase 時要貼到主控台的 Firestore →規則。

## 一次性設定（架設者做一次就好，玩家完全不用做）

1. 到 https://console.firebase.google.com 建立一個新專案（免費方案即可）。
2. 左側選單「Build → Firestore Database」→ 建立資料庫 → 選「正式環境模式」（region 選離台灣近的，如 asia-east1）。
3. 到「Firestore Database → 規則」，把 `firestore.rules` 的內容整個貼上並發布。
4. 左側選單「Build → Authentication」→ 開始使用 → 登入方式選「匿名／Anonymous」→ 啟用。這一步就是讓玩家完全不用登入的關鍵。
5. 左側選單「專案設定（齒輪）→ 一般」，往下捲到「你的應用程式」，點網頁圖示 `</>` 新增一個網頁應用程式（不用勾 Firebase Hosting）。
6. 複製出現的 `firebaseConfig` 物件，貼到 `index.html` 最上面取代裡面的 `YOUR_API_KEY` 等範例值。
7. 存檔、`git commit`、`git push`，GitHub Pages 會自動更新。

## App Check（評估過，目前決定不裝）

`firebaseConfig` 出現在公開 repo 裡是正常的（不是密鑰外洩），理論上有人拿到這組設定值可以不透過網頁、自己寫腳本直接打資料庫。App Check 可以擋掉這種流量，`index.html` 裡已經留了 `appCheckSiteKey` 的骨架、`YOUR_RECAPTCHA_SITE_KEY` 是保持未啟用的預留值。

**目前決定先不裝**：申請 reCAPTCHA 時 Google 主控台只提供「reCAPTCHA Enterprise」，需要幫這個 GCP 專案掛上帳單帳戶（信用卡）；一旦掛卡，Firebase 專案很可能會從 Spark（免費方案，超額只會擋掉請求、絕不收費）變成 Blaze（隨用隨付），連 Firestore 本身超過免費額度都會變成真的收費，而不是現在這種「壞掉但不花錢」的保底狀態。對這種朋友聚會用的小專案，這個代價大於被陌生人打爆配額的實際機率，所以先維持 Spark 方案、不裝 App Check。

如果之後想法改變（例如連結真的被分享出去、需要更強的防護），可以重新評估這個取捨。

## 部署到 GitHub Pages

Repo 設定裡打開 Settings → Pages，Source 選這個分支的根目錄，存檔後幾分鐘內會有一個 `https://<帳號>.github.io/<repo>/` 的連結，把這個連結分享給所有人（上帝跟玩家都用同一個連結）。
