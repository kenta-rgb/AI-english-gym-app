# GitHub 公開・Google 検索対応の手順

ローカルの Git リポジトリは作成・コミット完了しました。次のステップで GitHub に公開し、Google 検索に対応させます。

## 手順

### 1. GitHub リポジトリを作成

1. [GitHub](https://github.com/new) にアクセス
2. リポジトリ名：`AI-english-gym-app`（推奨）
3. 説明：`AI英会話ジム - 英会話アプリを安全にデプロイし、公開前にセキュリティリスクを確認・修正するスキル`
4. **Public** を選択（Google 検索対応のため）
5. **Initialize this repository with:**
   - ✓ README を選択 → 後で上書きするので不要
   - その他はチェックなし
6. **Create repository** をクリック

### 2. GitHub に Push する

リポジトリ作成後、以下のコマンドを実行：

```bash
# GitHub リポジトリをリモートとして追加
git remote add origin https://github.com/YOUR_USERNAME/AI-english-gym-app.git

# ブランチ名を main に変更（GitHub デフォルト）
git branch -M main

# GitHub に push
git push -u origin main
```

**`YOUR_USERNAME`** をあなたの GitHub ユーザー名に置き換えてください。

### 3. GitHub Pages を有効化

1. GitHub のリポジトリページにアクセス
2. **Settings** タブをクリック
3. 左メニューから **Pages** を選択
4. **Source** セクションで：
   - Deploy from a branch を選択
   - Branch: `main`
   - Folder: `/ (root)`
5. **Save** をクリック

数分後、以下の URL が生成されます：
```
https://YOUR_USERNAME.github.io/AI-english-conversation-app/
```

### 4. Google Search Console に登録

1. [Google Search Console](https://search.google.com/search-console) にアクセス
2. **URL プレフィックス** を選択
3. GitHub Pages の URL を入力：
   ```
   https://YOUR_USERNAME.github.io/AI-english-conversation-app/
   ```
4. **続行** をクリック
5. DNS レコードまたは HTML ファイルでサイト所有権を確認
   - **HTML ファイル** 方式がおすすめ（GitHub Pages には最適）
6. メタタグをコピー
7. リポジトリの **index.html** または **README.md** に追加

### 5. XML サイトマップを送信

Google Search Console で：

1. **サイトマップ** セクションに移動
2. サイトマップ URL を追加：
   ```
   https://YOUR_USERNAME.github.io/AI-english-conversation-app/sitemap.xml
   ```
3. **送信** をクリック

### 6. インデックス作成をリクエスト

Google Search Console で：

1. **URL検査** ツールを使用
2. GitHub Pages の URL を入力
3. **インデックス登録をリクエスト** をクリック
4. 24～48時間後に Google 検索に表示されます

## 注意事項

- GitHub Pages のビルドには数分かかる場合があります
- Google インデックス作成には 24～72時間かかる場合があります
- `_config.yml` と `sitemap.xml` の `your-username` をあなたの GitHub ユーザー名に置き換えてください

## 確認事項

```bash
# リモートが正しく設定されているか確認
git remote -v

# GitHub Pages が正しく構築されているか確認
# ブラウザで以下にアクセス
https://YOUR_USERNAME.github.io/AI-english-conversation-app/
```

## GitHub Pages ビルド確認

1. リポジトリの **Actions** タブをクリック
2. **pages build and deployment** ワークフローの状態を確認
3. ✓ が表示されたら成功

---

**完了後：**

- ✓ GitHub Pages で公開完了
- ✓ Google Search Console に登録完了
- ✓ 24～72時間後に「AI英会話練習」で Google 検索に表示されます

この README に記載されたすべてのステップを完了すれば、
「AI英会話練習」で Google 検索されたときにこのリポジトリが表示されるようになります。
