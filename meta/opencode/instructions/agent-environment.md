# Agent Environment

このリポジトリは**エージェント実行環境のみ**を Git 管理する。作業リポジトリのコードは管理しない。

## 用語集

| 用語                  | 指すもの                                                                                                                                                                              |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| メタエージェント      | `meta/` 環境で動くエージェント。本リポジトリ自体を操作する                                                                                                                            |
| 作業エージェント      | `<forge>/<org>/<repo>/` 環境（例: `gh/me/myrepo`）で動くエージェント。ホスト `${REPOS_ROOT:-$HOME}/<forge>/<org>/<repo>` を `/workspaces/main` にマウントして作業リポジトリを操作する |
| opencode エージェント | `opencode.jsonc` で定義される `plan` / `build` 等の opencode 機能。単に「エージェント」と書いた場合は原則上記2者を指し、区別が必要な場合のみ「opencode エージェント」と明記する       |

## レイアウト

```
<repo>/                 # コンテナ内 /workspaces/main
├── meta/               # メタ自身の環境
│   ├── env/
│   ├── opencode/
│   └── worklogs/
├── template/           # 生成用雛形
│   ├── env/
│   └── opencode/
├── <forge>/<org>/<repo>/  # 作業環境（template から生成）
│   ├── env/               # エージェントが動作するコンテナ環境
│   ├── opencode/          # 作業リポジトリ固有のOpenCodeの設定
│   └── worklogs/          # 作業エージェントの作業記録
├── opencode/           # 全ての環境で共通のOpenCode設定
└── scripts/            # ユーザー用のスクリプト
```

- ホスト `${REPOS_ROOT:-$HOME}/<forge>/<org>/<repo>`（作業リポジトリ実体）≠ `<forge>/<org>/<repo>`（本リポジトリ内のエージェント環境）。後者の `env/compose.yaml` が前者を `/workspaces/main` にマウントする。
- `meta/` と `template/` は独立して管理し同期を要しない。

## Dockerfile

- ツール不足を検知したエージェントは `env/Dockerfile` に追記する。
  - メタ: 実体 `meta/env/Dockerfile`
  - 作業: 実体 `<forge>/<org>/<repo>/env/Dockerfile`（例: `gh/me/myrepo/env/Dockerfile`）
- 追記後は `docker compose build` をユーザーに依頼する。
- `FROM agent/opencode:latest` は当面固定しない。
- `image` は `agents/<forge>/<org>/<repo>`（`meta` は `agents/meta`）を命名規則とし、`template/env/compose.yaml` の `agents/<forge>/<org>/<repo>` プレースホルダが正本。

## worklogs

- 作業記録は `meta/worklogs/` と `<forge>/<org>/<repo>/worklogs/` に保存し本リポジトリで一括管理。作業リポジトリ側（`${REPOS_ROOT:-$HOME}/<forge>/...`）では管理しない。
- 作業エージェントはコンテナ内で `/worklogs` に書き込む。
- コミットは基本的にメタが行う。
- フォーマット改善も基本的にメタが担う。
