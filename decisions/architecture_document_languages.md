# アーキテクチャ文書の日英運用
- この方針の対象は、造智機巧のアーキテクチャ文書である。
- `meta_architecture` 自体の文書言語は、引き続き日本語とする。
- アーキテクチャ文書の正本は英語版とする。
- 日本語版は補助版として扱う。
- 公開時と版確定時には、日英両版が揃っていることを必須とする。
- 草稿段階では片方のみの更新を許容するが、正本との差分が恒久化しないように管理する。

## Antora上の表現
- 日英版は、同一component内のAntora moduleで分ける。
- 英語正本は`docs/modules/ROOT/`に置く。
- 日本語補助版は`docs/modules/ja/`に置く。
- 日英で対応するページは、module配下の相対パスとファイル名を揃える。
- 英語ページは`:lang: en`、日本語ページは`:lang: ja`を明示する。
- Antoraの`version`やcomponent名を言語差分の表現に使わない。
- 草稿段階では片方のmoduleだけにページが存在してよいが、公開時と版確定時には対応ページを揃える。

## 日英対応検査
- 検査対象は造智機巧のアーキテクチャ文書とする。
- `meta_architecture` 自体は日本語運用のため、日英必須検査の対象外にする。
- 検査単位は、英語正本`docs/modules/ROOT/pages/**/*.adoc`と日本語補助版`docs/modules/ja/pages/**/*.adoc`のmodule配下の相対パスにする。
- 日英対応検査の標準コマンドは、専用サイトリポジトリ内の`tools/check-language-pairs`とする。
- 標準コマンド形式は、`tools/check-language-pairs <content-root>...`とする。
- `<content-root>`には、`antora.yml`を持つAntora content rootを渡す。代表例は、checkout済みの`docs`ディレクトリである。
- 実装言語はPython 3とし、標準ライブラリのみを使う。
- このコマンドは、公開workflowがcontent sourcesを取得した後、Antora buildの前に実行する。
- `ROOT`側のページは`:lang: en`、`ja`側のページは`:lang: ja`を必須にする。
- 草稿段階では片方のmoduleだけにページが存在してよい。
- 公開時と版確定時のCIでは、対応ページの欠落と`:lang:`不一致を失敗扱いにする。
- 検査違反は、`missing_ja`、`missing_en`、`missing_lang`、`lang_mismatch`のいずれかのissue codeを持つ行として出力する。
- 終了コードは、問題なしを`0`、検査違反ありを`1`、引数不正やcontent root不正など実行不能を`2`とする。
- 初期範囲では、翻訳内容の差分、更新鮮度、日英リンクの存在は検査しない。
