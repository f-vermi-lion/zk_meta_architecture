## 3\. コンテキストとスコープ

## 3.0 対象システム

本章における対象システムは、仮に **「アーキテクチャ文書公開基盤」** と呼ぶ。

アーキテクチャ文書公開基盤は、複数のソースリポジトリに配置された arc42 形式の AsciiDoc 文書、Structurizr DSL などの図定義、関連する補助ファイルを収集・変換し、GitHub Pages 上の静的ドキュメントサイトとして公開するための仕組みである。

対象システムに含めるものは、主に以下である。

- 各ソースリポジトリ内の文書配置規約
- arc42 / AsciiDoc / Structurizr DSL の取り扱い規約
- docToolchain 設定
- Antora component / module 構成
- Antora playbook
- GitHub Actions workflow
- GitHub Pages へ公開するためのビルド・デプロイ設定
- 日本語版・英語版文書を扱うための文書構成規約

対象システムに含めないものは、以下である。

- GitHub そのもの
- GitHub Actions 実行基盤
- GitHub Pages ホスティング基盤
- Antora / docToolchain / Structurizr などの外部製品本体
- 各ソースリポジトリで開発されるアプリケーション本体
- Codex / ChatGPT などの AI ツール本体
- 閲覧者のブラウザ

arc42 3章は、対象システムを外部の利用者・隣接システムから区切り、外部インターフェースを明らかにする章である。arc42 公式でも、必要に応じて業務コンテキストと技術コンテキストを分けて記述する形式が示されています。 [arc42 Docs](https://docs.arc42.org/section-3/)

---

## 3.1 業務コンテキスト

### 3.1.1 業務上の境界

アーキテクチャ文書公開基盤は、F、AI、リポジトリ貢献者がソースリポジトリ内で文書を更新したときに、公開サイトへ自動反映するための文書公開システムである。

この基盤の主目的は、文書の内容と図の定義をソースコードと同じリポジトリで管理しつつ、閲覧者には統合されたドキュメントサイトとして提供することである。

文書作成者は、原則として以下だけに集中できることを目指す。

- arc42 各章・各節の本文を書く
- Structurizr DSL などで図を書く
- 必要な相互参照を書く
- 日本語版・英語版の文書を更新する
- main ブランチへ反映する

サイト生成、複数リポジトリからの収集、図の変換、静的サイトの公開は、可能な限り自動化される。

---

### 3.1.2 業務コンテキスト図

```markdown
                 ┌──────────────┐
                 │      F       │
                 │ 文書作成・確認 │
                 └──────┬───────┘
                        │
                        │ 文書・図定義を編集
                        ▼
┌──────────────┐   文書改善案・編集支援   ┌──────────────┐
│    Codex     │ ─────────────────────▶ │              │
│ リポジトリ内AI │                       │              │
└──────────────┘                       │              │
                                       │ アーキテクチャ │
┌──────────────┐   文書改善案・設計支援   │ 文書公開基盤 │
│   ChatGPT    │ ─────────────────────▶ │              │
│ 対話型AI支援  │                       │              │
└──────────────┘                       │              │
                                       └──────┬───────┘
┌──────────────┐                              │
│ リポジトリ貢献者 │ ─ 文書・図・コード変更 ───────┘
└──────────────┘
                                               │
                                               │ 統合済み文書サイト
                                               ▼
                                      ┌──────────────┐
                                      │ 文書サイト閲覧者 │
                                      └──────────────┘
```

---

### 3.1.3 外部利用者・隣接システム

| 外部要素 | 入力 | 出力 | 備考 |
| --- | --- | --- | --- |
| F | 文書本文、図定義、レビュー結果、設計判断、公開可否の判断 | 公開サイト、プレビュー、ビルド結果、エラー情報 | 主利用者。日本語で読めることを重視する。 |
| Codex | リポジトリ内の文書・コード・タスク指示 | 文書修正案、英語文書案、構成修正、PR または差分 | ソースと文書が同じリポジトリにあるほど作業しやすい。 |
| ChatGPT | 文書断片、設計相談、構成案、レビュー依頼 | 設計整理、章立て案、文面案、指摘 | リポジトリ外からの構想整理・レビュー支援。 |
| リポジトリ貢献者 | 文書、図、コード、レビューコメント | 統合サイト、文書構成、参照可能な設計情報 | F と AI 以外に将来参加する可能性がある。 |
| 文書サイト閲覧者 | Web ブラウザからの閲覧要求 | GitHub Pages 上の静的ドキュメントサイト | 特定文書への直接リンク、概要から詳細への移動、関連文書への導線を期待する。 |
| ソースリポジトリ | arc42 文書、Structurizr DSL、画像、補助ファイル、Antora 設定 | サイト生成時に読み込まれる文書コンテンツ | 文書の正本を置く場所。 |
| サイト用リポジトリ | Antora playbook、UI、公開 workflow、共通設定 | 統合サイト成果物、Pages デプロイ | 仮定。全体サイト生成の中心。 |
| GitHub | Git push、PR、workflow 実行要求 | リポジトリ状態、Actions 実行環境、Pages 公開先 | GitHub Actions と GitHub Pages は対象システム外の実行・公開基盤。 |

---

### 3.1.4 業務インターフェース

#### 文書作成インターフェース

文書作成者は、各ソースリポジトリ内で arc42 形式の AsciiDoc ファイルを編集する。章または節ごとにファイルを分割し、必要に応じて上位文書から include する。

AsciiDoc の include directive は、大きな文書の分割、ソースコードや外部ファイルの挿入、再利用可能な断片の共有に使えるため、この要求と相性がよいです。 [Asciidoctor Docs](https://docs.asciidoctor.org/asciidoc/latest/directives/include/)

#### 図作成インターフェース

文書作成者は、Structurizr DSL を図の正本として編集する。Structurizr DSL は、C4 モデルに基づくソフトウェアアーキテクチャモデルをテキスト DSL として定義する仕組みです。 [Structurizr](https://docs.structurizr.com/dsl?utm_source=chatgpt.com)

図の最終出力形式は、サイト表示に適した SVG または画像ファイルとする。ただし、CI での変換方式は、Structurizr の PNG/SVG export を使う案と、docToolchain の `exportStructurizr` による PlantUML / C4-PlantUML 経由案を比較・検証する。

#### サイト閲覧インターフェース

閲覧者は、GitHub Pages 上の静的サイトを読む。特定文書への直リンク、関連文書へのリンク、全体概要から詳細文書への移動を行える必要がある。

Antora では、ページ間リンクに `xref` macro と resource ID を使う方式が示されているため、リポジトリ内外のページ参照は Antora の `xref` を基本にする。 [Antora Docs](https://docs.antora.org/antora/latest/page/xref/)

#### 再利用・取り込みインターフェース

共通説明、用語定義、横断的概念、上位文書から参照される章・節などは、Antora の partial または page include として取り込む。

Antora の partial は、AsciiDoc include directive と Antora resource ID によって任意の page または partial に挿入できる。 [Antora Docs](https://docs.antora.org/antora/latest/page/include-a-partial/)

---

## 3.2 技術コンテキスト

### 3.2.1 技術上の境界

アーキテクチャ文書公開基盤は、GitHub 上の複数リポジトリ、GitHub Actions、Antora、docToolchain、Structurizr、GitHub Pages を組み合わせて構成される。

技術的には、以下のような流れを想定する。

1. 各ソースリポジトリに、arc42 / AsciiDoc / Structurizr DSL / Antora component descriptor を配置する。
2. main ブランチへの push を契機に、文書検査・図生成・サイト再生成を実行する。
3. サイト用リポジトリの Antora playbook が、複数の source repository から文書を取得する。
4. Antora が統合サイトを生成する。
5. GitHub Actions が生成済み静的ファイルを GitHub Pages にデプロイする。
6. 閲覧者は GitHub Pages から公開済みサイトを読む。

Antora は、content source repositories として複数の Git リポジトリ、ブランチ、タグ、start path を playbook で指定できる。 [Antora Docs](https://docs.antora.org/antora/latest/content-source-repositories/?utm_source=chatgpt.com)  
また GitHub Pages は、push によるブランチ公開または GitHub Actions workflow による公開を選べる。今回のように Antora などの独自ビルド工程がある場合は GitHub Actions workflow による公開が適する。 [GitHub Docs](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site)

---

### 3.2.2 技術コンテキスト図

```markdown
┌─────────────────────────────┐
│        Source Repository A   │
│  code / arc42 / adoc / DSL   │
│  antora.yml / docToolchain   │
└──────────────┬──────────────┘
               │ push main
               │ repository_dispatch / workflow_dispatch / scheduled build
               ▼
┌─────────────────────────────┐
│        GitHub Actions        │
│  build / validate / export   │
└──────────────┬──────────────┘
               │
               │ checkout / fetch content sources
               ▼
┌─────────────────────────────┐
│       Site / Playbook Repo   │
│  antora-playbook.yml         │
│  UI / common config / CI     │
└──────────────┬──────────────┘
               │
               │ npx antora antora-playbook.yml
               ▼
┌─────────────────────────────┐
│            Antora            │
│  multi-repo AsciiDoc site    │
└──────────────┬──────────────┘
               │ static site
               ▼
┌─────────────────────────────┐
│        GitHub Pages          │
│  published documentation     │
└──────────────┬──────────────┘
               │ HTTPS
               ▼
┌─────────────────────────────┐
│        Web Browser           │
│  F / contributors / viewers  │
└─────────────────────────────┘

補助的な変換経路:

Structurizr DSL
      │
      ├─ Structurizr PNG/SVG export
      │
      └─ docToolchain exportStructurizr → PlantUML / C4-PlantUML → image
```

---

### 3.2.3 外部技術インターフェース

| 隣接システム / ツール | 技術インターフェース | 対象システムとの関係 |
| --- | --- | --- |
| GitHub Repositories | Git, branch, tag, pull request, push event | 文書正本とサイト設定を保存する。 |
| GitHub Actions | YAML workflow, runner, artifacts, repository events | 文書検査、図生成、Antora build、Pages deploy を実行する。 |
| GitHub Pages | 静的サイトホスティング, HTTPS | 生成済みサイトを公開する。GitHub Pages の Actions 公開は、build artifact を upload して deploy する流れになる。 [GitHub Docs](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site) |
| Antora | `antora-playbook.yml`, `antora.yml`, modules, pages, partials, assets | 複数リポジトリの AsciiDoc を統合し、Web サイトを生成する。 |
| docToolchain | `dtcw`, `docToolchainConfig.groovy`, Gradle tasks | arc42 テンプレート、AsciiDoc 生成、図連携、補助成果物生成を担う。docToolchain の Antora 連携は beta と明記されているため、採用時は検証対象にする。 [Doctoolchain](https://doctoolchain.org/docToolchain/v2.0.x/020_tutorial/050_multipleRepositories.html) |
| Structurizr | `.dsl`, CLI/export, PNG/SVG export, PlantUML export | C4 図モデルの正本を管理し、サイト掲載用の図を生成する。 |
| Asciidoctor / AsciiDoc 処理系 | include, xref, attributes, blocks | 文書分割、再利用、相互参照、図や生成物の埋め込みに使う。 |
| Web Browser | HTTPS, HTML, CSS, SVG, JS | 閲覧者が公開サイトを読む。 |
| Codex / ChatGPT | Git 差分、ファイル編集、対話、レビュー | 文書作成・修正を支援するが、対象システムの実行基盤ではない。 |

---

### 3.2.4 主要データ・成果物

| 成果物 | 所在 | 正本か | 説明 |
| --- | --- | --- | --- |
| arc42 章・節 AsciiDoc | 各ソースリポジトリ | はい | 文書本文の正本。章または節単位で分割する。 |
| Structurizr DSL | 各ソースリポジトリ | はい | C4 図モデルの正本。 |
| 生成済み SVG / PNG / PlantUML | build 出力または assets | いいえ | DSL などから生成される派生成果物。 |
| `antora.yml` | 各コンテンツルート | はい | Antora component version の識別・設定。 |
| navigation file | 各 component/module | はい | サイト内ナビゲーションの正本。 |
| `antora-playbook.yml` | サイト用リポジトリ | はい | どのリポジトリ・ブランチ・パスから文書を集めるかを定義する。 |
| GitHub Actions workflow | 各ソースリポジトリまたはサイト用リポジトリ | はい | 検査、生成、デプロイの自動化定義。 |
| GitHub Pages 公開サイト | GitHub Pages | いいえ | 利用者向けの公開結果。正本ではない。 |

---

### 3.2.5 技術的な入出力対応

| 業務入出力 | 技術チャネル | 技術形式 |
| --- | --- | --- |
| 文書本文の更新 | Git push / pull request | `.adoc` |
| 図定義の更新 | Git push / pull request | `.dsl` |
| 文書分割・再利用 | AsciiDoc include / Antora partial include | `include::...[]` |
| ページ間参照 | Antora xref | `xref:...[]` |
| 複数リポジトリ取り込み | Antora content sources | `antora-playbook.yml` |
| 図生成 | Structurizr export / docToolchain task | SVG, PNG, PlantUML, C4-PlantUML |
| サイト生成 | Antora CLI | HTML, CSS, JS, assets |
| サイト公開 | GitHub Actions → GitHub Pages | Pages artifact / static site |
| 手動確認 | ローカルビルド / Actions log / Pages preview | HTML, log, build artifact |

---

## 3.3 スコープ外

以下は本システムの責務外とする。

- 各ソースリポジトリのアプリケーション実装そのもの
- arc42 の章立てそのものの改変
- Antora、docToolchain、Structurizr、GitHub の本体開発
- AI ツールのモデル性能改善
- 文書内容の完全自動生成
- 翻訳品質の完全保証
- GitHub Pages 以外の公開基盤対応
- Confluence、PDF、Word などへの本格出力

ただし、docToolchain が PDF や Confluence などにも対応しうるため、将来拡張としては残す。

---

## 3.4 未決定事項

| ID | 未決定事項 | 影響 |
| --- | --- | --- |
| UC-1 | Antora playbook を専用サイトリポジトリに置くか、全体概要リポジトリに置くか | build trigger、権限、構成の複雑さに影響する。 |
| UC-2 | 各ソースリポジトリの push でサイト再生成をどう起動するか | `repository_dispatch` 、 `workflow_dispatch` 、定期実行、手動実行の選択が必要。 |
| UC-3 | Structurizr DSL から最終的に SVG を直接生成するか、PlantUML / C4-PlantUML 経由にするか | 図の見た目、CI 安定性、依存関係に影響する。 |
| UC-4 | 日本語版・英語版を Antora 上でどう表現するか | component、version、module、別サイト、同一ページ併記の設計に影響する。 |
| UC-5 | private repository の文書を public GitHub Pages に出してよいか | 情報漏洩リスクとアクセス制御に影響する。GitHub Pages は公開サイトになりうるため、公開範囲の確認が必要。 [GitHub Docs](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site) |

---

## 3.5 現時点の推奨仮決定

この段階では、次の前提で進めるのが一番破綻しにくいです。

**対象システムは「各リポジトリ内の文書正本」と「専用サイトリポジトリの Antora 集約・公開設定」からなる文書公開基盤と定義する。**

つまり、ソースリポジトリ側は「文書を書く場所」、サイトリポジトリ側は「集めて公開する場所」とする。

最初の実装候補は以下です。

```markdown
source-repo-a/
  docs/
    antora.yml
    modules/
      ROOT/
        pages/
          index.adoc
          01_introduction_and_goals.adoc
          02_architecture_constraints.adoc
          03_context_and_scope.adoc
        partials/
        assets/
          images/
    structurizr/
      workspace.dsl
  docToolchainConfig.groovy
  dtcw

docs-site/
  antora-playbook.yml
  .github/
    workflows/
      publish.yml
```

この構成なら、arc42 文書の正本を各ソースリポジトリに置きつつ、Antora の複数リポジトリ集約と GitHub Pages 公開に自然につなげられます。