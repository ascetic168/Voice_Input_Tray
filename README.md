# Voice Input Tray - Tauri 版本

> 跨平台系統托盤語音輸入工具 - 使用騰訊雲 ASR 將語音即時轉換為文字

## 專案狀態

✅ **已發布 v0.2.0** - Tauri v2 跨平台版本，包含 GUI 設置界面。

## 為什麼使用這個程式

與市面上其他語音輸入工具相比，Voice Input Tray 解決了三個核心痛點：

### 1. 連續麥克風錄音，隨按隨講不吃字
採用持續監聽技術，按下熱鍵的瞬間即可開始說話，完全沒有啟動延遲。同時，其他應用程式也能同時使用麥克風，不會有麥克風獨佔問題——您可以一邊通話、一邊使用語音輸入。

### 2. 單鍵 CTRL，微秒級啟動，智能判斷使用意圖
只需按住 Ctrl 鍵即開始錄音，程式會自動判斷您的使用意圖：
- 按下 Ctrl 後 2 秒內按其他鍵 → 自動取消，不影響 Ctrl+C、Ctrl+V 等快捷鍵

您再也不會看到「沒有錄到聲音」之類的訊息——從您按下的那一刻起，錄音就已經在進行了；即便取消錄音，也是安靜的處理，不會干擾使用者。

### 3. 騰訊雲自動標點，反應速度更快
內建騰訊雲的自動標點符號功能，識別結果直接帶上標點，無需額外的 LLM 後處理，大幅提升回應速度。

## 功能特色

- **跨平台支援** - 支援 Windows、macOS、Linux
- **GUI 設置界面** - 友好的圖形設置窗口
- **快捷鍵觸發** - 按住 Ctrl 鍵即可開始錄音，放開自動識別並貼上
- **系統托盤運行** - 背景運行，不干擾日常工作流程
- **即時語音識別** - 使用騰訊雲 ASR 引擎，準確度高
- **自動標點符號** - 輸出結果自動帶上標點符號，無需額外 LLM 處理，速度更快
- **多引擎支援** - 支援多種 ASR 引擎（普通話、粵語、英文等），可透過設置窗口切換
- **繁簡轉換** - 自動根據系統地區轉換中文繁簡體
- **開機自啟** - 支援系統開機自動啟動功能

## 系統需求

- **Windows**: Windows 10/11
- **macOS**: macOS 10.15+ (Catalina 或更新版本)
- **Linux**: 主流發行版 (Ubuntu、Fedora 等)
- 麥克風設備
- 騰訊雲 API 憑證

## 下載與安裝

### Windows 用戶

