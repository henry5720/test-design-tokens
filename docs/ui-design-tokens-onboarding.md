# UI 設計師上手指南：Design Tokens 自動化同步

本指南將引導您如何使用 Figma 的 **Tokens Studio** 插件，將設計變數（Tokens）自動同步到前端代碼庫。

---

## 🚀 快速設定步驟

### 1. 安裝與啟動
1. 在 Figma 中搜尋並安裝插件：**Tokens Studio for Figma**。
2. 開啟插件。

### 2. 連接 GitHub 儲存庫
為了實現自動同步，我們需要將插件連接到指定的 GitHub Repository。

1. 在 Tokens Studio 中點擊右上角的 **Settings** (齒輪圖示)。
2. 在 **Sync Providers** 中點擊 **Add New** -> 選擇 **GitHub**。
3. 填入以下參數：
    - **Name**: `TeamSync Tokens`
    - **Personal Access Token**: (請貼入您的 GitHub PAT，權限需包含 `repo`)
    - **Repository**: `ShuChenAI/teamsync-ui-design-token`
    - **Branch**: `main`
    - **File Path**: `tokens.json`
4. 點擊 **Save Config**。

### 3. 管理員設定 (GitHub Secrets)
為了讓 CI 能跨倉庫讀取私有的 Token 庫，管理員需完成以下設定：
1. 建立一個具備 `repo` 讀取權限的 **Personal Access Token (PAT)**。
2. 前往 `teamsync-frontend` 儲存庫的 **Settings > Secrets and variables > Actions**。
3. 點擊 **New repository secret**：
   - **Name**: `PAT_TOKEN`
   - **Value**: (貼入產生的 Token)

---


## 🎨 Token 管理規範

為了確保前端能正確讀取，請遵循以下命名層級：

| 類型 | 命名範例 | 說明 |
| :--- | :--- | :--- |
| **顏色** | `color.primary.500` | 主色調，數字代表深淺 |
| **間距** | `spacing.md` | 元件內外距 |
| **圓角** | `borderRadius.default` | 預設 6px |
| **字體** | `fontSize.base` | 預設 14px |

---

## 🔄 同步流程 (Figma -> Front-end)

當您在 Figma 修改了 Token 後：

1. **Push 變更**:
   - 在 Tokens Studio 點擊 **Sync** 頁籤。
   - 點擊 **Push** 並填寫變更描述（例如：`update: 修改主色調為綠色`）。
2. **自動觸發 Action**:
   - 前端 Repo (`teamsync-frontend`) 會自動偵測變更並建立一個 Pull Request。
3. **前端審核**:
   - 工程師審核 PR 並 Merge 後，變更將在 10 分鐘內反映在 **Storybook** 上。

---

## 📺 驗收工具：Storybook
您可以在以下網址查看組件的實際渲染效果，對照設計稿：
🔗 [Storybook 連結](https://shuchenai.github.io/teamsync-frontend/)

---

**遇到問題？** 
請聯繫前端工程師或在 GitHub 上提出 Issue。