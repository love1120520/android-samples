# Copilot 使用說明 — android-samples

以下說明讓 AI 編碼代理（Copilot/agents）能快速在此 repository 內勝任工作。內容聚焦可被程式碼直接驗證、且對編輯/新增程式碼有即時影響的專案慣例與工作流程。

- **專案一覽（Big picture）**: 這是 Google Maps Android 範例集合，主要子專案：
  - `ApiDemos`：大量小範例展示 Maps SDK 功能（有自動產生檔案標記 V3_FILE_HEADER，不要修改該檔案內的註記說明）。
  - `FireMarkers`：使用 Firebase Realtime Database 實作 controller/agent 同步動畫（請參考 `FireMarkers/README.md` 與 `FireMarkers/ARCHITECTURE.md`）。
  - `WearOS`、`tutorials/`、`snippets/`：其他示範與教學範例。

- **為何這樣分層**: 每個子資料夾通常是獨立的 Android Studio / Gradle module，便於單獨 build 與發佈；`ApiDemos` 還有 codegen（V3_FILE_HEADER）因此部分檔案不應手動編輯。

- **建置、執行與測試（關鍵命令）**:
  - 在 Linux/Mac/WSL: `./gradlew build`
  - 在 Windows: `gradlew.bat build`（或雙擊 `gradlew.bat`）
  - 在 Android Studio: 選「Open an existing project」，載入某個 sample 資料夾即可執行模擬器或裝置。 
  - 單一 module 範例: `./gradlew :ApiDemos:project:assembleDebug`（module 路徑視實際子專案而定）。

- **重要檔與設定位置（常查）**:
  - API 金鑰與秘密：`secrets.properties`（範例在 `FireMarkers/README.md` 說明）；不要提交至版本控制，`.gitignore` 已將其排除。
  - 範例預設值：`local.defaults.properties`（例如 `ApiDemos/project/local.defaults.properties`，內含 `MAPS_API_KEY` 等占位符）。
  - 版本管理：各子專案的 `gradle/libs.versions.toml`（例如 `ApiDemos/project/gradle/libs.versions.toml`）集中管理依賴版本。
  - 自動產生檔案標記：`ApiDemos/V3_FILE_HEADER` 明確宣告某些來源自 codegen，請勿直接修改被標記之檔案。
  - ProGuard/混淆：有 `consumer-rules.pro` 與 `proguard-rules.pro` 在相應模組中。

- **專案慣例與模式（可用於修正/新增程式碼時參考）**:
  - 多 module、以 Gradle Kotlin DSL (`build.gradle.kts`) 為主。修改版本請優先考慮 `libs.versions.toml`。
  - 範例程式通常保留明確的 README 與使用步驟（例如 `FireMarkers/README.md`），新增範例時請一併新增說明與執行指示。
  - 安全性：所有關鍵金鑰、`secrets.properties` 與 `google-services.json` 不應被提交 — 若需要示範，請建立 `local.defaults.properties` 或範例檔案且含明確替換指示。

- **整合點與外部依賴**:
  - Google Maps SDK for Android（必須在 GCP 啟用並取得 API key）。
  - FireMarkers 使用 Firebase Realtime Database 並示範 Hilt DI 與 Jetpack Compose（請參考 `FireMarkers/README.md` 有具體 setup 步驟）。

- **編輯注意事項（禁止/建議）**:
  - 不要在有 V3_FILE_HEADER 註記的檔案內直接修改內容；若需改變，找相對應的 `app/src/gms` 原始來源檔。
  - 不要將 `secrets.properties` 或真實 API key 提交；在 PR 的範例設定中請保留占位符。
  - 當更新依賴或 build logic，先修改相對應的 `libs.versions.toml` 或 `build.gradle.kts`，並執行 `./gradlew build` 以驗證整體相容性。

- **範例快速參考（可直接檢視）**:
  - 專案首頁說明: [README.md](README.md)
  - codegen 標記: [ApiDemos/V3_FILE_HEADER](ApiDemos/V3_FILE_HEADER)
  - 範例設定: [ApiDemos/project/local.defaults.properties](ApiDemos/project/local.defaults.properties)
  - Firebase 範例與架構說明: [FireMarkers/README.md](FireMarkers/README.md)

- **當你不確定時的安全作法**:
  - 若變更可能暴露金鑰或憑證，先檢查 `.gitignore` 與 `local.defaults.properties` 用法；若需要自動化測試的假值，使用占位符檔案。
  - 若修改看似由 codegen 產生，搜尋 `V3_FILE_HEADER` 與 `app/src/gms` 來源再行修改原始來源。

請檢查並回饋是否需要把更多模組（例如 `snippets`、`tutorials`）加入「常用任務」範例命令或加入 CI/CD 特殊條件說明。
