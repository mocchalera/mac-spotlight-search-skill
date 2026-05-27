# Mac Spotlight Search Skill

Use macOS Spotlight from AI coding agents.

This skill teaches agents to use `mdfind`, `mdls`, and `mdutil` as a fast candidate-finding layer for local Mac files before falling back to slower scans such as `rg` or `find`.

## What It Is

`mac-spotlight-search` is a lightweight agent skill for finding local documents, PDFs, images, notes, exported mail files, old project assets, and other indexed files on macOS.

It does not access Spotlight's private database directly. It uses the supported macOS command line tools:

- `mdfind` for Spotlight queries
- `mdls` for metadata inspection
- `mdutil` for indexing status checks

## When To Use

Use this skill when an AI agent needs to:

- Find files across a broad Mac filesystem scope
- Search by filename, indexed text, file kind, metadata, or date
- Quickly narrow candidates before opening large files
- Avoid slow full-disk scans when Spotlight can answer first

For exact source-code search inside the current repository, `rg` is usually still the right first tool.

## Installation

### Codex

```bash
mkdir -p ~/.codex/skills/mac-spotlight-search
cp -R SKILL.md agents ~/.codex/skills/mac-spotlight-search/
```

### Claude Code

If your Claude skills directory is separate:

```bash
mkdir -p ~/.claude/skills/mac-spotlight-search
cp -R SKILL.md agents ~/.claude/skills/mac-spotlight-search/
```

If you already share skills across tools with symlinks, point Claude at the same skill directory.

Add this to your global `~/.claude/CLAUDE.md` or project `CLAUDE.md`:

```markdown
For local Mac file discovery, use `mac-spotlight-search`: read `~/.claude/skills/mac-spotlight-search/SKILL.md` when a task involves finding local Mac files, documents, PDFs, images, notes, old project assets, or broad filesystem candidates. Prefer `mdfind` / `mdls` as the first candidate-finding step before slower scans, then verify results by directly reading or inspecting the matched files.
```

### Antigravity / Gemini CLI

Gemini-style agents often work best with a single Markdown file.

```bash
mkdir -p ~/.config/shared-skills
cp SKILL.md ~/.config/shared-skills/mac-spotlight-search.md
```

Then add an entry to your global `~/.gemini/GEMINI.md`:

```markdown
| mac-spotlight-search | `~/.config/shared-skills/mac-spotlight-search.md` | Find local Mac files quickly with Spotlight index search. |
```

If your Antigravity setup reads `~/.gemini/skills`, you can also install the full skill folder there:

```bash
mkdir -p ~/.gemini/skills/mac-spotlight-search
cp -R SKILL.md agents ~/.gemini/skills/mac-spotlight-search/
```

## Example Prompts

```text
Use mac-spotlight-search to find PDFs related to tax filing 2025 under my Documents folder.
```

```text
Use mac-spotlight-search before slower scans. Find old proposal files that mention GlobalTrack.
```

```text
Use mac-spotlight-search to locate image assets named logo or app icon across my Mac, then show candidate paths only.
```

## Safety Model

The skill treats Spotlight results as candidates, not truth. Agents should verify files directly before citing or summarizing them.

It also asks agents not to open sensitive-looking files unless the user clearly requested that category or the file is directly relevant. This matters for financial, legal, medical, identity, credential, and personal-message material.

## Requirements

- macOS
- Standard command line tools: `mdfind`, `mdls`, `mdutil`
- Spotlight indexing enabled for the directories you want to search

## License

MIT

---

# Mac Spotlight Search Skill 日本語版

AIコーディングエージェントに、macOSのSpotlight検索を使わせるためのスキルです。

このスキルは、`rg` や `find` のような遅い全探索に入る前に、`mdfind`、`mdls`、`mdutil` を使ってMac内のファイル候補を高速に絞り込む手順をエージェントに教えます。

## これは何か

`mac-spotlight-search` は、macOS上のローカル文書、PDF、画像、ノート、エクスポートされたメール、過去プロジェクト資産などを探すための軽量なエージェントスキルです。

Spotlightの内部データベースを直接読むものではありません。macOS標準のコマンドラインツールを使います。

- `mdfind`: Spotlight検索
- `mdls`: メタデータ確認
- `mdutil`: インデックス状態確認

## 使う場面

AIエージェントが次のような作業をするときに使います。

- Mac全体や広いフォルダ範囲からファイルを探す
- ファイル名、インデックス済み本文、種類、メタデータ、日付で探す
- 大きなファイルを開く前に候補だけ絞る
- Spotlightで答えられる検索を、低速な全探索より先に行う

現在のリポジトリ内の正確なソースコード検索では、通常は `rg` を先に使う方が適しています。

## インストール

### Codex

```bash
mkdir -p ~/.codex/skills/mac-spotlight-search
cp -R SKILL.md agents ~/.codex/skills/mac-spotlight-search/
```

### Claude Code

Claudeのスキルディレクトリが独立している場合:

```bash
mkdir -p ~/.claude/skills/mac-spotlight-search
cp -R SKILL.md agents ~/.claude/skills/mac-spotlight-search/
```

複数ツールでスキルディレクトリを共有している場合は、Claudeから同じスキルディレクトリを参照させてください。

グローバルの `~/.claude/CLAUDE.md` またはプロジェクトの `CLAUDE.md` に次を追加します。

```markdown
For local Mac file discovery, use `mac-spotlight-search`: read `~/.claude/skills/mac-spotlight-search/SKILL.md` when a task involves finding local Mac files, documents, PDFs, images, notes, old project assets, or broad filesystem candidates. Prefer `mdfind` / `mdls` as the first candidate-finding step before slower scans, then verify results by directly reading or inspecting the matched files.
```

### Antigravity / Gemini CLI

Gemini系エージェントでは、単一Markdownファイルの方が扱いやすい場合があります。

```bash
mkdir -p ~/.config/shared-skills
cp SKILL.md ~/.config/shared-skills/mac-spotlight-search.md
```

そのうえで、グローバルの `~/.gemini/GEMINI.md` に次の項目を追加します。

```markdown
| mac-spotlight-search | `~/.config/shared-skills/mac-spotlight-search.md` | SpotlightインデックスでMac内のファイルを高速に候補検索する |
```

Antigravity環境が `~/.gemini/skills` を読む場合は、フルのスキルフォルダも配置できます。

```bash
mkdir -p ~/.gemini/skills/mac-spotlight-search
cp -R SKILL.md agents ~/.gemini/skills/mac-spotlight-search/
```

## プロンプト例

```text
Use mac-spotlight-search to find PDFs related to tax filing 2025 under my Documents folder.
```

```text
Use mac-spotlight-search before slower scans. Find old proposal files that mention GlobalTrack.
```

```text
Use mac-spotlight-search to locate image assets named logo or app icon across my Mac, then show candidate paths only.
```

## 安全性

このスキルでは、Spotlightの結果を「候補」として扱います。引用や要約をする前に、エージェントは実ファイルを直接確認する必要があります。

また、金融、法律、医療、身分証、認証情報、個人的なメッセージなど、機微なファイルに見えるものは、ユーザーが明確に求めている場合や作業に直接関係する場合を除き、勝手に開かない方針にしています。

## 必要環境

- macOS
- 標準コマンドラインツール: `mdfind`, `mdls`, `mdutil`
- 検索対象ディレクトリでSpotlightインデックスが有効であること

## ライセンス

MIT
