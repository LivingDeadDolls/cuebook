# Cuebook

日本語 | [English](README.en.md)

ソフトウェア開発を「調査済みの要件定義」と「必要最小限で検証済みの実装」という2幕に分けます。  
Claude CodeとCodexの両方に対応し、役割ごとに指定したモデルとeffortへ処理を振り分けます。

## クイックインストール

利用するランタイムごとにCuebookをインストールしてください。  
以下のコマンドはターミナルで実行し、ユーザー環境へプラグインを導入します。

```
https://github.com/LivingDeadDolls/cuebook をセットアップして
```

### Claude Code

インストール済みでログイン済みの`claude` CLIが必要です。

```sh
claude plugin marketplace add LivingDeadDolls/cuebook && \
  claude plugin install cuebook@cuebook --scope user
```

インストールを確認します。

```sh
claude plugin list
```

### Codex

インストール済みでログイン済みの`codex` CLIが必要です。

```sh
codex plugin marketplace add LivingDeadDolls/cuebook --ref main && \
  codex plugin add cuebook@cuebook
```

インストールを確認します。

```sh
codex plugin list
```

## 使い方

作業対象のプロジェクトでClaude CodeまたはCodexを開き、チャットへ次のいずれかを入力します。

| 幕 | Claude Code | Codex | 結果 |
| --- | --- | --- | --- |
| Script | `/cuebook:script #123` | `$cuebook:script #123` | `plans/issue-123.md`を保存します |
| Perform | `/cuebook:perform` | `$cuebook:perform` | 最新のready planを実装します |

Issue番号の代わりに依頼内容を直接渡すこともできます。  
特定のplanを実行する場合も、Issue番号またはslugだけで指定でき、`plans/`の入力は不要です。  
`script`は質問する前に事実を調査し、製品コードを編集しません。  
`perform`は原則として専用branchとworktreeを作成し、検証済みの完了まで目標を所有します。

## モデル構成を変更する

Cuebookのモデル構成は通常のMarkdownに記載されています。  
新しい設定レイヤーはないため、利用するhostが対応しているモデル名とeffortへ直接書き換えられます。

### Claude Code

| 役割 | ファイル | 編集箇所 |
| --- | --- | --- |
| Script Director | `claude/skills/script/SKILL.md` | Frontmatterの`model`と`effort` |
| Dramaturg | `agents/dramaturg.md` | Frontmatterの`model`と`effort` |
| Perform Director | `claude/skills/perform/SKILL.md` | Frontmatterの`model`と`effort` |
| Performer | `agents/performer.md` | Frontmatterの`model`と`effort` |

### Codex

| 役割 | ファイル | 編集箇所 |
| --- | --- | --- |
| Script Director・Dramaturg | `skills/script/SKILL.md` | spawn指示内のモデル名とreasoning effort |
| Perform Director・Dramaturg・Performer | `skills/perform/SKILL.md` | spawn指示内のモデル名とreasoning effort |

インストール済みのプラグインcacheは更新時に置き換わる可能性があります。  
設定を維持する場合は、cloneまたはforkしたrepoを編集してローカルmarketplaceとして導入してください。

```sh
git clone https://github.com/LivingDeadDolls/cuebook.git
cd cuebook
# 上表のファイルを編集し、利用するruntimeをインストールします。
claude plugin marketplace add "$PWD"
claude plugin install cuebook@cuebook --scope user
codex plugin marketplace add "$PWD"
codex plugin add cuebook@cuebook
```

GitHub版を既にインストールしている場合は、先にアンインストールしてからカスタマイズ版を追加してください。

## 更新

### Claude Code

```sh
claude plugin marketplace update cuebook && \
  claude plugin update cuebook@cuebook --scope user
```

### Codex

```sh
codex plugin marketplace upgrade cuebook && \
  codex plugin remove cuebook@cuebook && \
  codex plugin add cuebook@cuebook
```

## アンインストール

### Claude Code

```sh
claude plugin uninstall cuebook@cuebook --scope user
claude plugin marketplace remove cuebook
```

### Codex

```sh
codex plugin remove cuebook@cuebook
codex plugin marketplace remove cuebook
```

## デフォルトのモデル構成

| Runtime | 役割 | Model | Effort |
| --- | --- | --- | --- |
| Claude Code | Script Director | Fable 5.1 | medium |
| Claude Code | Dramaturg（調査） | Opus 5 | medium |
| Claude Code | Perform Director | Opus 5 | medium |
| Claude Code | Performer | Sonnet 5 | high |
| Codex | Script Director | GPT-5.6 Sol | high |
| Codex | Dramaturg（調査） | GPT-5.6 Terra | medium |
| Codex | Perform Director | GPT-5.6 Terra | high |
| Codex | Performer | GPT-5.6 Luna | xhigh |

## 契約

Planは作業対象プロジェクトの`plans/`に保存されます。  
Planに記載した目標・非目標・受け入れ基準・権限境界が実装契約になります。  
実装中に事実が不明になった場合は調査し、契約を変えない範囲で実装手順を再計画できます。  
現在のcheckoutがすでに専用環境でない限り、`perform`は専用branchとworktreeを使用します。  
ユーザーが望む結果を変更した場合だけ、`script`を再実行します。

## ライセンス

[MIT](LICENSE)
