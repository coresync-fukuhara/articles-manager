# articles

Qiita に同期する記事を、ジャンル別ディレクトリで管理しています。

## ディレクトリ構成

```
articles/
├── ai/             … AI 全般
│   └── claude/     … Claude / Claude Code
└── hardware/       … ハードウェア・電子工作
```

各ジャンルディレクトリには、記事の雛形となる `_template.md` を置いています。

## 記事の追加手順

1. 対象ジャンルの `_template.md` をコピーし、英小文字スラッグ（例: `claude-code-getting-started.md`）にリネームする
2. ファイル先頭のヘッダー（`title` / `tags` / `private`）と本文を記入する
3. 公開してよければ `private: false` に変更する
4. main に push、または「記事を同期」で qiita-sync スキルを実行する

## 同期の仕組み

- 記事はファイル先頭の `<!-- title / tags / id / private -->` ヘッダーで Qiita と紐づく
- `id` は初回 sync 時に qiita_sync が自動採番するため、手動記入は不要
- `README.md` と `_template.md` は qiita_sync の除外対象なので Qiita には投稿されない
