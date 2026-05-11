# UI/UX 設計師操作手冊：Design Tokens 管理指南

> **目標讀者：** UI/UX 設計師  
> **前置需求：** Figma 帳號、GitHub 帳號  
> **預計學習時間：** 2 小時

---

## 📋 目錄

1. [什麼是 Design Tokens？](#什麼是-design-tokens)
2. [工具安裝與設定](#工具安裝與設定)
3. [建立 Design Tokens](#建立-design-tokens)
4. [將 Tokens 套用到設計稿](#將-tokens-套用到設計稿)
5. [同步到前端（Push to GitHub）](#同步到前端push-to-github)
6. [在 Storybook 驗收](#在-storybook-驗收)
7. [常見問題](#常見問題)

---

## 什麼是 Design Tokens？

### 簡單來說

**Design Tokens 是設計與代碼之間的翻譯機**。

以前：
```
你在 Figma: "主色是 #67C47B"
        ↓
      口頭告訴工程師
        ↓
工程師在代碼: "好，我改成 #67C47B"
```

現在：
```
你在 Figma: 定義 token "primary-500 = #67C47B"
        ↓
    自動同步到 GitHub
        ↓
前端代碼: 自動變成 #67C47B
```

### 為什麼需要？

**問題：** 你改了 Figma 的顏色，但代碼裡的組件沒有同步改，導致實際網站跟設計稿不一致。

**解決：** Design Tokens 讓顏色、字體、間距等「設計變數」自動同步，不再需要手動告訴工程師。

### Tokens 可以管理什麼？

| Token 類型 | 範例 | 用途 |
|-----------|------|------|
| **顏色** | `#67C47B`, `rgba(103, 196, 123, 0.5)` | 主色、次色、灰階 |
| **間距** | `16px`, `1rem` | 元件內距、外距 |
| **字體大小** | `14px`, `1rem` | 標題、內文 |
| **字體粗細** | `400`, `600`, `bold` | 粗體、細體 |
| **圓角** | `6px`, `50%` | 按鈕圓角、卡片圓角 |
| **陰影** | `0 2px 8px rgba(0,0,0,0.15)` | 按鈕陰影、卡片陰影 |

---

## 工具安裝與設定

### 第一步：安裝 Tokens Studio Plugin

1. **打開 Figma**
   - 進入任何 Figma 檔案

2. **打開 Plugins 面板**
   - 點擊上方選單：`Plugins` → `Browse plugins in Community`

3. **搜尋並安裝**
   - 搜尋：`Tokens Studio`
   - 點擊 `Install`

4. **啟動 Plugin**
   - 點擊上方選單：`Plugins` → `Tokens Studio for Figma`
   - 第一次啟動會看到歡迎畫面

### 第二步：連接 GitHub

**為什麼需要連接 GitHub？**
- Tokens 需要存放在 GitHub，前端才能自動抓取
- 你每次改 Tokens，會自動 Push 到 GitHub，觸發自動同步

**操作步驟：**

1. **在 Tokens Studio 中點擊 Settings**
   - 右上角齒輪圖示

2. **選擇 Sync Provider**
   - 點擊 `Add new` → 選擇 `GitHub`

3. **授權 GitHub**
   - 點擊 `Authenticate with GitHub`
   - 登入你的 GitHub 帳號
   - 授權 Tokens Studio 存取

4. **設定 Repository**
    - **Repository:** `ShuChenAI/teamsync-ui-design-token`
    - **Branch:** `main`
    - **File Path:** `tokens.json`
    - **Personal Access Token:** 請向前端工程師索取（或自己建立，見附錄 A）


5. **測試連接**
   - 點擊 `Test Connection`
   - 看到 ✅ 表示成功

---

## 建立 Design Tokens

### 第一步：建立 Token Set

**什麼是 Token Set？**
- 一組相關的 tokens 集合，例如「顏色」、「間距」

**操作步驟：**

1. **打開 Tokens Studio**
   - `Plugins` → `Tokens Studio for Figma`

2. **建立新的 Set**
   - 點擊左上角 `+` → `Create new set`
   - 命名：`Core`（核心設計變數）

3. **選擇 Set**
   - 確保 `Core` 是啟用狀態（開關打開）

### 第二步：建立顏色 Tokens

**範例：建立主色 Token**

1. **點擊 Colors 分頁**
   - 左側選單選擇 `Colors`

2. **新增 Token**
   - 點擊 `+` → `Add token`

3. **填寫資訊**
   ```
   名稱: color.primary.500
   值: #67C47B
   類型: Color (自動偵測)
   ```

4. **完成**
   - 點擊 `Create`

**建立完整的顏色系統：**

```
color.primary.500 = #67C47B  (主色)
color.primary.600 = #209D14  (主色 hover)
color.gray.500 = #B5BEC7     (灰色)
color.gray.600 = #717D8B     (深灰色)
color.danger.500 = #FF7070   (錯誤紅色)
```

**💡 命名技巧：**
- 使用 `.` 分隔層級：`color.primary.500`
- 數字代表深淺（500 是基準，600 是較深）
- 參考現有的 ScComponents 顏色系統

### 第三步：建立間距 Tokens

1. **點擊 Spacing 分頁**

2. **新增 Tokens**
   ```
   spacing.xs = 4px
   spacing.sm = 8px
   spacing.md = 16px
   spacing.lg = 24px
   spacing.xl = 32px
   ```

### 第四步：建立其他 Tokens

**字體大小：**
```
fontSize.sm = 12px
fontSize.base = 14px
fontSize.lg = 16px
fontSize.xl = 20px
```

**圓角：**
```
borderRadius.default = 6px
borderRadius.full = 50%
```

**陰影：**
```
shadow.button = 0 2px 4px rgba(0, 0, 0, 0.1)
shadow.card = 0 4px 8px rgba(0, 0, 0, 0.15)
```

---

## 將 Tokens 套用到設計稿

### 方法一：手動套用 Token

1. **選中元件**
   - 例如選中一個按鈕

2. **右鍵 → Apply token**
   - 或使用快捷鍵（Tokens Studio 會顯示）

3. **選擇 Token**
   - 選擇 `color.primary.500`

4. **完成**
   - 按鈕顏色變成 #67C47B
   - 未來改 token，這個按鈕會自動更新

### 方法二：在 Tokens Studio 中套用

1. **選中元件**

2. **在 Tokens Studio 的 Inspect 分頁**
   - 可以看到目前元件的顏色、間距等

3. **點擊顏色欄位旁的 token 圖示**
   - 選擇要套用的 token

### 💡 最佳實踐

**好的做法：**
```
✅ 使用 token: color.primary.500
✅ 使用 token: spacing.md
```

**不好的做法：**
```
❌ 直接填顏色: #67C47B
❌ 直接填間距: 16px
```

**為什麼？**
- 使用 token 才能自動同步
- 直接填值，改 token 不會影響到這個元件

---

## 同步到前端（Push to GitHub）

### 每次改完 Tokens 後的操作

1. **確認 Tokens 正確**
   - 在 Tokens Studio 檢查所有 tokens

2. **Push to GitHub**
   - 點擊右上角 `Push` 按鈕
   - 或點擊 `Sync` → `Push changes`

3. **填寫 Commit Message**
   ```
   範例：
   - "更新主色為 #5AB86A"
   - "新增按鈕陰影 token"
   - "調整間距系統"
   ```

4. **確認 Push**
   - 點擊 `Push`
   - 看到 ✅ 表示成功

### 自動化流程

**你 Push 到 GitHub 後會發生什麼？**

```
1. GitHub 收到 tokens.json
   ↓
2. 自動觸發 GitHub Actions（約 1 分鐘）
   ↓
3. 轉換成前端代碼（CSS Variables, Tailwind Config）
   ↓
4. 建立 PR 給前端工程師審查
   ↓
5. 工程師 Merge PR（約 2-5 分鐘）
   ↓
6. Storybook 自動部署新版本
   ↓
7. 你可以在 Storybook 看到更新後的組件
```

**總耗時：約 10 分鐘**

---

## 在 Storybook 驗收

### 什麼是 Storybook？

**Storybook 是組件的「展示櫥窗」**

- 可以看到所有前端組件
- 可以看到組件的各種狀態（hover, disabled, loading）
- 可以對照 Figma 設計稿
- 可以調整 props 即時預覽

### 如何訪問 Storybook？

**URL：** `https://shuchenai.github.io/teamsync-frontend/`

（請加入書籤，方便隨時訪問）

### Storybook 介面說明

```
左側欄：組件列表
├─ ScComponents/
│  ├─ Basic/
│  │  ├─ ScButton          ← 點擊這裡
│  │  └─ ScInput
│  └─ Form/
│     └─ ScForm
```

```
中間區域：組件預覽
- 可以看到組件實際渲染的樣子
- 可以互動（點擊、hover）
```

```
右側欄：
├─ Controls      ← 調整 props
├─ Actions       ← 查看事件
└─ Design        ← 查看 Figma 設計稿（重點！）
```

### 驗收流程

**你改了 Figma Tokens 後：**

1. **等待約 10 分鐘**
   - 前端工程師 merge PR
   - Storybook 自動部署

2. **訪問 Storybook**
   - 打開 `https://shuchenai.github.io/teamsync-frontend/`

3. **檢查受影響的組件**
   - 例如你改了 `color.primary.500`
   - 去看 ScButton, ScInput 等用到這個顏色的組件

4. **對照 Figma**
   - 點擊右側的 `Design` tab
   - 可以看到 Figma 設計稿
   - 對照組件實際渲染結果

5. **驗收結果**
   - ✅ 一致：沒問題
   - ❌ 不一致：在 GitHub 提 issue 給前端工程師

### 💡 驗收檢查清單

- [ ] 顏色是否正確？
- [ ] 間距是否正確？
- [ ] 圓角是否正確？
- [ ] hover 狀態是否正確？
- [ ] disabled 狀態是否正確？
- [ ] 長文字是否會破版？
- [ ] 小螢幕是否正常？

---

## 常見問題

### Q1: 我改了 Token，但 Storybook 沒更新？

**可能原因：**

1. **還沒 Push 到 GitHub**
   - 檢查：Tokens Studio 右上角是否有未同步的標記
   - 解決：點擊 `Push`

2. **GitHub Actions 還在跑**
   - 檢查：訪問 `https://github.com/ShuChenAI/teamsync-frontend/actions`
   - 等待：通常 1-2 分鐘完成

3. **前端工程師還沒 merge PR**
   - 檢查：訪問 `https://github.com/ShuChenAI/teamsync-frontend/pulls`
   - 催促：私訊工程師 😄

4. **瀏覽器快取**
   - 解決：按 `Ctrl + Shift + R`（Windows）或 `Cmd + Shift + R`（Mac）強制重新整理

### Q2: 我不小心 Push 錯了，怎麼辦？

**不用擔心！**

- 所有變更都會建立 PR，工程師會審查
- 如果發現錯誤，工程師會拒絕 merge
- 你可以再 Push 一次正確的版本

### Q3: Token 命名有規範嗎？

**推薦規範：**

```
✅ 語義化命名
color.primary.500    (主色)
color.danger.500     (錯誤色)
spacing.md           (中等間距)

❌ 不推薦
color.green          (如果之後主色改成藍色呢？)
spacing.16px         (寫死數值)
```

**分層結構：**
```
{類型}.{用途}.{變體}
  ↓      ↓      ↓
color.primary.500
```

### Q4: 我可以在 Tokens Studio 看到前端目前用的 Token 嗎？

**可以！**

1. **Pull from GitHub**
   - Tokens Studio → Sync → `Pull changes`

2. **查看 Tokens**
   - 所有 tokens 會更新成前端目前使用的版本

**💡 建議：**
- 每次開始工作前，先 Pull 一次
- 確保你看到的是最新版本

### Q5: Figma Variables vs Tokens Studio，該用哪個？

**Tokens Studio（推薦）**
- ✅ 免費
- ✅ 可以直接 Push to GitHub
- ✅ 支援更多 token 類型
- ❌ 需要安裝 Plugin

**Figma Variables**
- ✅ Figma 原生功能
- ❌ 需要 Professional Plan ($15/月)
- ❌ 需要手動匯出 JSON

**結論：** 使用 Tokens Studio

### Q6: 我改了 Token，但有些組件沒變？

**可能原因：**

1. **組件沒有使用 Token**
   - 檢查：這個組件是直接填顏色，而不是用 token
   - 解決：請工程師修改組件改用 token

2. **組件用了不同的 Token**
   - 檢查：這個組件用的是 `color.secondary.500` 而不是 `color.primary.500`
   - 解決：確認應該用哪個 token

### Q7: 我可以刪除 Token 嗎？

**可以，但要小心！**

1. **先檢查有沒有組件在用**
   - 在 Figma 搜尋這個 token 名稱
   - 在 Storybook 看有沒有組件用到

2. **確認沒有使用後再刪除**
   - 刪除 token
   - Push to GitHub

3. **通知工程師**
   - 讓工程師知道你刪除了某個 token
   - 工程師會檢查代碼中是否還有使用

### Q8: Token 可以參考其他 Token 嗎？

**可以！這叫做 Alias**

```
範例：
color.primary.500 = #67C47B
color.button.primary = {color.primary.500}  ← 參考另一個 token
```

**好處：**
- 語義化更清楚
- 改一個地方，其他地方自動更新

**在 Tokens Studio 中操作：**
1. 建立新 token
2. 值的欄位輸入：`{color.primary.500}`
3. Tokens Studio 會自動識別為 alias

---

## 附錄

### A. 建立 GitHub Personal Access Token

**為什麼需要？**
- **設計師端**：Tokens Studio 插件需要權限才能 Push 到 GitHub。
- **CI/CD 端**：GitHub Actions 需要 `PAT_TOKEN` (Secret) 才能跨倉庫讀取私有的 `teamsync-ui-design-token`。

**步驟：**

1. **登入 GitHub**
   - 訪問 `https://github.com`

2. **進入 Settings**
   - 右上角頭像 → `Settings`

3. **進入 Developer settings**
   - 左側欄最下方 → `Developer settings`

4. **建立 Token**
   - `Personal access tokens` → `Tokens (classic)` → `Generate new token`

5. **設定權限**
   ```
   Note: Tokens Studio for Figma
   Expiration: No expiration（或選擇適當的期限）
   Scopes: 
   - ✅ repo (全部勾選)
   ```

6. **生成並複製**
   - 點擊 `Generate token`
   - **立刻複製 token**（離開頁面後就看不到了）

7. **貼到 Tokens Studio**
   - 在 Tokens Studio 的 GitHub 設定中貼上

### B. 參考資源

- **Tokens Studio 官方文件：** https://docs.tokens.studio/
- **Storybook：** https://shuchenai.github.io/teamsync-frontend/
- **GitHub Repo (Tokens)：** https://github.com/ShuChenAI/teamsync-ui-design-token
- **GitHub Repo (Frontend)：** https://github.com/ShuChenAI/teamsync-frontend

### C. 聯絡資訊

**遇到問題？**

1. **技術問題：** 在 GitHub 提 issue
2. **操作問題：** 私訊前端工程師
3. **緊急問題：** 直接找前端工程師

---

## 快速參考卡

### 日常工作流程

```
1. 打開 Tokens Studio
   ↓
2. Pull latest changes (確保是最新版本)
   ↓
3. 修改或新增 Tokens
   ↓
4. 在 Figma 套用 Tokens 到設計稿
   ↓
5. Push to GitHub
   ↓
6. 等待 10 分鐘
   ↓
7. 在 Storybook 驗收
```

### 常用快捷鍵

| 功能 | 快捷鍵 |
|------|--------|
| 打開 Tokens Studio | (在 Figma Plugins 中設定) |
| Apply Token | 右鍵 → Apply token |
| Push to GitHub | Tokens Studio → Push |

### Token 命名範例

```
顏色：
color.primary.{500, 600, 700}
color.gray.{300, 500, 700}
color.danger.500
color.success.500

間距：
spacing.{xs, sm, md, lg, xl}

字體：
fontSize.{sm, base, lg, xl}
fontWeight.{normal, medium, bold}

其他：
borderRadius.{default, full}
shadow.{button, card}
```

---

**祝你使用愉快！🎨**

有任何問題歡迎隨時提出。
