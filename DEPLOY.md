# MindMap デプロイ手順

> リポジトリ名：`mindmap-app` ／ 公開設定：Public ／ ホスティング：Vercel
> 作業はすべて Mac のターミナルで実行。所要時間：5〜10分。

---

## ステップ0：いまどのCLIが入っているか確認（任意）

```bash
which gh && gh --version
which vercel && vercel --version
```

- `gh` が入っていれば → **ルートA（速い）**
- 入っていなくても → **ルートB（GitHub.comでぽちぽち）**

`gh` がなくて、これを機にインストールしたいなら：

```bash
brew install gh
gh auth login
```

---

## ルートA：`gh` CLIが使える場合（最速）

ターミナルで以下を順番に叩く。

```bash
cd ~/Desktop/claude/MindMap

git init
git add .
git commit -m "Initial commit: MindMap — 初期値は、愛"
git branch -M main

# GitHubリポジトリを作成して、そのままpushまで一発
gh repo create mindmap-app --public --source=. --push
```

リポジトリURLが表示されるので控えておく（後でVercelに渡す）。

そのまま続けて Vercel CLI でデプロイ：

```bash
# Vercel CLI を入れていない場合
brew install vercel-cli   # または npm i -g vercel

vercel login
vercel link --yes
vercel --prod
```

数十秒待つと本番URL（`https://mindmap-app-xxx.vercel.app`）が出る。

---

## ルートB：GitHub.com と Vercel.com のWeb画面でやる場合

### B-1. ローカルでgit初期化

```bash
cd ~/Desktop/claude/MindMap
git init
git add .
git commit -m "Initial commit: MindMap — 初期値は、愛"
git branch -M main
```

### B-2. GitHubでリポジトリ作成

1. https://github.com/new を開く
2. Repository name：`mindmap-app`
3. Public を選択
4. Initialize（README/.gitignore/license）はすべて **オフ**（ローカルにすでにある）
5. **Create repository** ボタン

### B-3. ローカルとGitHubを接続してpush

GitHubが表示する `…or push an existing repository from the command line` の3行をそのままターミナルで実行。例：

```bash
git remote add origin git@github.com:bookenn510/mindmap-app.git
git branch -M main
git push -u origin main
```

> SSHでなくHTTPSにしたい場合は `git@github.com:...` の代わりに `https://github.com/bookenn510/mindmap-app.git` を使う。

### B-4. Vercel でインポート

1. https://vercel.com/new を開く（GitHubアカウントでログイン）
2. `mindmap-app` を見つけて **Import**
3. Framework Preset：**Other**（静的HTMLなのでそのまま）
4. Build Command / Output Directory：触らない（空のまま）
5. **Deploy** ボタン

約30秒で完了。本番URLが発行される。

---

## デプロイ後の確認

- PCのSafari / Chrome で本番URLを開いて、サンプルマップが表示されることを確認
- iPhone でも同じURLを開いて動作チェック
- 「🔗 共有」を押す → コピーされたURLを別デバイスで開いて同じマップが復元されるか確認

---

## 以降の更新フロー

ローカルでHTMLを直したあと：

```bash
cd ~/Desktop/claude/MindMap
git add .
git commit -m "更新の中身を一行で（例: ノードのドラッグUI改善）"
git push
```

push するだけでVercelが自動で再デプロイする（30秒〜1分）。

---

## カスタムドメインを当てたいとき（任意・将来）

例：`mindmap.bookenn.dev` のように独自URLにしたい場合

1. Vercel ダッシュボード → プロジェクト → Settings → Domains
2. ドメインを追加 → DNS設定の指示に従う
3. ドメインがなければお名前.com／Cloudflare Registrarで取得（年1,000円〜）

---

## 数字と気持ちの行き先

このデプロイで、堅太郎さんが「想夫恋の店内で思いついたこと」をiPhoneで開いてそのまま整理し、PCで仕上げ、家族にURLで共有できる流れが完成する。

「サーバーを契約して、データベース立てて、認証つけて…」という重力を一切持たない。push一発で世界に出る。これがVercel + 静的HTMLの軽さで、85点で動くの実装版。

「初期値は、愛」を画面上部に固定したスローガンが、誰のどこの画面で開かれても変わらず残る。それがデプロイの意味だと思う。
