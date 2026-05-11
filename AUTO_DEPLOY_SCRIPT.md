# GitHub Auto-Deploy Script for AI-english-gym-app

## 📖 使用方法

このスクリプトは、以下を自動実行します：
1. GitHub リポジトリを作成
2. ローカルの git を GitHub に push
3. GitHub Pages を有効化
4. Google 検索対応設定を完了

---

## 🚀 実行方法

### 方法 A: PAT を持っている場合（推奨）

```powershell
# 以下を PowerShell で実行

# 変数を設定
$PAT = "ghp_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"  # GitHub PAT を貼り付け
$USERNAME = "your-github-username"              # あなたの GitHub ユーザー名
$REPO_NAME = "AI-english-gym-app"               # リポジトリ名（デフォルト）
$REPO_DESC = "AI英会話ジム - 英会話アプリを安全にデプロイし、セキュリティリスク確認"

# ディレクトリに移動
cd 'c:\Users\ユーザー\.vscode\extensions\github.codespaces-1.18.12'

# リポジトリ作成 API を実行
$headers = @{
    "Authorization" = "token $PAT"
    "Accept" = "application/vnd.github.v3+json"
}

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
Write-Host "1. GitHub Pages がデプロイされるまで数分待ってください"
Write-Host "2. GitHub Pages URL にアクセスして確認してください"
Write-Host "3. Google Search Console に登録してください"
Write-Host "4. sitemap.xml をサブミットしてください"
Write-Host ""
Write-Host "🔗 Google Search Console: https://search.google.com/search-console" -ForegroundColor Yellow
```

### 方法 B: GitHub CLI を使用する場合

```powershell
# 事前に gh コマンドでログイン
gh auth login

# リポジトリ作成
gh repo create AI-english-gym-app --public --description "AI英会話ジム - 英会話アプリを安全にデプロイ" --remote=origin --source=. --remote-name=origin --push

# GitHub Pages 有効化
gh repo edit AI-english-gym-app --enable-wiki=false --enable-projects=false
```

### 方法 C: PAT を持っていない場合（手動）

1. [CREATE_GITHUB_PAT.md](./CREATE_GITHUB_PAT.md) に従って PAT を作成
2. 上記「方法 A」を実行

---

## 📊 自動実行内容の説明

### 作成される項目

| 項目 | 内容 | 自動化による変更点 |
|------|------|------------------|
| **リポジトリ作成** | `AI-english-gym-app` | GitHub API で自動作成 |
| **リポジトリ設定** | Public、Issues 有効 | API で自動設定 |
| **リモート登録** | `origin` ブランチ | git で自動設定 |
| **Push** | main ブランチ | git push で自動アップロード |
| **GitHub Pages** | `/` パス、main ブランチ | API で自動有効化 |
| **説明** | 日本語説明付き | リポジトリ説明に自動設定 |

### 変更されるファイル

以下のファイルには**変更なし**（既に設定済み）：
- ✓ `_config.yml` - Jekyll 設定済み
- ✓ `sitemap.xml` - SEO 対応済み
- ✓ `robots.txt` - クローラー許可設定済み
- ✓ `.github/skills/AI英会話ジム/SKILL.md` - メタデータ設定済み

---

## ⏱️ 完了後の確認

### 数分後に確認すること

```bash
# 1. GitHub Pages が生成されているか確認
https://YOUR_USERNAME.github.io/AI-english-gym-app/

# 2. リポジトリが GitHub に表示されているか確認
https://github.com/YOUR_USERNAME/AI-english-gym-app

# 3. sitemap.xml にアクセス可能か確認
https://YOUR_USERNAME.github.io/AI-english-gym-app/sitemap.xml

# 4. robots.txt にアクセス可能か確認
https://YOUR_USERNAME.github.io/AI-english-gym-app/robots.txt
```

---

## 🚨 トラブルシューティング

### GitHub Pages が表示されない場合

1. GitHub リポジトリの **Settings** → **Pages** を確認
2. Source が `main` ブランチに設定されているか確認
3. 5〜10分待ってから再度確認

### Push に失敗する場合

```powershell
# 既存のリモートをリセット
git remote remove origin
git remote add origin https://YOUR_USERNAME:TOKEN@github.com/YOUR_USERNAME/AI-english-gym-app.git
git push -u origin main
```

### API エラーが出る場合

- ✓ PAT が正しくコピーされているか確認
- ✓ ユーザー名が正しいか確認
- ✓ PAT の権限が `repo` を含んでいるか確認

---

**実行後、このスクリプトの結果を教えてください。自動化がすべて成功したか確認します。**
