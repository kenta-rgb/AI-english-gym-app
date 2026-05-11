# GitHub Personal Access Token (PAT) 作成ガイド

GitHub に自動で push して GitHub Pages を有効化するには、Personal Access Token が必要です。

## 📋 PAT の作成手順

### Step 1: GitHub にログイン
[GitHub](https://github.com/login) にアクセス

### Step 2: 設定に移動
1. 右上のプロフィールアイコンをクリック
2. **Settings** をクリック

### Step 3: Developer settings に移動
左メニューから **Developer settings** をクリック

### Step 4: Personal access tokens を選択
1. **Personal access tokens** セクションで **Tokens (classic)** をクリック
   （または **Fine-grained tokens** でより安全な設定も可能）

### Step 5: 新しいトークンを生成
**Generate new token (classic)** をクリック

### Step 6: トークン設定

**Note（トークンの説明）:**
```
GitHub Auto-Deploy Token for AI-english-gym-app
```

**Expiration（有効期限）:**
- **90 days** を推奨（セキュリティの観点から定期更新推奨）
- 必要に応じて **No expiration** も選択可能

**Scopes（権限範囲）:**
以下をチェック：
- ✅ `repo` - リポジトリへの完全アクセス
  - ✅ `repo:status` - リポジトリのステータス
  - ✅ `repo_deployment` - デプロイメント
  - ✅ `public_repo` - 公開リポジトリ
- ✅ `workflow` - GitHub Actions ワークフロー

### Step 7: トークン生成
**Generate token** をクリック

### Step 8: トークンをコピー
⚠️ **重要：このページを離れるとトークンは二度と表示されません**

```
ghp_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

このトークンをコピーして保存してください。

---

## 🔒 セキュリティ上の注意

- ✅ トークンは絶対に git リポジトリに commit しないこと
- ✅ 不要になったら GitHub Settings で削除すること
- ✅ 定期的に有効期限を更新すること
- ✅ リポジトリを公開する場合、トークンは環境変数で管理すること

---

## ✅ 準備完了後

PAT を作成したら、以下のコマンドでこのスクリプトに提供してください：

```powershell
# PowerShell で実行
$PAT = "ghp_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"  # ← 作成した PAT を貼り付け
$USERNAME = "your-github-username"             # ← あなたの GitHub ユーザー名

# 自動デプロイスクリプトを実行
# （別途提供されます）
```

---

## 🚀 PAT 不要の代替方法（推奨度：低）

GitHub CLI (`gh` コマンド) を使用する場合は、以下でも可能：

```powershell
gh auth login
# → 対話的にログイン（ブラウザで認証）
```

---

**質問や問題がある場合は、GitHub の公式ドキュメントを参照してください：**
https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token
