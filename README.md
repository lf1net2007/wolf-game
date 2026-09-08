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

## 部署到 GitHub Pages

Repo 設定裡打開 Settings → Pages，Source 選這個分支的根目錄，存檔後幾分鐘內會有一個 `https://<帳號>.github.io/<repo>/` 的連結，把這個連結分享給所有人（上帝跟玩家都用同一個連結）。
