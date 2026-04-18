# arc42文書をMarkdown前提からAsciiDocとGitHub Pages前提へ改めた理由

## 変更
- arc42本文の標準形式をMarkdownからAsciiDocへ改めた。
- GitHub上の`README.md`と、公開用の`index.adoc`の役割を分けた。
- 公開表示の標準先をGitHub Pagesにした。

## 理由
- `supporting_data/`にあるarc42のAsciiDocテンプレートと整合する形にした方が、公式構成を流用しやすい。
- AsciiDocのincludeを使う前提なら、`index.adoc`と章ファイルの分割構成が自然である。
- GitHub上ではAsciiDocのinclude結果をそのまま読む前提にしにくいため、READMEは入口、公開本文はGitHub Pagesという役割分離の方が分かりやすい。
