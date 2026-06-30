# articles-qiita

[qiita-sync](https://github.com/ko-he-8/qiita-sync) を使って、Qiita の記事を Markdown としてこのリポジトリで管理しています。記事は [`articles/`](./articles/) 配下にジャンル別ディレクトリで置き、Qiita との同期はスキル・コマンド・GitHub Actions のいずれかで行います。

## 前提

- **DevContainer 上での実行**を前提としています（`qiita-sync` の導入や環境変数の注入は DevContainer 任せ）。
- ルートに `.env` を置き、Qiita のアクセストークンを設定してください（`.env` は Git 管理外）。

  ```
  QIITA_ACCESS_TOKEN=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
  ```

  DevContainer 起動時に `--env-file` でコンテナの環境変数として注入されるため、`source .env` は不要です。

## ディレクトリ構成

```
articles/
├── README.md       … articles 全体のインデックス
├── ai/             … AI 全般
│   └── claude/     … Claude / Claude Code
└── hardware/       … ハードウェア・電子工作
```

各ジャンルディレクトリには記事の雛形 `_template.md` と、説明用の `README.md` を置いています。`README.md` と `_template.md` は同期対象から除外されるため、Qiita には投稿されません。

## 記事の書き方

1. 対象ジャンルの `_template.md` をコピーし、英小文字スラッグにリネームする
   （例: `articles/ai/claude/claude-code-getting-started.md`）
2. ファイル先頭のヘッダーと本文を記入する

   ```markdown
   <!--
   title:   記事のタイトル
   tags:    Claude,ClaudeCode,AI
   private: true
   -->
   # 記事のタイトル

   本文…
   ```

   - `private`: 既定は限定共有の `true`。公開してよければ `false` に変更する
   - `id`: 初回同期時に qiita-sync が自動採番するので手動記入は不要
   - `tags`: カンマ区切り・最大5つまで

## 記事を同期する

### スキルで同期する（推奨）

Claude Code で **「記事を同期」** と伝えると、[`qiita-sync` スキル](./.claude/skills/qiita-sync/SKILL.md)が次を行います。

1. DevContainer 上でローカルの `qiita_sync sync` を実行
2. `git status` / `git diff` で変更内容を提示
3. 確認の上でコミット

### コマンドで同期する

プロジェクトルート（`.env` がある場所）で直接実行します。

```bash
qiita_sync sync . -e '**/README.md' '.*/**/*.md' '**/_template.md'
```

`-e` は qiita-sync のデフォルト除外を上書きするため、既定の `**/README.md`・`.*/**/*.md` も併記したうえで、雛形 `**/_template.md` を加えています。**これを省くとテンプレートが新規記事として Qiita に投稿されてしまう**ため、必ずこのオプション付きで実行してください。

### GitHub Actions で自動同期する

`main` に push すると [Qiita Sync ワークフロー](./.github/workflows/qiita_sync.yml)が走り、自動で同期します。リポジトリの Secrets に `QIITA_ACCESS_TOKEN` を設定しておく必要があります。
