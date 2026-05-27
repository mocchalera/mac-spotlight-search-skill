# Security Policy

## Scope

This repository provides an agent skill: Markdown instructions for using macOS Spotlight command line tools from an AI coding-agent environment.

The skill itself does not:

- install a daemon or background service
- expose a network endpoint
- grant filesystem permissions
- bypass macOS privacy controls
- transmit local file paths or file contents
- read Spotlight's private database directly

It uses standard macOS tools such as `mdfind`, `mdls`, and `mdutil`.

## Primary Risk

The main security risk is not code execution in this repository. The risk is that an AI agent using the skill may discover local files more efficiently than expected.

That can expose sensitive information if the agent, runtime, or user workflow allows broad local searches without clear task scope.

Sensitive examples include:

- financial records
- legal documents
- medical documents
- identity documents
- credentials, tokens, and key material
- private messages or exported mail
- confidential work files

## Expected Agent Behavior

Agents using this skill should:

- use it only when the user intent is clear for the current task
- prefer scoped searches with `mdfind -onlyin <directory>`
- avoid whole-home or whole-disk searches unless explicitly requested
- treat Spotlight results as candidate paths, not permission to read files
- show candidate paths before opening files when private material may be present
- ask before reading or summarizing sensitive-looking files
- verify returned paths with direct file checks before relying on them
- avoid changing Spotlight settings or rebuilding indexes unless explicitly requested

Agents should not:

- run `sudo mdutil`
- disable or modify Spotlight privacy settings
- rebuild indexes as a default troubleshooting step
- search for credentials, secrets, tokens, or private keys unless the user explicitly requests a security audit or recovery task
- paste large lists of local paths into model context when a small candidate set is enough

## User Guidance

Before installing or invoking this skill, review your agent runtime's local-file permissions, command approval policy, and transcript/log retention.

This skill is safest when used with:

- an approval prompt before shell commands
- a narrow working directory
- explicit task-specific search terms
- candidate-path review before file reads
- transcript handling appropriate for private local paths

## Reporting Security Issues

If you find a security issue in the instructions or examples in this repository, please open a GitHub issue with:

- what behavior is unsafe
- which file and section caused it
- a safer wording or command pattern, if you have one

Do not include private local file paths, secrets, credentials, or personal documents in public reports.

## Supported Versions

Security updates apply to the `main` branch.

---

# セキュリティポリシー

## 対象範囲

このリポジトリは、AIコーディングエージェント環境からmacOSのSpotlightコマンドラインツールを使うためのMarkdown手順書です。

このスキル自体は次のことを行いません。

- 常駐プロセスやバックグラウンドサービスのインストール
- ネットワークエンドポイントの公開
- ファイルシステム権限の付与
- macOSのプライバシー制御の回避
- ローカルファイルパスやファイル内容の送信
- Spotlightの非公開データベースの直接読み取り

使うのは `mdfind`、`mdls`、`mdutil` などのmacOS標準ツールです。

## 主なリスク

このリポジトリの主なリスクは、コード実行そのものではありません。リスクは、このスキルを使うAIエージェントが、想定以上に効率よくローカルファイルを見つけられることです。

タスク範囲が曖昧なまま広いローカル検索を許すと、機微な情報が露出する可能性があります。

例:

- 金融記録
- 法務文書
- 医療文書
- 身分証関連文書
- 認証情報、トークン、鍵
- 個人的なメッセージやメール書き出し
- 機密の業務ファイル

## 期待されるエージェントの振る舞い

このスキルを使うエージェントは、次のように振る舞うべきです。

- 現在のタスクに対するユーザー意図が明確な場合だけ使う
- できるだけ `mdfind -onlyin <directory>` で範囲を絞る
- 明示的に求められない限り、ホーム全体やディスク全体を検索しない
- Spotlightの結果を候補パスとして扱い、ファイルを読む許可とは見なさない
- 私的な情報が含まれそうな場合、ファイルを開く前に候補パスを提示する
- 機微なファイルに見えるものは、読む前や要約する前に確認する
- 返されたパスは、直接確認してから利用する
- 明示的に求められない限り、Spotlight設定変更やインデックス再構築を行わない

エージェントは次を行うべきではありません。

- `sudo mdutil` を実行する
- Spotlightのプライバシー設定を無効化・変更する
- 通常のトラブルシュートとしてインデックス再構築を行う
- ユーザーが明示的にセキュリティ監査や復旧を依頼していないのに、認証情報、秘密情報、トークン、秘密鍵を探す
- 小さな候補集合で十分な場面で、大量のローカルパスをモデル文脈に貼り付ける

## ユーザー向けガイダンス

このスキルをインストールまたは呼び出す前に、利用しているエージェント実行環境のローカルファイル権限、コマンド承認ポリシー、会話ログやトランスクリプトの保存方針を確認してください。

このスキルは、次の条件でより安全に使えます。

- シェルコマンド実行前に承認がある
- 作業ディレクトリや検索範囲が狭い
- タスク固有の検索語が明確
- ファイルを読む前に候補パスを確認する
- ローカルパスを含む会話ログの扱いが適切

## セキュリティ問題の報告

このリポジトリの手順や例にセキュリティ上の問題を見つけた場合は、GitHub Issueで報告してください。

報告には次を含めてください。

- どの振る舞いが危険か
- どのファイル・セクションが原因か
- より安全な表現やコマンド例があればそれも記載

公開Issueには、私的なローカルファイルパス、秘密情報、認証情報、個人文書を含めないでください。

## サポート対象

セキュリティ更新は `main` ブランチに適用します。
