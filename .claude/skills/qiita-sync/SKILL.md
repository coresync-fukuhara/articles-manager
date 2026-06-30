---
name: qiita-sync
description: Qiita と記事を同期する。DevContainer 上でローカルの qiita_sync sync を実行し、差分があればコミットする。ユーザーが「記事を同期」「Qiita と同期」などと依頼したときに使う。
---

**前提: DevContainer 上での実行を想定しています。**
`qiita-sync` のインストールや `.env` の配置は DevContainer 環境が整っていることを前提としています。

`.env` に設定された `QIITA_ACCESS_TOKEN` を使ってローカルで `qiita_sync sync` を実行し、差分があればコミットします。

## 手順

1. `qiita_sync sync . -e '**/README.md' '.*/**/*.md' '**/_template.md'` を実行して Qiita と同期する
2. `git status` で変更があるか確認する
3. `git status` の結果を踏まえ、変更内容をユーザーに伝える（`git diff` で既存ファイルの変更を、新規ファイル（untracked）があればそれも併せて提示する）
4. ユーザーの確認を取ってからコミットする（コミットメッセージは `Sync: Qiita から記事の変更を同期した`）

## 実行コマンド

```bash
qiita_sync sync . -e '**/README.md' '.*/**/*.md' '**/_template.md'
```

`-e` はデフォルトの除外 glob を上書きするため、既定の `**/README.md` と `.*/**/*.md` も併記したうえで、各ジャンル下のひな形 `**/_template.md` を加えています。これを省くとテンプレートが新規記事として Qiita に投稿されてしまうので、必ずこのオプション付きで実行してください。

`QIITA_ACCESS_TOKEN` は DevContainer 起動時に `--env-file`（`.env`）でコンテナ環境変数として注入されるため、`source .env` は不要です。
コマンドは必ずプロジェクトルート（`.env` があるディレクトリ）で実行してください。
