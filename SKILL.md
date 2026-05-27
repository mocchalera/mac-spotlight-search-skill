---
name: mac-spotlight-search
description: Use macOS Spotlight from the terminal with mdfind and mdls to quickly locate local files by filename, indexed text, metadata, kind, date, or scope before falling back to slower filesystem scans.
metadata:
  short-description: Search Mac files with Spotlight index
---

# Mac Spotlight Search

Use this skill when a user asks an agent to find local Mac files, documents, PDFs, images, notes, emails exported as files, or old project assets, especially outside the current repository.

Spotlight is a fast candidate finder, not a source of truth. Use it to narrow paths, then verify with direct file reads, `mdls`, `rg`, `file`, `stat`, PDF/text extraction, or the app-specific parser that fits the file.

## Default Workflow

1. Start with `mdfind` when the search may span many folders or the whole Mac.
2. Prefer scoped searches with `-onlyin` when you know the likely root.
3. Limit output early with `head` or a small loop; do not dump thousands of paths into context.
4. Inspect metadata with `mdls` before opening large or private-looking files.
5. Verify the exact file content or filename with a direct tool before citing results.
6. Fall back to `rg`, `find`, app indexes, or manual inspection if Spotlight returns nothing or appears stale.

## Commands

Basic indexed text or filename search:

```bash
mdfind '確定申告 2025' | head -50
```

Search within one directory:

```bash
mdfind -onlyin "$HOME/Documents" '確定申告 2025' | head -50
```

Filename search:

```bash
mdfind 'kMDItemFSName == "*invoice*"cd' | head -50
```

PDF search:

```bash
mdfind 'kMDItemContentTypeTree == "com.adobe.pdf" && (* == "確定申告 2025"cd)' | head -50
```

Kind search examples:

```bash
mdfind 'kMDItemKind == "*PDF*"cd && (* == "契約書"cd)' | head -50
mdfind 'kMDItemContentTypeTree == "public.image" && (* == "logo"cd)' | head -50
mdfind 'kMDItemContentTypeTree == "public.movie" && (* == "demo"cd)' | head -50
```

Date filters:

```bash
mdfind 'kMDItemFSContentChangeDate >= $time.today(-30) && (* == "invoice"cd)' | head -50
mdfind 'kMDItemFSCreationDate >= $time.iso(2026-01-01T00:00:00Z) && (* == "proposal"cd)' | head -50
```

Metadata inspection:

```bash
mdls -name kMDItemDisplayName -name kMDItemKind -name kMDItemContentTypeTree -name kMDItemFSContentChangeDate -- "$path"
```

Check whether a directory is excluded from indexing:

```bash
mdutil -s "$HOME/Documents"
```

## Query Rules

- Use quoted predicates for metadata searches.
- Add `c` for case-insensitive and `d` for diacritic-insensitive matches, for example `"*term*"cd`.
- Use `kMDItemFSName` for filename, `kMDItemTextContent` or `*` for indexed content, and `kMDItemContentTypeTree` for broad file type families.
- Search short known terms first. If nothing appears, try filename fragments, Japanese/English variants, and likely date or directory scopes.
- For code inside the current repo, start with `rg`; Spotlight is more useful for broad personal/project asset discovery than exact source navigation.

## Safety and Privacy

- Do not open or summarize sensitive-looking files unless the user asked for that category or the file is clearly task-relevant.
- Show candidate paths and ask before reading private categories such as finances, medical, identity documents, credentials, personal messages, or legal files when the user's intent is ambiguous.
- Do not run `sudo mdutil`, rebuild indexes, or change Spotlight privacy settings unless the user explicitly asks.
- If a path includes cloud storage, external drives, backups, or app containers, verify existence with `test -e` or `stat`; Spotlight can return stale or unavailable paths.

## When Spotlight Fails

Spotlight may miss files that are unindexed, newly created, excluded by privacy settings, inside packages/archives, on unavailable volumes, or in formats without text importers.

Fallback sequence:

1. Use `rg --files` or `find` inside the likely root.
2. Use `rg -i` for exact text across plain text files.
3. Use format-specific extraction for PDFs, DOCX, mail exports, archives, or databases.
4. Report that Spotlight was attempted, what query was used, and why fallback was needed.