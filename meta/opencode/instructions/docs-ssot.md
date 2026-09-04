# ドキュメント SSoT 原則

挙動を定義するファイルが正本（Single Source of Truth）。Markdown は正本への参照と要約のみを担い、値のコピーを禁ずる。コピーは必ず乖離し、二重修正を強いる。

## 適用範囲

この原則は Markdown（`README.md`, `AGENTS.md`, `docs/**/*.md`, `worklogs/**/*.md`）を書く際の制約である。設定ファイル自体（`*.yaml`, `*.json`, `Dockerfile`, `*.sh`, `.gitignore`, `.pre-commit-config.yaml` 等）に SSoT 宣言コメントを付与してはならない。設定ファイルは動作に必要な最小のコメントのみを残し、自己記述的に保つ。

## 原則

### 1. 正本の一元化 — 値をコピーしない

- `compose.yaml` の `volumes` 定義が正本なら、`AGENTS.md` に yaml ブロックを再掲しない。
- `opencode.jsonc` の `model` 定義が正本なら、Markdown にモデル名を列挙しない。
- 追跡対象は `.gitignore` / `.git/info/exclude` が正本。`README.md` / `AGENTS.md` に `追跡: meta/, template/` と書かない。共有は `.gitignore` を更新することで行う。

### 2. 責務分離 — 利用者向けと開発者向けを混在させない

- `README.md` は利用者が `scripts/new-env` を使うために必要な最小情報のみ（`docs/guide` 相当）。
- 内部の管理境界や運用詳細は `AGENTS.md` にも冗長に書かない。

### 3. 自己記述性の活用 — 設定自体が説明を兼ねる場合は補足文を書かない

- 例: `${VAR:?msg}` / `${VAR:-default}` は `fail-fast` / `fallback` の説明を兼ねる。
- 例: `compose.yaml` のプレースホルダ定義は `scripts/new-env` のコメントが正本。

## 判断基準

- この記述を削除しても、正本ファイルを読めば同じ情報が得られるか？ → Yes なら削除。
- この記述を残すと、正本更新時に二重修正が必要になるか？ → Yes なら削除。
- 削除により初学者が迷うか？ → 迷うなら「正本への参照1行」に置換。値のコピーはしない。
  - ただしファイル名・配置から自明な場合（例: lint 設定なら `.pre-commit-config.yaml` を見ればわかる）は参照1行も書かない。

## 具体例（本リポジトリ）

- ❌ `AGENTS.md` に `## compose の bind` の yaml ブロックを再掲 → ✅ `compose.yaml を参照`
- ❌ `README.md` に `## Git 管理境界` → ✅ 追跡対象は `.gitignore` を参照
- ❌ `AGENTS.md` に `opencode` モデル列挙 / `AGENT_HOME fail-fast` 補足 → ✅ 正本が自己記述的

## 禁止事項 — 成果物に SSoT コメントを残さない

- 成果物の先頭や各所に `# SSoT: ...`, `# 正本は本ファイル`, `# docs へのコピーはしない` 等のメタコメントを書かない。
- 設定ファイルのコメントは、その行の動作理由がコードから自明でない場合のみに留める。
- ❌ `# SSoT: lint/format の正本は本ファイル。docs への値コピーはしない` — 原則の宣言は Markdown 側の責務ではなく、そもそも見ればわかるため何も書かない
