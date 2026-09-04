# Agents Environment

エージェント実行環境（Dockerfile / compose / opencode 設定 / worklogs）のみを Git 管理するメタリポジトリ。

## レイアウト

```
.
├── meta/                 # メタエージェント自身の環境
│   ├── env/
│   ├── opencode/  
│   └── worklogs/
├── template/             # <forge>/* 生成用テンプレート
│   ├── env/
│   └── opencode/
├── <forge>/<org>/<repo>/ # 作業リポジトリごとの環境
│   ├── env/
│   ├── opencode/
│   └── worklogs/
├── opencode/             # 全ての環境で共通のOpenCode設定
└── scripts/
```

`meta/worklogs/` と `<forge>/<org>/<repo>/worklogs/` にエージェントの全作業記録を集約。

## 運用手順

```sh
scripts/new-env gh/me/myrepo
scripts/agent gh/me/myrepo
```

詳細は `scripts/README.md` を参照すること。

### worklogs

- 作業エージェントはコンテナ内で `/worklogs` に書き込む。
- ホストでは `meta/worklogs/` と `<forge>/<org>/<repo>/worklogs/` に現れ、本リポジトリで一括管理。
