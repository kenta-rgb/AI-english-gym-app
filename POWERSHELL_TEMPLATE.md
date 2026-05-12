# PowerShell 実行テンプレート

以下の手順に従ってコピペしてください。

---

## 🔴 Step 1: PowerShell を開く

1. **Windows キー** + **R** を押す
2. `powershell` と入力
3. **Enter** を押す

---

## 🟠 Step 2: 作業ディレクトリに移動

PowerShell に**以下をすべてコピペして**実行：

```powershell
cd 'c:\Users\ユーザー\.vscode\extensions\github.codespaces-1.18.12'
```

---

## 🟡 Step 3: トークンとユーザー名を設定

以下のテンプレートを使用してください。**かっこ内を自分の値に置き換えてから**コピペ実行：

```powershell
# ========== ここから ==========
# 【重要】以下の 2 行を自分の値に置き換えてください：

$PAT = "ghp_自分のトークンをここに貼り付け"
$USERNAME = "自分のGitHubユーザー名"

# ========== ここまで ==========

# 以下はそのまま（変更不要）
$REPO_NAME = "AI-english-gym-app"
$REPO_DESC = "AI英会話ジム - 英会話アプリを安全にデプロイし、セキュリティリスク確認"
```

**例（実際の値）：**
```powershell
$PAT = "ghp_a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6"
$USERNAME = "taro-yamada"
$REPO_NAME = "AI-english-gym-app"
$REPO_DESC = "AI英会話ジム - 英会話アプリを安全にデプロイし、セキュリティリスク確認"
```

---

## 🟢 Step 4: 実行スクリプト（全体をコピペ）

以下の**全体をすべてコピーして**、PowerShell に貼り付け、**1 回だけ Enter を押す**：

```powershell
# API 呼び出しヘッダーを設定
$headers = @{
    "Authorization" = "token $PAT"
    "Accept" = "application/vnd.github.v3+json"
}

# リポジトリ作成リクエスト
$body = @{
    "name" = $REPO_NAME
    "description" = $REPO_DESC
    "private" = $false
    "has_issues" = $true
    "has_projects" = $false
    "has_downloads" = $false
} | ConvertTo-Json

Write-Host "Creating GitHub repository..." -ForegroundColor Green
$repoResponse = Invoke-RestMethod -Uri "https://api.github.com/user/repos" `
    -Method POST `
    -Headers $headers `
    -Body $body

$repoUrl = $repoResponse.clone_url
Write-Host "✓ Repository created: $repoUrl" -ForegroundColor Green

# リモートを設定して push
Write-Host "Configuring git remote and pushing..." -ForegroundColor Green
git remote add origin $repoUrl
git branch -M main
git push -u origin main

Write-Host "✓ Pushed to GitHub successfully!" -ForegroundColor Green

# GitHub Pages 有効化
Write-Host "Enabling GitHub Pages..." -ForegroundColor Green

$pagesBody = @{
    "source" = @{
        "branch" = "main"
        "path" = "/"
    }
} | ConvertTo-Json

Invoke-RestMethod -Uri "https://api.github.com/repos/$USERNAME/$REPO_NAME/pages" `
    -Method POST `
    -Headers $headers `
    -Body $pagesBody | Out-Null

Write-Host "✓ GitHub Pages enabled!" -ForegroundColor Green

# 完了メッセージ
Write-Host ""
Write-Host "========================================" -ForegroundColor Cyan
Write-Host "✅ デプロイ完了！" -ForegroundColor Green
Write-Host "========================================" -ForegroundColor Cyan
Write-Host ""
Write-Host "GitHub リポジトリ: https://github.com/$USERNAME/$REPO_NAME" -ForegroundColor Yellow
Write-Host "GitHub Pages URL: https://$USERNAME.github.io/$REPO_NAME/" -ForegroundColor Yellow
Write-Host ""
Write-Host "📝 次のステップ:" -ForegroundColor Cyan
Write-Host "1. 上記 GitHub Pages URL にアクセスして確認してください"
Write-Host "2. Google Search Console に登録してください"
Write-Host "3. sitemap.xml をサブミットしてください"
```

---

## ✅ 成功時

以下のように表示されます：

```
Creating GitHub repository...
✓ Repository created: https://github.com/your-username/AI-english-gym-app.git

Configuring git remote and pushing...
Enumerating objects: XX, done.
Counting objects: 100% (XX/XX), done.
Compressing objects: 100% (XX/XX), done.
Writing objects: 100% (XX/XX), done.
Total XX (delta XX), reused XX (delta XX), pack-reused 0 (delta 0)
To https://github.com/your-username/AI-english-gym-app.git
 * [new branch]      main -> main
Branch 'main' set up to track remote branch 'main' from 'origin'.
✓ Pushed to GitHub successfully!

Enabling GitHub Pages...
✓ GitHub Pages enabled!

========================================
✅ デプロイ完了！
========================================

GitHub リポジトリ: https://github.com/your-username/AI-english-gym-app
GitHub Pages URL: https://your-username.github.io/AI-english-gym-app/

📝 次のステップ:
1. 上記 GitHub Pages URL にアクセスして確認してください
2. Google Search Console に登録してください
3. sitemap.xml をサブミットしてください
```

---

## 🔴 エラーが出た場合

### よくあるエラーと対応

**エラー: `Bad credentials`**
- ✓ トークンが正しくコピーされているか確認
- ✓ GitHub で PAT の有効期限を確認

**エラー: `name already exists`**
- ✓ リポジトリ名を変更
  ```powershell
  $REPO_NAME = "AI-english-gym-app-v2"
  ```

**エラー: `remote already exists`**
- ✓ git リモートをリセット
  ```powershell
  git remote remove origin
  ```
  その後、Step 4 を再実行

---

## 🎯 わかりやすく

### トークン、ユーザー名がわからない場合

1. **GitHub にログイン**: https://github.com/login
2. **トークン確認**: Settings → Developer settings → Personal access tokens (classic)
3. **ユーザー名確認**: 右上のプロフィール → 左側に表示されるユーザー名

---

## 📝 最速ガイド

```
1. PowerShell を開く
   ↓
2. cd 'c:\Users\ユーザー\.vscode\extensions\github.codespaces-1.18.12'
   ↓
3. $PAT と $USERNAME を設定
   ↓
4. 実行スクリプトをコピペして実行
   ↓
5. 完了！
```

---

**実行後、ブラウザで以下にアクセスして確認：**
```
https://your-username.github.io/AI-english-gym-app/
```

README が表示されれば成功です！
