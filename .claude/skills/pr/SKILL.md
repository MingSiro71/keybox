---
name: pr
description: Generate a PR title and body from the current branch changes
argument-hint: "[base-branch]"
user-invocable: true
allowed-tools:
  - Bash
  - Read
  - Grep
  - Glob
---

# PR 本文の作成

現在の作業ブランチの変更内容から PR のタイトルと本文を生成して会話上に出力する。

## 手順

1. `git log` と `git diff` でベースブランチ（デフォルト: `master`、引数で変更可: `$ARGUMENTS`）からの変更内容を把握する
2. 変更内容を分析して、タイトルと本文を生成する
3. 会話上に出力する

## 出力ルール

- **タイトル**: 英語で書く
- **本文**: 英語と日本語の両方で書く
  1. まず英語で全体を書く
  2. その後に日本語訳を続けて書く
  3. 章ごとに英語→日本語と交互にするのではなく、英語ブロック全体の後に日本語ブロック全体を置く

## 出力フォーマット

タイトルは平文で出力し、本文はマークダウンのコードブロックで出力する。
本文の外側のコードブロックにはチルダを使用すること。本文中でコードブロックを記載する際にバッククオートを使用するが、これと衝突して表示が崩れるのを防ぐため。

~~~~
**Title**: <英語タイトル>

~~~markdown
## Summary
...
## Changes
...

---

## 概要
...
## 変更内容
...
~~~
~~~~
