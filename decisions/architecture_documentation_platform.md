# アーキテクチャ文書公開基盤
- 対象システム名は「アーキテクチャ文書公開基盤」とする。
- アーキテクチャ文書公開基盤は、複数のソースリポジトリに配置されたarc42形式のAsciiDoc文書、Structurizr DSLなどの図定義、補助ファイルを収集・変換し、GitHub Pages上の静的ドキュメントサイトとして公開する仕組みである。

## 含むもの
- 各リポジトリ内の文書配置規約
- arc42、AsciiDoc、Structurizr DSLの取り扱い規約
- docToolchain設定
- Antora component / module構成
- Antora playbook
- GitHub Actions workflow
- GitHub Pages公開設定
- 日本語版・英語版文書を扱うための文書構成規約

## 含まないもの
- GitHub本体
- GitHub Actions実行基盤
- GitHub Pagesホスティング基盤
- Antora、docToolchain、Structurizrの本体
- 各ソースリポジトリで開発されるアプリケーション本体
- Codex、ChatGPTなどのAIツール本体
- 閲覧者のブラウザ

## 基本構成
- 各ソースリポジトリは、文書本文、図定義、Antora component設定、docToolchain設定、必要な検査workflowを正本として持つ。
- 各ソースリポジトリは、統合用のAntora playbookを持たない。
- 専用サイトリポジトリの標準名は`zouchikikou-docs-site`とする。
- `zouchikikou-docs-site`は、この決定後すぐ作成する。
- `zouchikikou-docs-site`は、Antora playbook、共通UI、公開workflow、公開workflowで使う共通検査ツールを持つ。
- `zouchikikou-docs-site`は、Antora playbookで複数ソースリポジトリの文書を集約する。
- GitHub Actionsは、検査、図生成、Antora build、GitHub Pages deployを実行する。
- GitHub Pagesは、生成済み静的サイトの公開先として扱う。
