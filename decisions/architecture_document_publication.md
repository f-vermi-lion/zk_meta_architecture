# アーキテクチャ文書の公開方法
- アーキテクチャ文書の公開表示の標準先はGitHub Pagesとする。
- GitHub上の`README.md`は、各文書ディレクトリの入口として残す。
- GitHub上でAsciiDocのinclude解決結果を直接読むことは前提にしない。
- 公開サイトでは、Antoraで生成したHTMLページを見せる。
- `README.md`から対応するGitHub Pages上の公開ページへのリンクを置く。
- 複数リポジトリの文書集約は、Antoraのcontent sourcesを基本にする。
- GitHub Pages上の公開物は派生成果物であり、正本ではない。
- 文書正本は各ソースリポジトリのAsciiDoc、Structurizr DSL、Antora設定、docToolchain設定、GitHub Actions workflowに置く。

## 高レベル自動化手順
- ソースリポジトリで文書、図定義、Antora設定を更新する。
- ソースリポジトリのGitHub Actionsで、必要な文書検査や図生成を実行する。
- サイト用リポジトリのGitHub Actionsで、content sources取得、必要な検査・生成、Antora buildを実行する。
- サイト用リポジトリのAntora playbookで複数リポジトリの文書を集約する。
- 生成された静的サイトをGitHub Pagesへdeployする。
- 公開後はGitHub Pages上のページとActions logで結果を確認する。

## サイト再生成の起動方式
- 標準トリガーは、各ソースリポジトリの`main`更新後に、サイト用リポジトリへ`repository_dispatch`で再生成を依頼する方式にする。
- サイト用リポジトリの公開workflowは、`repository_dispatch`と手動の`workflow_dispatch`を受け付ける。
- `repository_dispatch`で起動した公開workflowは、content sources取得、必要な検査・生成、Antora build、GitHub Pages deployを実行する。
- ソースリポジトリ側のpushは文書正本更新の契機であり、GitHub Pages上の公開成果物を正本化するものではない。
