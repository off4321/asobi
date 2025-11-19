---
doc-id: "dev-git-flow-006"
title: "標準Gitブランチ戦略 (Git-Flow)"
tags: ["Git", "ブランチ", "開発プロセス"]
versions: "1.0"
---
# 標準Gitブランチ戦略 (Git-Flow)
このドキュメントは、安定したリリースと機能開発を両立するためのGit-Flowブランチモデルの適用方法を定義します。

---
## 1. 概要
- **目的**: 大規模プロジェクトでの並行開発を管理し、リリースサイクルを構造化する。
- **対象**: すべての新規プロジェクト、開発チーム。

---
## 2. 主要ブランチ
### ✅ 永続的なブランチ
- **`master`** または **`main`**:
    - **本番環境**にデプロイされているコード。
    - タグ付けされたリリースのみをマージする。
- **`develop`**:
    - 次期リリースに向けた**最新の開発状態**。
    - `feature`、`release`、`hotfix`ブランチの統合先。

### ✅ サポートブランチ
- **`feature/*`**:
    - 新規機能開発用。`develop`から分岐し、完了後に`develop`にマージ。
- **`release/*`**:
    - リリース準備用。`develop`から分岐し、バグ修正後に`master`と`develop`の両方にマージ。
- **`hotfix/*`**:
    - 本番環境（`master`）の緊急バグ修正用。`master`から分岐し、`master`と`develop`の両方にマージ。

---
## 3. ブランチのライフサイクル
新規機能開発の基本的なフローは以下の通りです。

1. 新しい機能を開発するため、`develop`から`feature/new-feature`を**分岐**します。
2. 開発が完了したら、`feature/new-feature`を`develop`に**マージ**します。
3. `release`ブランチでQAを実施後、`master`にマージし、**本番リリース**を行います。

---
## 4. ブランチ連携フロー
サポートブランチと主要ブランチ間のマージの概念図です。

```mermaid
gitGraph
    commit
    commit
    branch develop
    checkout develop
    commit
    commit
    branch feature/A
    checkout feature/A
    commit
    checkout develop
    merge feature/A
    branch release/1.0.0
    checkout release/1.0.0
    commit tag: "v1.0.0-rc"
    checkout main
    merge release/1.0.0
    tag "v1.0.0"
    checkout develop
    merge release/1.0.0
```