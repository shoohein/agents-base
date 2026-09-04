# scripts

## 概要

| スクリプト | 用途                                     |
| ---------- | ---------------------------------------- |
| `new-env`  | 作業環境を新規に生成する場合に使用する。 |
| `agent`    | エージェントを起動する場合に使用する。   |

## new-env: 作業環境の生成

用途: 作業環境を新規に生成する場合に使用する。

使用方法:

```sh
scripts/new-env <forge>/<org>/<repo> [host-repo-path]
```

実行例:

```sh
scripts/new-env gh/me/myrepo
scripts/new-env gl/mygroup/myproject
```

`forge` を省略した場合は `gh` として扱われる。`host-repo-path` を省略した場合は `${REPOS_ROOT:-$HOME}/<forge>/<org>/<repo>` が作業リポジトリとして設定される。

生成物は `<forge>/<org>/<repo>/` である。生成後は `env/compose.yaml` の内容を確認し、`scripts/agent <forge>/<org>/<repo>` で起動する。

## agent: エージェントの起動

前提条件: `AGENT_HOME` が設定されていること。

```sh
export AGENT_HOME=...
```

使用方法:

```sh
scripts/agent <env> [opencode_args...]
```

`<env>` には `meta` または `<forge>/<org>/<repo>` を指定する。`forge` を省略した `<org>/<repo>` 形式も利用可能である。

実行例:

```sh
scripts/agent meta
scripts/agent gh/me/myrepo
scripts/agent --build gh/me/myrepo
scripts/agent --list
```

オプションの概要は以下のとおりである。詳細は `scripts/agent --help` を参照すること。

| オプション     | 用途                                           |
| -------------- | ---------------------------------------------- |
| `--build`      | 起動時にイメージをビルドする場合に指定する。   |
| `--list`       | 利用可能な環境の一覧を表示する場合に指定する。 |
| `--help`, `-h` | 使用方法を表示する場合に指定する。             |
