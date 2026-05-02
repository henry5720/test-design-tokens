# TeamSync Design Tokens (Test)

這是 TeamSync 專案的設計變數 (Design Tokens) 來源。

## 🛠 工具整合
- **Figma 插件**: [Tokens Studio for Figma](https://tokens.studio/)
- **同步對象**: `teamsync-frontend` 儲存庫

## 命名規範
我們採用 Ant Design 5.0 的 Token 結構：
- `color`: 包含 primary, success, warning, error, neutral 等。
- `borderRadius`: 元件圓角。
- `spacing`: 間距系統。

## 同步流程
1. 在 Figma 修改 Token。
2. 使用 Tokens Studio 點擊 "Push" 到此儲存庫。
3. 前端專案的 GitHub Actions 將自動偵測變更並開啟 PR。
