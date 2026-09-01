# Cuebook

[English](README.md) | [日本語](README.ja.md)

Cuebookは、ソフトウェア開発を「調査済みの要件定義」と「必要最小限で検証済みの実装」という2幕に分けます。Claude CodeとCodexの両方に対応し、役割ごとに意図したモデルとeffortへ処理を振り分けます。

## クイックインストール

利用するruntimeごとにCuebookをインストールしてください。以下のコマンドはターミナルで実行し、ユーザー環境へpluginを導入します。

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

作業対象のprojectでClaude CodeまたはCodexを開き、chatへ次のいずれかを入力します。

| 幕 | Claude Code | Codex | 結果 |
| --- | --- | --- | --- |
| Script | `/cuebook:script #123` | `$cuebook:script #123` | `plans/issue-123.md`を保存します |
| Perform | `/cuebook:perform plans/issue-123.md` | `$cuebook:perform plans/issue-123.md` | planを実装して検証します |

Issue番号の代わりに、依頼内容を直接渡すこともできます。`script`は質問する前に事実を調査し、product codeを編集しません。`perform`は完了まで目標を所有し、ユーザーの権限が必要な判断だけで停止します。

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

## モデル構成

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

各hostでmodel aliasを利用できる必要があります。Claude Codeはskillとagentのfrontmatterから、Codexは各skillの委譲時にmodelを切り替えます。

## 契約

Planは作業対象projectの`plans/`に保存されます。Planに記載した目標・非目標・受け入れ基準・権限境界が実装契約になります。実装中に事実が不明になった場合は調査し、契約を変えない範囲で実装手順を再計画できます。ユーザーが望む結果を変更した場合だけ、`script`を再実行します。

## ライセンス

[MIT](LICENSE)