前往 [Releases 頁面](https://github.com/ascetic168/Voice_Input_Tray/releases) 下載最新版本：

- **NSIS 安裝包** (推薦): `Voice Input Tray_0.1.0_x64-setup.exe`
- **MSI 安裝包**: `Voice Input Tray_0.1.0_x64_en-US.msi`

雙擊安裝包完成安裝後，程式會自動啟動並在系統托盤顯示藍色麥克風圖示。

### macOS / Linux 用戶

目前需要自行編譯，請參考下方的「從原始碼編譯」章節。
(目前本專案仍在初期發展中，尚未開源)

### 取得騰訊雲 API 憑證

程式需要使用騰訊雲 ASR（語音識別）服務，因此需要先取得 API 憑證。

#### 步驟一：註冊騰訊雲帳號

1. 前往 [騰訊雲官網](https://cloud.tencent.com/) 註冊帳號
2. 支援以下註冊方式：
   - 微信掃碼註冊（推薦，最快）
   - 手機號碼註冊
   - 電子郵件註冊

#### 步驟二：完成實名認證

根據中國大陸法規，使用騰訊雲服務需要完成實名認證：

- **個人認證**：微信已實名認證時，透過微信掃碼即可。
- **企業認證**：需要營業執照或公司登記證明

> 💡 **提示**：如果不想進行中國大陸實名認證，可以使用 [騰訊雲國際版](https://www.tencentcloud.com/)，註冊流程更簡單。

#### 步驟三：開通語音識別服務

1. 登入騰訊雲控制台
2. 搜索或前往「語音識別」產品頁面
3. 開通語音識別服務（通常有免費額度可供測試）

截至撰稿為止，騰訊雲每個月都會送免費資源，對於個人來說相當夠用。測試的結果顯示，跨境可以使用非流式的呼叫。

#### 步驟四：取得 API 密鑰（Secret ID 和 Secret Key）

1. 前往 **[雲 API 密鑰控制台](https://console.cloud.tencent.com/cam/capi)**
2. 點擊「**新建密鑰**」按鈕
3. 系統會生成一對密鑰：
   - **SecretId**：類似 `AKIDxxxxxxxxxxxxxxxxxx`
   - **SecretKey**：類似 `xxxxxxxxxxxxxxxxxx`
4. ⚠️ **重要**：SecretKey 只在創建時顯示一次，請立即複製保存！
   - 如果遺失 SecretKey，無法再次查看，只能重新創建新的密鑰

**相關連結：**
- [雲 API 密鑰控制台](https://console.cloud.tencent.com/cam/capi)
- [如何取得雲 API 密鑰 - 腾讯云文档](https://cloud.tencent.com/document/product/598/40489)
- [騰訊雲實名認證指引](https://cloud.tencent.com/document/product/378/10496)

### 設置 API 憑證

安裝完成後，右鍵點擊系統托盤中的藍色麥克風圖示，選擇「設置」，在設置窗口中輸入您的騰訊雲 API 憑證。

或者，您也可以將 API 憑證設置為系統環境變數：

#### 方法一：使用 GUI 設置

1. 按 `Win + R`，輸入 `sysdm.cpl` 並按下 Enter
2. 切換到「進階」分頁，點擊「環境變數」按鈕
3. 在「使用者變數」或「系統變數」區域點擊「新增」
4. 新增以下兩個變數：

   | 變數名稱 | 變數值 |
   |---------|-------|
   | `TENCENTCLOUD_SECRET_ID` | 您的 Secret ID |
   | `TENCENTCLOUD_SECRET_KEY` | 您的 Secret Key |

5. 點擊「確定」儲存設定

#### 方法二：使用命令列設置

**設定使用者變數（推薦）：**
```powershell
setx TENCENTCLOUD_SECRET_ID "your_secret_id_here"
setx TENCENTCLOUD_SECRET_KEY "your_secret_key_here"
```

**設定系統變數（需要管理員權限）：**
```powershell
setx TENCENTCLOUD_SECRET_ID "your_secret_id_here" /M
setx TENCENTCLOUD_SECRET_KEY "your_secret_key_here" /M
```

> **注意：** 設定環境變數後需要重新啟動程式才能生效。

## 使用方法

### 啟動程式

安裝後程式會自動啟動。手動啟動方式：

- **Windows**: 開始選單 → Voice Input Tray
- 或雙擊安裝目錄中的執行檔

程式啟動後會在系統托盤顯示藍色麥克風圖示。

### 語音輸入操作

1. 按住 **Ctrl** 鍵開始錄音（可立即說話）
2. 說話
3. 放開 **Ctrl** 鍵停止錄音
4. 識別完成的文字會自動貼上到焦點視窗

#### Ctrl 單鍵模式說明

- **按下 Ctrl** → 立即開始錄音（可同時開始說話）
- **在 2 秒內按其他鍵**（如 C、V）→ 自動取消錄音，不影響 Ctrl+C、Ctrl+V 等快捷鍵操作
- **超過 2 秒放開 Ctrl** → 進行語音識別

這個設計讓您可以在不影響日常快捷鍵操作的情況下，隨時使用語音輸入功能。

### 托盤選單功能

右鍵點擊托盤圖示可顯示選單：

- **設置** - 開啟設置窗口（API 憑證、引擎選擇、簡繁轉換等）
- **結束** - 退出程式

### 設置窗口

通過設置窗口可以配置：

- **API 憑證** - 輸入騰訊雲 Secret ID 和 Secret Key
- **辨識引擎** - 選擇 ASR 引擎（普通話、普通話（視頻）、英文、粵語、川渝方言）
- **繁簡轉換** - 自動檢測、強制正體、不轉換

### 支援的 ASR 引擎

| 引擎代碼 | 說明 |
|---------|------|
| `16k_zh` | 普通話（預設） |
| `16k_zh_video` | 普通話（視頻場景） |
| `16k_en` | 英文 |
| `16k_yue` | 粵語 |
| `16k_ca` | 川渝方言 |

### 簡繁轉換模式

| 模式 | 說明 |
|------|------|
| `auto` | **預設**，自動檢測系統地區 |
| | - 繁體地區：轉換為正體中文 |
| | - 簡體地區：不轉換，維持簡體 |
| | - 非中文地區：轉換為正體中文 |
| `traditional` | 強制將辨識結果轉換為正體中文（台灣用詞） |
| `none` | 保持原始辨識結果，不進行轉換 |

## 開機自動啟動

### Windows

安裝包會自動創建開機自動啟動項。如需手動設置：

**方法一：使用設置窗口（推薦）**
1. 右鍵點擊托盤圖示，選擇「設置」
2. 在「系統設置」中勾選「系統開機時載入執行」
3. 點擊「保存設置」

**方法二：手動設置**
1. 按 `Win + R`，輸入 `shell:startup`
2. 將程式捷徑移動到開啟的資料夾

### macOS / Linux

目前需要通過系統設置或手動添加啟動項：

**macOS:**
- 系統設置 → 一般 → 登入項 → 添加程式

**Linux:**
- 複製 `.desktop` 文件到 `~/.config/autostart/` 目錄

## 從原始碼編譯

### 開發環境需求

- Node.js 18+
- Rust 1.70+
- (Windows) Visual Studio C++ Build Tools
- (macOS) Xcode Command Line Tools
- (Linux) libwebkit2gtk-4.0-dev libssl-dev libgtk-3-dev libayatana-appindicator3-dev librsvg2-dev

### 編譯步驟

```bash
# 克隆專案
git clone https://github.com/ascetic168/Voice_Input_Tray.git
cd Voice_Input_Tray

# 安裝依賴
npm install

# 開發模式運行
npm run tauri:dev

# 構建發布版本
npm run tauri build
```

構建完成後，安裝包會在 `src-tauri/target/release/bundle/` 目錄中。

## 故障排除

**Q: 程式無法啟動？**
A: 請檢查 API 憑證是否正確設定。可通過設置窗口或環境變數設置。

**Q: 無法錄音？**
A: 請確認系統有可用的麥克風設備，且應用程式有權限存取。

**Q: 識別結果不正確？**
A: 嘗試切換不同的 ASR 引擎，或在安靜環境下使用。

**Q: 如何查看詳細執行訊息？**
A: 啟動程式時加上 `--debug` 參數可顯示控制台輸出：
- Windows: `Voice Input Tray.exe --debug`
- 開發模式: `npm run tauri:dev -- --debug`

## 版本資訊

| 版本 | 發布日期 | 變更說明 |
|------|----------|----------|
| v0.1.0 | 2026-03-24 | Tauri v2 跨平台版本，GUI 設置界面，開機自啟功能 |
| v0.0.2 | 2026-03-23 | 優化貼上延遲，新增引擎選單切換功能 |
| v0.0.1 | 2026-03-20 | 初始發布版本 |

## 授權

本軟體尚未開源，但可免費使用。
版權所有 © 2026 朱國棟 (Charlie Chu)。

未經授權，禁止複製、修改、散布或以任何形式使用本軟體及其相關文件。

---
**聯絡信箱**：charliechu1688@gmail.com
