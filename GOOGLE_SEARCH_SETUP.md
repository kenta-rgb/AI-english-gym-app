# 【重要】Google 検索対応 - 最終セットアップガイド

## 完了した部分 ✓

- ✓ ローカル Git リポジトリ作成・初期コミット完了
- ✓ README.md（詳細ドキュメント）作成
- ✓ GitHub Pages 用 _config.yml 作成
- ✓ SEO 用 robots.txt、sitemap.xml 作成
- ✓ SKILL.md とアイコン設定完了
- ✓ メタデータ（keywords）追加完了

---

## あなたが実行する必要なステップ

### Step 1: GitHub ユーザー名を変数に設定

以下の手順で、`YOUR_USERNAME` をあなたの GitHub ユーザー名に置き換えます。

```powershell
# パワーシェルで実行
$USERNAME = "your-github-username"  # ← あなたの GitHub ユーザー名に置き換え

# _config.yml を更新
(Get-Content ".\\_config.yml") -replace "your-username", $USERNAME | Set-Content ".\\_config.yml"

# sitemap.xml を更新
(Get-Content ".\\sitemap.xml") -replace "your-username", $USERNAME | Set-Content ".\\sitemap.xml"

# robots.txt を更新
(Get-Content ".\\robots.txt") -replace "your-username", $USERNAME | Set-Content ".\\robots.txt"

# git にコミット
git add _config.yml sitemap.xml robots.txt
git commit -m "Configure GitHub Pages URLs for username: $USERNAME"
```

### Step 2: GitHub にリポジトリを作成して Push

```powershell
# リモートリポジトリを追加
git remote add origin https://github.com/YOUR_USERNAME/AI-english-conversation-app.git

# ブランチ名を main に変更
git branch -M main

# GitHub に push
git push -u origin main
```

**`YOUR_USERNAME`** をあなたの GitHub ユーザー名に置き換えます。

### Step 3: GitHub Pages を有効化

1. GitHub リポジトリにアクセス
2. **Settings** → **Pages**
3. **Source**: `main` ブランチ、`/ (root)` フォルダ
4. **Save** をクリック

### Step 4: Google Search Console に登録

1. [Google Search Console](https://search.google.com/search-console) を開く
2. **URL プレフィックス** で GitHub Pages URL を登録：
   ```
   https://YOUR_USERNAME.github.io/AI-english-conversation-app/
   ```
3. 所有権を確認（HTML ファイル方式推奨）
4. サイトマップを追加：`/sitemap.xml`

### Step 5: インデックス作成をリクエスト

Google Search Console で「インデックス登録をリクエスト」をクリック

---

## 結果

✓ **24～72時間後に Google 検索で「AI英会話練習」と検索すると表示されます。**

---

## トラブルシューティング

### GitHub Pages がビルドされない場合

1. リポジトリの **Actions** タブを確認
2. ワークフロー失敗がないか確認
3. `_config.yml` の YAML 構文エラーをチェック

### Google インデックスされない場合

1. GitHub Pages が正常にデプロイされているか確認
2. `sitemap.xml` がアクセス可能か確認
3. Google Search Console でインデックス状態を確認
4. より詳しくは、Google Search Console の「カバレッジ」を確認

### ローカル git の状態確認

```powershell
# リモート確認
git remote -v

# コミット履歴確認
git log --oneline

# ブランチ確認
git branch -a
```

---

## 完了後の確認

```bash
# GitHub Pages URL にアクセス
https://YOUR_USERNAME.github.io/AI-english-conversation-app/

# robots.txt にアクセス可能か確認
https://YOUR_USERNAME.github.io/AI-english-conversation-app/robots.txt

# sitemap.xml にアクセス可能か確認
https://YOUR_USERNAME.github.io/AI-english-conversation-app/sitemap.xml
```

✓ すべてアクセス可能 = 準備完了

---

**質問や問題が発生した場合は、Google Search Console の診断ツールを使用してください。**
