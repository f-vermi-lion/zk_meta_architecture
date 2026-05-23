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
- サイト用リポジトリ`zouchikikou-docs-site`のGitHub Actionsで、content sources取得後にcheckout済みcontent rootを引数として`tools/check-language-pairs <content-root>...`を実行し、必要な検査・生成、Antora buildを実行する。
- `zouchikikou-docs-site`のAntora playbookで複数リポジトリの文書を集約する。
- 生成された静的サイトをGitHub Pagesへdeployする。
- 公開後はGitHub Pages上のページとActions logで結果を確認する。

## 初期build workflow
- `zouchikikou-docs-site`の初期`.github/workflows/build.yml`は、bootstrap用の手動build workflowとして扱う。
- 初期`build.yml`は`workflow_dispatch`のみを受け付け、`npm run build`でAntora buildを確認する。
- 初期`build.yml`は、GitHub Pages deploy、`repository_dispatch`、token / secret利用、`tools/check-language-pairs`実行を含めない。
- GitHub Pages deployを行う公開workflowは、初期build確認後に別途有効化する。

## GitHub Pages deploy有効化手順
- 初期build確認後、`zouchikikou-docs-site`のGitHub Pages publishing sourceはGitHub Actionsにする。
- 初回のPages deploy workflowは`workflow_dispatch`で手動起動する。
- workflowは`npm run build`で生成した`build/site`をPages artifactとしてuploadし、そのartifactをGitHub Pagesへdeployする。
- Pages deploy workflowには、`contents: read`、`pages: write`、`id-token: write`の最小権限を設定する。
- Pages deployでは、GitHub公式の`actions/configure-pages@v5`、`actions/upload-pages-artifact@v4`、`actions/deploy-pages@v4`を使う。
- Antoraが生成するGitHub Pages向けサイトには`.nojekyll`を含め、GitHub PagesがJekyllとして処理しないようにする。
- 初回公開用のworkflowは、`zouchikikou-docs-site`の`.github/workflows/publish.yml`に置く。
- 初回有効化では、`repository_dispatch`、cross-repository token / secret、private repositoryのcontent source、`tools/check-language-pairs`実行はまだ含めない。
- GitHub Pagesで公開されたHTMLは派生成果物であり、正本は引き続き各ソースリポジトリと`zouchikikou-docs-site`の設定に置く。

## 初回公開結果
- 初回公開日は2026-05-23とする。
- 初回公開URLは、https://f-vermi-lion.github.io/zouchikikou-docs-site/meta-architecture/architecture-documentation-publishing-platform/index.html とする。
- 初回確認では、公開ページはローカルbuild結果と大きな差がないことをFが確認した。
- 初回公開後の公開URLと確認結果は、以後のREADME、公開手順、確認手順の参照先として扱う。

## サイト再生成の起動方式
- 標準トリガーは、各ソースリポジトリの`main`更新後に、`zouchikikou-docs-site`へ`repository_dispatch`で再生成を依頼する方式にする。
- `zouchikikou-docs-site`の公開workflowは、`repository_dispatch`と手動の`workflow_dispatch`を受け付ける。
- `repository_dispatch`で起動した公開workflowは、content sources取得後にcheckout済みcontent rootを引数として`tools/check-language-pairs <content-root>...`を実行し、必要な検査・生成、Antora build、GitHub Pages deployを実行する。
- ソースリポジトリ側のpushは文書正本更新の契機であり、GitHub Pages上の公開成果物を正本化するものではない。
