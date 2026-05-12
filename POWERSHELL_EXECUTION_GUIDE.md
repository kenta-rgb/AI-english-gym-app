# 🚀 PowerShell スクリプト実行ガイド

## 準備物

生成したトークンを手元に用意してください：
```
ghp_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

---

## 📋 実行手順

### Step 1: PowerShell を開く

1. **Windows キー** を押す
2. **powershell** と入力
3. **Windows PowerShell** をクリック
   （または右クリック → 「管理者として実行」）

### Step 2: 作業ディレクトリに移動

以下のコマンドをコピーして PowerShell に貼り付け、Enter を押す：

```powershell
cd 'c:\Users\ユーザー\.vscode\extensions\github.codespaces-1.18.12'
```

確認：以下のようにプロンプトが表示されれば OK
```
c:\Users\ユーザー\.vscode\extensions\github.codespaces-1.18.12>
```

### Step 3: 変数を設定

以下のコマンドをコピーして実行します。**かっこ内を自分の値に置き換えてください：**

```powershell
# トークンを設定（ghp_xxx...の部分を自分のトークンに置き換え）
$PAT = "ghp_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"

# GitHub ユーザー名を設定（your-github-username を自分のユーザー名に置き換え）
$USERNAME = "your-github-username"

# リポジトリ名（そのまま）
$REPO_NAME = "AI-english-gym-app"

# リポジトリ説明（そのまま）
$REPO_DESC = "AI英会話ジム - 英会話アプリを安全にデプロイし、セキュリティリスク確認"
```

### Step 4: 実行スクリプト

以下の**全体**をコピーして PowerShell に貼り付け、**Enter を 1 回** だけ押す：

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
Write-Host "1. 上記 URL にアクセスして確認してください"
Write-Host "2. Google Search Console に登録してください"
Write-Host "3. sitemap.xml をサブミットしてください"
```

---

## ⚠️ トラブルシューティング

### エラー 1: トークンが無効

```
AuthenticationError: Bad credentials
```

**対応：**
- ✓ トークンをコピーするときに余分なスペースがないか確認
- ✓ GitHub で PAT の有効期限が切れていないか確認
- ✓ PAT に `repo` 権限があるか確認

### エラー 2: リポジトリがすでに存在

```
Repository creation failed: name already exists
```

**対応：**
- ✓ `$REPO_NAME` を別の名前に変更
  ```powershell
  $REPO_NAME = "AI-english-gym-app-v2"
  ```
- ✓ または GitHub でリポジトリを削除してから実行

### エラー 3: git の認証エラー

```
fatal: authentication failed
```

**対応：**
```powershell
# git 認証をリセット
git credential reject

# または、リモートを設定し直し
git remote remove origin
git remote add origin https://$USERNAME:$PAT@github.com/$USERNAME/$REPO_NAME.git
git push -u origin main
```

### エラー 4: GitHub Pages 有効化に失敗

```
422: Validation failed
```

**対応：**
- ✓ 5 分程度待ってから再度実行
- ✓ GitHub リポジトリページの Settings → Pages で手動確認

---

## ✅ 成功時の表示

スクリプトが正常に実行されると、以下のように表示されます：

```
Creating GitHub repository...
✓ Repository created: https://github.com/YOUR_USERNAME/AI-english-gym-app.git

Configuring git remote and pushing...
✓ Pushed to GitHub successfully!

Enabling GitHub Pages...
✓ GitHub Pages enabled!

========================================
✅ デプロイ完了！
========================================

GitHub リポジトリ: https://github.com/YOUR_USERNAME/AI-english-gym-app
GitHub Pages URL: https://YOUR_USERNAME.github.io/AI-english-gym-app/

📝 次のステップ:
1. 上記 URL にアクセスして確認してください
2. Google Search Console に登録してください
3. sitemap.xml をサブミットしてください
```

---

## 🔍 実行後の確認

### 1. GitHub リポジトリが作成されたか確認

ブラウザで以下にアクセス：
```
https://github.com/YOUR_USERNAME/AI-english-gym-app
```

### 2. GitHub Pages が有効化されたか確認

ブラウザで以下にアクセス：
```
https://YOUR_USERNAME.github.io/AI-english-gym-app/
```

README.md が表示されれば成功！

### 3. SEO ファイルが公開されているか確認

```
https://YOUR_USERNAME.github.io/AI-english-gym-app/robots.txt
https://YOUR_USERNAME.github.io/AI-english-gym-app/sitemap.xml
```

両方アクセス可能なら成功！

---

## 📝 PowerShell コピペ用

### ステップ 3: 変数設定（コピペ用）

```powershell
$PAT = "ghp_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
$USERNAME = "your-github-username"
$REPO_NAME = "AI-english-gym-app"
$REPO_DESC = "AI英会話ジム - 英会話アプリを安全にデプロイし、セキュリティリスク確認"
```

**⚠️ 必ず置き換えてください：**
- `ghp_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx` → 自分のトークン
- `your-github-username` → 自分の GitHub ユーザー名

---

## 🎯 早期確認ポイント

実行中に以下が表示されたら成功しています：

```
✓ Repository created: https://github.com/YOUR_USERNAME/AI-english-gym-app.git
✓ Pushed to GitHub successfully!
✓ GitHub Pages enabled!
```

---

**これで AI英会話ジムが WEB 上に公開されます！**
