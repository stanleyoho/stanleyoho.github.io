# GitHub Pages 部署問題解決記錄 - 2025/08/07

## 問題描述

### 1. 主要問題
- **現象**: GitHub Pages 顯示 XML 代碼而不是正常的網頁內容
- **錯誤訊息**: "This XML file does not appear to have any style information..."
- **影響範圍**: 整個網站無法正常訪問，用戶看到的是 RSS feed 的 XML 內容

### 2. 次要問題
- Git push 操作失敗，出現認證錯誤
- 網站本地運行正常，但部署到 GitHub Pages 後出現問題

## 問題分析思路

### 第一步：確認問題範圍
1. **本地測試**: 先確認本地 `hugo server` 運行正常
2. **部署差異**: 識別本地和線上環境的差異點
3. **時間點分析**: 確認何時開始出現問題（之前是正常的）

### 第二步：檢查部署配置
1. **GitHub Actions 檢查**: 查看 `.github/workflows/` 目錄
2. **配置文件分析**: 檢查 `hugo.toml` 設置
3. **構建輸出**: 查看部署日誌和輸出文件

## 解決步驟詳解

### 步驟 1: 發現根本原因

**檢查命令:**
```bash
ls -la .github/workflows/
```

**發現問題:**
- 存在兩個衝突的 workflow 文件：
  - `deploy.yml`: 正確的 Hugo 部署配置
  - `man.yaml`: 錯誤的 notablog 部署配置（每5分鐘運行一次）

**分析思路:**
- `man.yaml` 使用 notablog 框架，會生成不同的輸出格式
- 該 workflow 每5分鐘自動運行，持續覆蓋 Hugo 生成的內容
- 這解釋了為什麼網站顯示 XML 而不是 HTML

### 步驟 2: 移除衝突的 Workflow

**執行操作:**
```bash
# 刪除錯誤的 workflow
rm .github/workflows/man.yaml

# 確認刪除
git status
git add .github/workflows/
```

**決策邏輯:**
- `man.yaml` 使用錯誤的框架（notablog vs Hugo）
- 頻繁運行會持續覆蓋正確的部署
- 保留 `deploy.yml` 作為唯一的部署方式

### 步驟 3: 更新現有 Workflow

**問題:** `deploy.yml` 使用已棄用的 Actions 版本

**修復命令:**
```yaml
# 從：
- uses: actions/upload-pages-artifact@v2
- uses: actions/deploy-pages@v3

# 更新到：
- uses: actions/upload-pages-artifact@v3  
- uses: actions/deploy-pages@v4
```

**技術考量:**
- GitHub Actions 會警告棄用的版本
- 新版本提供更好的穩定性和安全性
- 避免未來的兼容性問題

### 步驟 4: 解決 Git 認證問題

**問題現象:**
```
git push 失敗，顯示認證錯誤
cached credentials for wrong user (JenniferCheng1027)
```

**診斷過程:**
1. 檢查 git 配置：`git config --list`
2. 確認遠程倉庫：`git remote -v`
3. 識別認證緩存問題

**解決方案:**
1. **清除緩存憑證** (如果需要):
   ```bash
   # macOS
   git config --global --unset credential.helper
   # 或清除 Keychain 中的 GitHub 憑證
   ```

2. **重新配置認證:**
   - 使用正確的 GitHub 用戶名
   - 設置 Personal Access Token (如果使用 HTTPS)
   - 或配置 SSH 密鑰 (如果使用 SSH)

### 步驟 5: 測試和驗證

**測試命令順序:**
```bash
# 1. 提交所有更改
git add .
git commit -m "Fix deployment conflicts and update workflow"

# 2. 推送到遠程倉庫
git push origin main

# 3. 監控 GitHub Actions
# 在 GitHub 倉庫頁面 > Actions 標籤頁觀察

# 4. 驗證網站
# 訪問 https://stanleyoho.github.io 確認正常顯示
```

## 技術要點總結

### 1. GitHub Actions 最佳實踐
- **單一職責**: 每個倉庫只保持一個主要的部署 workflow
- **版本管理**: 定期更新 Actions 到最新穩定版本
- **命名規範**: 使用清晰的 workflow 文件名

### 2. Hugo 部署配置關鍵點
```yaml
# 正確的 Hugo 部署配置要素
- name: Setup Hugo
  uses: peaceiris/actions-hugo@v2
  with:
    hugo-version: '0.148.1'    # 指定版本
    extended: true             # 支援 SCSS

- name: Build with Hugo
  env:
    HUGO_ENVIRONMENT: production
    HUGO_ENV: production       # 生產環境設定
  run: |
    hugo \
      --gc \                   # 垃圾回收
      --minify \              # 最小化輸出
      --baseURL "${{ steps.pages.outputs.base_url }}/"
```

### 3. 問題診斷方法論
1. **症狀 → 原因**: XML 顯示 → 錯誤的內容生成器
2. **時間分析**: 何時開始出現 → 找到變更點
3. **環境對比**: 本地 vs 線上 → 識別差異
4. **日誌分析**: GitHub Actions 執行記錄
5. **逐步排除**: 從最可能的原因開始

## 預防措施

### 1. 定期維護檢查清單
- [ ] 每月檢查 GitHub Actions 版本更新
- [ ] 監控部署狀態和網站可用性
- [ ] 備份重要配置文件

### 2. 開發最佳實踐
- [ ] 在添加新的 workflow 前，檢查現有配置
- [ ] 使用有意義的 commit 訊息
- [ ] 定期清理不需要的文件和配置

### 3. 故障應對流程
1. **記錄問題**: 詳細描述症狀和錯誤訊息
2. **本地驗證**: 先在本地環境確認問題
3. **逐步回滾**: 從最近的更改開始檢查
4. **文檔記錄**: 記錄解決過程供未來參考

## 學習要點

### 1. 系統性思考
- 問題可能有多個層面（部署 + 認證）
- 解決一個問題時，要考慮對其他部分的影響
- 保持變更的原子性和可回滾性

### 2. 工具熟練度
- **Git**: 版本控制和協作基礎
- **GitHub Actions**: 自動化部署和 CI/CD
- **Hugo**: 靜態網站生成器的配置和優化

### 3. 問題解決技巧
- **日誌分析**: 學會讀懂錯誤訊息和系統日誌
- **版本對比**: 使用 git diff 和 git log 追蹤變更
- **環境隔離**: 區分本地、測試和生產環境的差異

---

*這次問題解決讓我們學到了 GitHub Actions workflow 衝突的診斷和修復，以及 Git 認證問題的處理方法。重要的是建立系統性的問題解決思路，而不僅僅是記住具體的命令。*