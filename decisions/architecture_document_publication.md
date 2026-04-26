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
- GitHub Actionsで文書検査、Structurizr DSLなどからの図生成、Antora buildを実行する。
- サイト用リポジトリのAntora playbookで複数リポジトリの文書を集約する。
- 生成された静的サイトをGitHub Pagesへdeployする。
- 公開後はGitHub Pages上のページとActions logで結果を確認する。
