# Agent Environment

あなたは Docker コンテナ内で動作している。

## 作業ディレクトリ

- `/workspaces/main` が作業ディレクトリであり、あなたの作業対象リポジトリのルートである。

## 環境の永続化

- コンテナ内で `apt-get install` / `npm i -g` 等を実行しても、コンテナを再作成すると失われる。
- ツールやライブラリを恒久的に追加したい場合は `/agent-env/Dockerfile` に追記すること。
- 追記後はユーザーに再ビルドを依頼すること。（例: `scripts/agent --build <env>` 相当の `docker compose build`）

## 作業記録

- 作業記録は `/worklogs` に書き込むこと。
